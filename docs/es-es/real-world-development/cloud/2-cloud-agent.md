---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que siguen en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede realizar tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código.
- asegurarte de que las operaciones se realicen de forma segura.
- crear incidencias con un ámbito bien definido.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver varios problemas de deuda técnica. La empresa que creó la primera versión del sitio dejó la documentación en un estado deficiente: de hecho, no hay documentación. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego, para que los visitantes puedan seguir explorando. Por ahora no hace falta una implementación pulida; quieren algo que les permita probar y aceptar la experiencia de usuario (UX). El trabajo está bloqueado, pero otras incidencias tienen mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente y son buenas candidatas para asignarlas al agente de GitHub Copilot en la nube. El agente trabaja de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, cuando termina, puede revisar su trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Hay varias maneras de asignarle tareas, como [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se le asigna una incidencia, Copilot crea un borrador de solicitud de incorporación de cambios para mostrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud.

## La importancia de definir bien el ámbito de las instrucciones

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden ser complejas al examinarlas detenidamente. Por eso, [define cuidadosamente el ámbito de las tareas del agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores se presentaron las instrucciones personalizadas y su función. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa en modo de solo lectura los archivos de instrucciones que incluye el repositorio y observa después su efecto.

Abre estos archivos en la interfaz web de GitHub o en el Codespace, si ya tienes uno en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica las instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, comprueba si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones; volveremos a señalarlo al revisar las PR en otro ejercicio.

## Configura el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y scripts de configuración para asegurarse de que todo está listo. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Este flujo se ha configurado de antemano para el taller y proporciona Node.js, las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para estos pasos. Copilot recibirá un token propio para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de Node.js para el frontend y la aplicación (ahora toda la aplicación usa Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos puntos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Es útil para probarlo sin tener que esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica reduce la productividad, dificulta el mantenimiento y complica la incorporación de nuevos desarrolladores. Copilot puede ayudar a generar documentación, por lo que esta es una buena tarea para asignar al agente en la nube. Trabajará en segundo plano y revisarás el resultado en un ejercicio posterior.

1. En una pestaña nueva del navegador, abre tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios (PR), cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está configurando su entorno mediante el flujo de trabajo anterior, analizando el proyecto y decidiendo cómo abordar el problema.

13. Revisa la lista y las tareas que Copilot va a completar.
14. Desplázate por la cronología de la PR. Deberías ver un aviso de que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo: Copilot explora el código y su estado, se detiene a pensar y elegir un plan y, después, crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube es trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecha esa función asignándole otra tarea y, mientras trabaja, añade una funcionalidad a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube es que permite dividir el trabajo: tú puedes ocuparte de unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, podrías usarlo para otras tareas. Asígnasela al agente en la nube.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás varios emoji 👀 en el primer comentario de la incidencia, lo que indica que Copilot ya está trabajando.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. El agente en la nube trabaja de forma parecida a un ingeniero de software, así que no tienes que supervisarlo continuamente; puedes revisar el resultado cuando termine. Ahora puedes crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Con él puedes asignar incidencias a Copilot para que trabaje de forma asíncrona. Puedes usarlo para resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has aprendido a:

- personalizar el entorno para generar código.
- asegurarte de que las operaciones se realicen de forma segura.
- crear incidencias con un ámbito bien definido.
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, puedes crear y usar agentes personalizados. El [agente de Copilot en la nube también puede usar servidores MCP][cloud-agent-mcp] e instrucciones personalizadas, que vimos en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
