---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo el contexto del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito por ruta.
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no ejecutarás un prompt antes y después. El agente de GitHub Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar su impacto en tiempo real. Más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente, verás la influencia del archivo de instrucciones.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

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
> Al trabajar en un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las sugerencias de finalización de código ni las de siguiente edición.
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

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones del repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo. Observa la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas directrices se aplican a todas las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior, que define un patrón glob relativo a la raíz del repositorio. En este caso, coincide con los archivos de prueba TypeScript, como los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones extensos en archivos más pequeños y reutilizables, y dirigir Copilot a ejemplos que puede seguir. Las rutas se resuelven desde el archivo de instrucciones, no desde la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación, pero aún no exige documentación en el código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de archivo.

## Añade un estándar para todo el repositorio

Ahora editarás el único archivo del ejercicio: añadirás un estándar general para que la documentación del código use comentarios TSDoc y encabezados de comentarios a nivel de archivo. El agente en la nube usará estas directrices en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero confirmarás los cambios de todos modos:

1. En el terminal del Codespace, crea una rama y cambia a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se explicó antes, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Documenta los estándares para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de programación, pero aún no incluye instrucciones para documentar el código.
3. Añade estas líneas de Markdown debajo de los estándares actuales para pedir a Copilot que incluya encabezados de comentarios por archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones a nivel de repositorio (`.github/copilot-instructions.md`) para directrices generales, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas específicas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios de documentación que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, asignarás una incidencia a Copilot, que creará una rama a partir de `main`. Para que el agente lea las instrucciones, los cambios deben llegar a `main`.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` hacia `main` y combínala. El agente en la nube leerá las instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama refleja cómo se trabaja en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones de este proyecto y has añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`. Durante el resto del entorno verás ese estándar en las solicitudes de incorporación de cambios que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
