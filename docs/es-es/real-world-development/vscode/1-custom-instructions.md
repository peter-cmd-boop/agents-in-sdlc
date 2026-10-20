---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Hay varias maneras de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo el contexto del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito por ruta.
- generarás el primer componente de datos para el filtrado —una función auxiliar para editoriales— con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- repetirás el mismo prompt y observarás cómo el código generado adopta el nuevo estándar.
- confirmarás la actualización de las instrucciones y el componente de filtrado en `main` para que Copilot pueda usar las nuevas directrices en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares indicados. Copilot es probabilístico. El objetivo de este ejercicio es observar la tendencia del cambio de comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe incluir pruebas unitarias.
- La interfaz de usuario debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- La parte superior de cada archivo debe incluir un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones, te asegurarás de que Copilot disponga de la información necesaria para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para ayudarle a entender mejor tu estilo de programación y tus requisitos. Esta función te permite orientar las sugerencias y los fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos y tareas específicas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de Copilot Chat o CLI: por ejemplo, la pila tecnológica, una descripción general del proyecto, buenas prácticas y otras directrices generales.
- `.github/instructions/*.instructions.md`: archivos que pueden proporcionar directrices para lenguajes concretos, como TypeScript, o para tareas como crear componentes de interfaz de usuario o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni las de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de GitHub Copilot en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también [admite otros estándares de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para administrar archivos de instrucciones

Crear archivos de instrucciones completos va más allá del alcance de este taller. Sin embargo, el proyecto de ejemplo muestra un enfoque representativo. En general:

- Limita las instrucciones de `copilot-instructions.md` a directrices generales del proyecto, como la descripción del trabajo, la estructura y los estándares de programación globales.
- Usa `*.instructions.md` para instrucciones concretas según el tipo de archivo —pruebas unitarias, componentes de Astro o la capa de datos, por ejemplo— o para tareas específicas.
- Escribe las instrucciones con un lenguaje claro y natural, e incluye ejemplos del aspecto que debe y no debe tener el código.

No existe una única forma correcta de crear archivos de instrucciones ni de usar la IA. Descubre qué funciona mejor para el proyecto mediante la experimentación.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían incluir un conjunto completo de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá observes que hay instrucciones para muchos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta opción de forma distinta: por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. Las lecciones del entorno correspondiente lo indican cuando procede.
>
> Para consultar plantillas o ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

El repositorio incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa también las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, coincide con los archivos de prueba TypeScript, como los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir instrucciones extensas en archivos más pequeños y reutilizables, y dirigir Copilot a ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación, pero aún no exige documentación en el código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Crea una rama para los cambios

Aplica buenas prácticas y crea una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cambia a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección del modo y del modelo varía según el ejercicio; cada paso lo indica cuando corresponde.

Para observar el efecto de las instrucciones personalizadas, primero envía un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas del editor para que Copilot solo use el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios en el código, a menudo tanto en el archivo de la función auxiliar como en sus pruebas.
6. Observa que la función propuesta usa tipos, recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Estas características proceden de las convenciones de la capa de datos definidas en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Fíjate en que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentarios a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, es posible que añada comentarios de documentación incluso sin indicárselo. No pasa nada: el objetivo es observar la mejora de la coherencia tras actualizar las instrucciones.

## Añade un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade un estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares, pero aún no incluye instrucciones para documentar el código.
3. Añade estas líneas de Markdown debajo de los estándares actuales:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones a nivel del repositorio (`.github/copilot-instructions.md`) para directrices generales, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas específicas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto de la conversación.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc similar a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has indicado a Copilot que siga un nuevo estándar del proyecto y lo aplique a código real que servirá de base para el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se administran con el mismo sistema de control de versiones que los demás archivos. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que la actualización de las instrucciones y los cambios de la función auxiliar estén presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de las instrucciones y todos los archivos que Copilot haya modificado para crear la base de la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

La rama ya está publicada. Ahora crea una solicitud de incorporación de cambios asociada a la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear una PR que la cierre y combinarla mediante las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> En este ejercicio Copilot interactuará con GitHub por primera vez. La plantilla del proyecto ya configura las herramientas de GitHub, así que no tienes que hacer nada. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre la actualización de las instrucciones y que cree una PR desde la rama actual. Indica que la PR debe describir tanto la actualización de las instrucciones como la base de la función auxiliar de editoriales:

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si procede.
6. Cuando se cree la PR, pide a Copilot que la combine y vuelva a `main`:

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide permiso para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *actuales*.
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- repetir el prompt y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar la actualización de las instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda aprovecharlas.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson], cuando Copilot añada una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
