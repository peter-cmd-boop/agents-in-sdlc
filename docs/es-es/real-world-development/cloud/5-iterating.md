---
title: "Ejercicio 5: Itera sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Pediste a Copilot que añadiera documentación al código, creara una sección de juegos relacionados para que el equipo de diseño pudiera evaluar la experiencia e implementara funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios y, si hace falta, proporciona comentarios a Copilot para mejorar el resultado.

### Escenario

Como ya se ha señalado varias veces, la IA generativa no cambia los fundamentos del diseño de software ni de DevOps. Siempre debemos revisar el código generado y seguir el proceso habitual de DevOps. Por eso, revisaremos las propuestas de GitHub Copilot para la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

El agente de GitHub Copilot en la nube trabaja de forma asíncrona y sin supervisión directa. Para mantener la seguridad, se han establecido algunas restricciones:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para el código.
- El agente trabaja en GitHub Actions, dentro de un entorno efímero y separado.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- De forma predeterminada, el acceso a recursos externos, incluidos los servidores MCP, está limitado: [Acceso a recursos externos](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall).

## Revisa la documentación generada

Empieza por revisar la primera solicitud de incorporación de cambios (PR) que haya generado el agente de GitHub Copilot en la nube: la que añade documentación al código. Usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR, puede que aparezca un aviso indicando que un firewall ha bloqueado a GitHub Copilot. Es **esperable**, ya que el acceso del agente a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la PR titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot todavía está trabajando en la tarea, la PR incluirá la etiqueta **[WIP]**. Espera a que termine. Puede tardar unos minutos; mientras tanto, puedes descansar o repasar lo aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

   Al revisar los cambios, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas terminado la revisión, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una PR no es una actividad unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otros miembros del equipo. Copilot leerá los comentarios e iniciará otra sesión para resolverlos. Como los resultados son probabilísticos, no existe un texto exacto que debas usar. Por ejemplo, puedes pedir a Copilot que:

- añada encabezados de comentarios al principio de cada archivo de código para describir brevemente su propósito.
- añada comentarios TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar cambios en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia sobre la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta quedar conforme. Después, convierte la PR en lista para revisión y combínala en la rama principal.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título se parezca a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando termines la revisión, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que algunos flujos de trabajo esperan aprobación.
7. Si hay flujos pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. Puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en el Codespace para probar manualmente los juegos relacionados. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ya ha implementado los juegos relacionados. Como antes, puedes solicitar más cambios al agente en la nube. Por ejemplo, puedes pedirle que modifique el número de juegos relacionados o que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para pedir cambios, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas por el agente personalizado. Esta PR debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que pediste desde mission control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título se parezca a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando en la tarea, la PR tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript/TypeScript que controla los cambios de modo.

6. Cuando termines la revisión, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos flujos de trabajo esperan aprobación.
8. Si hay flujos pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro única se completarán correctamente. Puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en el Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Después, inicia la aplicación y prueba los selectores de alto contraste y modo claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al volver a cargar la página.

El agente personalizado de accesibilidad ayudó a Copilot a implementar las funciones siguiendo buenas prácticas. Si observas problemas o mejoras de accesibilidad, puedes mencionarlo en un comentario a **@copilot** y pedirle que los resuelva, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando puedes asignarle elementos reales del backlog y ocuparte de otras tareas. Para adquirir el hábito, crea más incidencias en el repositorio y asígnalas a Copilot. Algunas ideas:

- crear un formulario de interés para patrocinadores en la página de detalles de los juegos.
- implementar paginación en la lista de juegos.
- añadir validación de entrada y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizadas del repositorio** para ver su efecto en los resultados del agente.
- **Asignaste incidencias al agente de GitHub Copilot en la nube** y observaste cómo configuraba su entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como mission control** para supervisar y orientar la sesión de accesibilidad.
- **Revisaste e iteraste sobre las solicitudes de incorporación de cambios del agente**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Repaso y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado aquí:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP en el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de control de calidad personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asigna incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configura el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
