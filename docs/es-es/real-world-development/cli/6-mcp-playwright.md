---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos visto, programar no consiste solo en escribir código. También necesitamos trabajar con datos y servicios externos, y permitir que Copilot use más automatizaciones. Para ello sirven los servidores MCP: amplían las funciones integradas en CLI y proporcionan a Copilot más herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar los cambios de la interfaz de usuario hay que interactuar con ella. Permitirás que Copilot use el sitio web como lo haría una persona para automatizar aún más los cambios y tener más confianza en que funcionan según lo esperado.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de IA comunicarse con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones por ti.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para administrar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona funciones de automatización del navegador con Playwright.

Hay muchos otros servidores MCP. GitHub hospeda un [registro de MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la siguiente sección.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe este comando para abrir el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor es **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes comprobaste manualmente que la funcionalidad se comportaba como esperabas. Ahora usa el servidor MCP de Playwright disponible en Copilot para hacer lo mismo.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No hace falta pedir expresamente a Copilot que use el servidor MCP, ya que normalmente lo determina por sí mismo. Sin embargo, no está de más indicar cuál debe utilizar. Así obtendrás resultados más coherentes y ahorrarás tokens.

2. Observa los pasos que realiza Copilot en el navegador para confirmar que la funcionalidad funciona.
3. Lee el informe y comprueba que todo se comporta como esperas.

Copilot iniciará el servidor, usará Playwright para interactuar con el sitio web, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

Has usado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI. En resumen:

- aprendiste qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadiste el servidor MCP de Playwright si todavía no estaba disponible.
- pediste al agente que controlara un navegador y explorara la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añade servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
