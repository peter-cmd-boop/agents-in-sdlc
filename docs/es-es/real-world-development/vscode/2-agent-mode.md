---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos en una aplicación suelen requerir actualizaciones en varios archivos y tareas como ejecutar pruebas. Normalmente, el flujo de trabajo de desarrollo implica localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, actualizarlo y volver a empezar. La lista continúa.

El modo agente de Copilot te ayuda con estas tareas.

El modo agente de Copilot puede trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, después realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, con ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona como otro miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo agente de Copilot para explorar el proyecto, identificar archivos pertinentes y coordinar cambios.
- implementar funcionalidades en la interfaz de usuario y la capa de datos con el modo agente de GitHub Copilot.
- revisar los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste una función auxiliar para las editoriales. Ahora terminarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para conocer las funciones actuales.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con una temática de desarrollo de software. Permite consultar juegos y ver sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter mediante Drizzle ORM; no hay una API de backend separada ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos están en `src/lib/` y la interactividad se implementa con un pequeño `<script>` de Astro que usa las API estándar del DOM.

### Inicia el sitio web

Para facilitar el inicio del sitio, se ha configurado la tarea `npm run dev`, que ejecuta el único servidor de desarrollo de Astro. Para ejecutarla en GitHub Codespaces:

1. Vuelve a tu Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el Codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un aviso con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un Codespace, al seleccionar el enlace de la URL localhost en el terminal, se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora hospeda el servidor web.

### Explora el sitio web

Con el sitio en marcha, explora sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección del modo y del modelo varía según el ejercicio; cada paso lo indica cuando corresponde.

La versión inicial del sitio web funciona, pero queremos añadir funciones. Empecemos por revisar el backlog. Al crear el proyecto desde la plantilla, se creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esas incidencias.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. Este agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo u otro. Durante su elaboración se usó Claude Sonnet 4.5, por lo que se incluye en las instrucciones con la esperanza de obtener sugerencias relativamente coherentes y ofrecer una experiencia uniforme. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de los ejemplos del taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede usar un comando MCP distinto, pero debería completar la tarea igualmente.

3. Envía a Copilot este prompt para que revise las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado las incidencias según las que considera más útiles para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le proporcionará contexto. Usarás el archivo de instrucciones de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones, entre ellas:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para distintos tipos de archivo, incluidos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar instrucciones, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, habrá que hacer al menos tres cambios en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de listado de juegos para añadir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código base. El modo agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o inicia otra con **New Chat**.
2. Selecciona **Add Context**, **Instructions** y `ui` como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes para la funcionalidad. Debería encontrar las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código base. Mientras tanto, puede que aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, habrá un botón **Keep** o **Undo** para cada cambio; no hace falta seleccionar ninguno mientras el agente trabaja.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador donde está abierto el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has aprendido a usar el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. En concreto:

- usaste el modo agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos.
- exploraste el proyecto con el modo agente, identificaste archivos pertinentes y coordinaste cambios.
- revisaste los cambios y las pruebas generados por el modo agente antes de combinarlos en el código base.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que añada paginación al sitio. Al preparar el prompt:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o será fijo.
- proporciona a Copilot las directrices necesarias para obtener la implementación que deseas.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
