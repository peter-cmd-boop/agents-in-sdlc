---
title: "Lección 10 - Repaso y siguientes pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo completo en Tailspin Toys. En concreto:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y encontraste la incidencia de filtrado ya creada.
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- partiste de la incidencia de filtrado, definiste el enfoque en el modo Plan, lo implementaste en el modo Autopilot y lo revisaste en el modo Interactive.
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- revisaste todos los cambios del filtrado y autorizaste Agent Merge para la PR de esta funcionalidad.
- exploraste comandos de barra para el contexto, los modelos, las sesiones compartidas y la delegación opcional a la nube.

## Lo que has publicado

El taller tiene dos hitos de PR, cada uno en una rama propia creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito para los juegos sin valoración en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, usaste la misma conversación y rama. Para agilizar el taller, combinamos ese trabajo en una sola PR.

## Diferentes tipos de validación

Has comprobado el código de varias maneras: con pruebas automatizadas, inspeccionando el sitio en el navegador y pidiendo a Copilot que explorara el navegador mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y presentó los resultados con el nuevo formato. El agente de control de calidad reunió esos resultados con la revisión de los requisitos y de la cobertura de pruebas antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA no necesita pruebas adicionales, también puede ser correcto. Antes de autorizar la combinación, revisa el código y las pruebas, y actualiza las pruebas de validación afectadas cuando haya cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller, actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados especifican roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para participar en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para cambios rutinarios y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar con claridad *qué* quieres crear, *por qué* y *cómo* cambia significativamente el resultado.

## Más funciones

Ya has completado el flujo de trabajo principal. También puedes explorar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para analizar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar una tarea aparte.
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Siguientes pasos

La mejor manera de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo desarrollar. Comparte lo que aprendas con tu equipo y aprende de los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de GitHub Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personaliza Copilot CLI][customize-cli]
- [Gestiona solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
