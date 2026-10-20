---
title: "Lección 1 - Instala GitHub Copilot CLI"
description: "Instala y autentícate en Copilot CLI en tu Codespace, familiarízate con la herramienta y encuentra la incidencia de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Te permite delegar tareas, solicitar cambios y mantener la concentración. El primer paso, como puedes imaginar, es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación rápida.
- encontrarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a usar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde muchos desarrolladores ya trabajan habitualmente. En esta lección lo instalarás, te autenticarás y lo dejarás listo para usar durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet y Homebrew. Como GitHub Codespaces incluye Node.js preinstalado, usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI de forma global:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Deberías ver el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si aparece el aviso, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI muestra el siguiente aviso:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde al repositorio de Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace, es posible que ya estés autenticado con tu sesión de GitHub. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está todo listo.

## Familiarízate con la herramienta

Los comandos que escribes en el símbolo del sistema normal se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las teclas de flecha para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y, después, pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla a Copilot para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de contestar.
5. Escribe `/mcp list` y confirma que el servidor MCP integrado de GitHub está disponible.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de hacer un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que está seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el atajo del taller

Copilot CLI suele pedir permiso antes de usar herramientas que no estén incluidas en los permisos establecidos. En este taller, lo volverás a iniciar con `--yolo`, un atajo aprobado por el usuario que elimina esos avisos de aprobación dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita todos los permisos automáticos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y no lo conviertas en un alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de GitHub de lectura y escritura que se usarán en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu ordenador local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, usarás `copilot --resume` para volver a la misma conversación de funcionalidad y a la misma rama.

## Resumen y siguientes pasos

En esta lección:

- instalaste GitHub Copilot CLI con npm.
- te autenticaste con tu cuenta de GitHub.
- confiaste en el repositorio del taller y probaste una conversación rápida.
- encontraste la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [empezarás tu primer cambio específico][next-lesson] y usarás Copilot CLI para mostrar una valoración con estrellas en las tarjetas de los juegos.

## Recursos

- [Instala GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
