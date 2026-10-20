---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente de GitHub Copilot en la nube que se ocupara de tres tareas para mejorar la experiencia de usuario y añadir funciones. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar estas tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas, explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para pedir otra función.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Ya se está actualizando el estilo del sitio y añadiendo controles para cambiar de modo, así que parece lógico incluirlo. Orientarás al agente para que añada tanto el modo claro como el modo de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Consulta el estado actual de las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa los pasos completados por Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar las distintas tareas necesarias para completar el trabajo asignado.

> [!NOTE]
> Los pasos que se muestren variarán según el estado del trabajo de Copilot y el enfoque elegido.

4. Observa también el panel de solicitudes de incorporación de cambios, situado a la derecha. Permite ver la PR y los archivos modificados mientras supervisas el trabajo.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada el selector de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión para añadir el modo de alto contraste. El título exacto depende del nombre que use Copilot y del estado actual del trabajo.

   ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes similares a los ejemplos.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de la tarea del agente en la página de agentes, con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo reconoce y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot seguirá trabajando en la tarea actualizada. Incorporará la nueva petición al flujo de trabajo cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso también tardará varios minutos. Puedes aprovechar la espera para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Desde aquí puedes hacer seguimiento de todas las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para cambiar su ámbito o proporcionarle directrices adicionales.

Has aprendido a:

- usar mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para pedir otra función.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
