---
title: "Lección 7 - Crea y usa un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora combinarás esas funciones en un agente personalizado con un proceso de control de calidad claramente definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de control de calidad y revisarás sus conclusiones según la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de las pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar este proceso de control de calidad y ofrecer un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás un rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen distintas funciones. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas del navegador. El perfil de control de calidad indica a Copilot cómo usar esas funciones para evaluar los requisitos e informar de los resultados. No las sustituye ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil reutilizable de control de calidad. El perfil definirá las comprobaciones y los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha incluido el flujo de control de calidad y los límites previstos. Así evitarás que un agente incompleto o demasiado amplio cambie la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. Se requiere `description`; `name` es opcional, pero incluirlo proporciona al agente un nombre claro para mostrar.
3. Lee las instrucciones del perfil y confirma que el agente de control de calidad parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa Playwright MCP.
4. Confirma que el agente presenta pruebas que respaldan sus conclusiones, pregunta antes de cambiar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si el perfil generado no incluye alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta el control de calidad según la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona el agente de control de calidad para que use la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista de agentes disponibles. Sin embargo, el comando anterior lo activará.

2. Pide al agente que revise la funcionalidad con este prompt:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su resultado. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creaste y revisaste un perfil reutilizable de control de calidad.
- seleccionaste el agente y revisaste sus conclusiones según la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil del agente de control de calidad, las pruebas y el informe de validación, todo listo para su revisión. A continuación, [reunirás estos elementos en una PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crea agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
