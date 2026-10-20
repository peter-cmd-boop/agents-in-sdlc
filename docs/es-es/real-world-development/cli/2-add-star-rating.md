---
title: "Lección 2 - Añade valoraciones con estrellas: un cambio rápido"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de los juegos, revisarlo en un navegador reenviado y combinarlo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, puedes hacer tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página principal todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación específica en una rama de funcionalidad.
- pedirás al agente que haga un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para comprobar el cambio en un navegador reenviado.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página principal solo muestran el título, la categoría, la editorial y la descripción. Como calentamiento, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git seleccionados en el terminal, en lugar de crear un árbol de trabajo dedicado. Las conversaciones guardadas te permiten volver a la misma conversación más adelante, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas que usa el agente para explorar y editar archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no sigue abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está iniciado, ejecuta este comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa las diferencias

Debes revisar todos los cambios generados por IA antes de combinarlos, aunque sean pequeños. Inspecciona los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta del juego muestra la valoración numérica cuando está disponible y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos estados.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla de diferencias.

> [!NOTE]
> Como todas las herramientas de IA generativa, Copilot es probabilístico y no determinista, por lo que el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación concreta.

## Comprueba los cambios

No basta con leer el código y suponer que funciona. Pide a Copilot que inicie el sitio web para poder revisar la interfaz de usuario (IU) actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de los juegos muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora crea una solicitud de incorporación de cambios para incorporar el código nuevo al proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI publica la confirmación existente al crear la solicitud y muestra su URL.
3. Abre la solicitud manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando todo esté listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación específica en una rama de funcionalidad.
- pediste al agente que hiciera un pequeño cambio en las tarjetas de los juegos.
- revisaste el cambio con `/diff`.
- ejecutaste la aplicación para comprobar la valoración con estrellas en un navegador reenviado.
- abriste y combinaste tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
