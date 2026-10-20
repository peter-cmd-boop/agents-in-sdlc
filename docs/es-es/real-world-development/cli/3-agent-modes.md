---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo de trabajo que ya utiliza una organización y ayudar a desarrollar lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, por último, la combinarás en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante todo el flujo de trabajo de la funcionalidad. Normalmente trabajarías con ramas o solicitudes de incorporación de cambios distintas para los diferentes tipos de archivos, pero en este taller seguiremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad nueva con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

A medida que avances con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de control de calidad y abrirás la solicitud de incorporación de cambios de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar detalles como la combinación de categorías antes de programarla. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un flujo parecido a este:

1. Partir de una incidencia creada que describa lo que hay que hacer.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la funcionalidad nueva.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se haya completado correctamente la integración continua, combinar los cambios.

> [!NOTE]
> Los detalles exactos varían según el equipo y la organización, pero la mayoría de los flujos siguen un proceso similar al descrito.

Si sigues este enfoque estándar, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito a mano.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis conjuntamente. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la solicitud de incorporación de cambios de la valoración con estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y, después, crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El carácter `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir cómo implementar una funcionalidad o realizar una tarea antes de asignársela a Copilot. Para cualquier tarea compleja, conviene dedicar tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree un plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Comprueba que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empieza a elaborar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede hacerte preguntas sobre cómo debería funcionar el filtrado. Respóndelas según tus preferencias: no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte del plan que no se ajuste a tus decisiones.

## Aprueba Autopilot

Ya has creado y revisado el plan; ahora puedes implementarlo. Deja que Copilot haga el trabajo con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de la versión instalada.
2. Comprueba que el indicador de modo debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede comenzar inmediatamente. Revísalo antes de aprobarlo. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y valida la implementación

Como cualquier otro código, el código generado debe revisarse antes de combinarlo. Revisa el código y ejecuta el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Comprueba que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste durante la planificación.
4. Cuando termines la revisión del código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los errores.

## Prueba la funcionalidad nueva

El código parece correcto, pero ¿funciona? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones que acordaste en el plan.
4. Comprueba que el restablecimiento y el comportamiento cuando no hay resultados coinciden con la incidencia y con tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- iniciaste una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definiste los requisitos en el modo Plan.
- implementaste la funcionalidad nueva con Autopilot.
- revisaste el código.
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que sigue las prácticas documentadas al [usar instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
