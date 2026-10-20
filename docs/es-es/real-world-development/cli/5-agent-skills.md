---
title: "Lección 5 - Personaliza y usa la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya hemos validado manualmente que el código funciona y hemos usado archivos de instrucciones para asegurarnos de que cumple nuestros estándares. Pero ¿qué pasa con las pruebas, el lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la mejor opción. Las habilidades ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo incluye frontmatter con un nombre y una descripción que definen la habilidad, además de una descripción general y directrices sobre cuándo debe usarse. La carpeta también puede contener subcarpetas con scripts y otros recursos para la habilidad.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` del proyecto para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que el equipo de Tailspin Toys ha creado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo debe usar la habilidad.
3. Lee las instrucciones y fíjate en cómo guían a Copilot durante las pruebas y el lint.
4. Observa que la habilidad todavía no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de hacer cambios

Puedes llamar a las habilidades directamente desde Copilot CLI o pedirlas en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personaliza el informe

El informe debería indicar qué se ejecutó, si tuvo éxito y qué resultados proporcionaron las herramientas. Actualiza la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la habilidad actualizada

Ahora comprueba el resultado del cambio. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de sus resultados.
- volviste a cargar y ejecutaste la habilidad.

Ese cambio acompañará al filtrado en la solicitud de incorporación de cambios de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven como referencia, no como tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
