---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma determinada, conviene que Copilot tenga esas indicaciones a mano. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones con ámbito por ruta.
- actualizarás el archivo de instrucciones para asegurar que se siguen los estándares de programación.
- verás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir lo que hace el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices existentes de formato y lint.

Con los archivos de instrucciones, te asegurarás de que Copilot dispone de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que comprenda mejor tu estilo de programación y tus requisitos. Esta función te permite orientar las sugerencias y los fragmentos de código de manera eficaz. Puedes especificar las convenciones de programación que prefieres, las bibliotecas que usas y los comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivo o tareas específicas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto que sea pertinente para la mayoría de las solicitudes enviadas a Copilot Chat o CLI.
- `.github/instructions/*.instructions.md`: archivos que proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La disponibilidad de otros formatos de instrucciones y su compatibilidad varían según el entorno. Antes de confiar en un formato, consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

## Explora los archivos de instrucciones de este proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Explóralos antes de hacer cambios para ver qué efecto tienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y en sus directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
5. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior, que define un patrón glob para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir instrucciones extensas en archivos más pequeños y reutilizables, y dirigir Copilot a ejemplos que pueda seguir.

## Actualiza las instrucciones del equipo

Los archivos existentes son un buen punto de partida, pero falta una directriz. Modifica el archivo principal `copilot-instructions.md` para que el código TypeScript generado incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección titulada **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade esta línea como último elemento de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, sal de la conversación de filtrado y reanúdala para que Copilot cargue las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si aparece el aviso, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.

3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Fíjate en los nuevos comentarios TSDoc y confirma que describen correctamente el código.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI usa el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a tu funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones con ámbito por ruta.
- actualizaste el archivo de instrucciones para asegurar que se siguen los estándares de programación.
- comprobaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para asegurar que el lint y las pruebas se ejecutan de forma coherente.

## Recursos

- [Añade instrucciones personalizadas para GitHub Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
