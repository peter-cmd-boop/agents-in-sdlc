---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye numerosos comandos de barra con los que puedes interactuar. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Explora algunos otros que también te pueden resultar útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás a exportar o compartir una sesión con `/share`.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has completado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente en el [agente de GitHub Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes acercarte al límite de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando lo necesita; también puedes inspeccionar o compactar el contexto manualmente con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Puede que no haya cambiado mucho si la conversación ya era corta.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando la ventana se llena. Usa `/compact` si quieres decidir cuándo compactarlo. Usa `/clear` o `/new` cuando cambies a una tarea sin relación y quieras empezar una conversación nueva.

## Elige un modelo

Cada modelo tiene sus puntos fuertes y cada desarrollador sus preferencias. Copilot CLI permite mostrar los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar y compartir lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` permite exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicarla como un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir esta sesión, elige el destino que se ajuste a tus necesidades, por ejemplo, `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que tu equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

Has completado el taller principal. Copilot CLI también incluye comandos para tareas de mayor alcance:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea aparte.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Empieza por una tarea nueva y bien delimitada, y revisa el resultado con el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar lo que ocurre internamente. En esta lección:

- usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usaste `/model` para explorar los modelos disponibles.
- aprendiste a exportar o compartir una sesión con `/share`.
- exploraste los comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI incluye más comandos de barra y todavía hay mucho por explorar. Para terminar, [repasa lo que has aprendido][next-lesson] y descubre cómo seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de GitHub Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
