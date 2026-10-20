---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de control de calidad y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la actualización de la habilidad, el perfil de control de calidad y las pruebas están guardados en una misma rama. Ahora toca revisarlos conjuntamente y abrir una solicitud de incorporación de cambios. Combinaste tú mismo la solicitud de incorporación de cambios (PR) de la valoración con estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, dividiríamos la funcionalidad, las instrucciones actualizadas, la actualización de la habilidad y el agente de control de calidad en varias PR. Para agilizar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de validación.
- crearás la PR del filtrado.
- habilitarás Agent Merge solo después de revisar la PR y confirmarás que se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y validar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener el control de la autorización para combinarla en manos del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que hayan fallado, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando sea necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora, seleccionabas **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Deja que Agent Merge gestione la PR

Ya has creado el código. Ahora revísalo, crea la PR y deja que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones terminen correctamente, combinará la PR.
7. Comprueba que Copilot muestra un mensaje similar a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si el proceso se bloquea, lee el motivo indicado y, si el repositorio lo permite, completa manualmente la combinación después de revisar los cambios.

## Resumen y siguientes pasos

Has automatizado varios pasos del desarrollo: generar código, probarlo y validarlo, y ahora gestionar el proceso de la solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de validación.
- creaste la PR del filtrado.
- habilitaste Agent Merge solo después de revisar la PR y confirmaste que se había combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para trabajar con el contexto, los modelos, las sesiones compartidas y la delegación opcional a la nube.

## Recursos

- [Gestiona las solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
