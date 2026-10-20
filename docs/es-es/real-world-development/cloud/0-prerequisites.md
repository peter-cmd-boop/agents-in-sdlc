---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente de GitHub Copilot en la nube, tienes que dejarlo todo preparado. Crearás tu propia copia del repositorio Tailspin Toys y pondrás en marcha un [Codespace][codespaces] para editar los archivos de instrucciones y revisar el trabajo del agente en la nube.

## Configura el repositorio del taller

Para crear una copia del repositorio en la que trabajar, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios para el taller.

1. En una ventana nueva del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. En la página del repositorio del taller, selecciona **Use this template** para crear tu propia copia del repositorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones del equipo mentor. Si no es así, puedes crear el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**nombre-de-organización-o-usuario/nombre-del-repositorio**), ya que la necesitarás más adelante en el taller.

## Crea un Codespace

Ahora usarás un Codespace para completar los ejercicios del taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que te permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un IDE completo compatible con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Selecciona el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

    ![Crea un Codespace](../../../_images/ex0-create-codespace.png)

La creación del Codespace tardará varios minutos, aunque seguirá siendo mucho más rápida que instalar manualmente todos los servicios. Mientras esperas, puedes explorar otras funciones de GitHub Copilot, que veremos a continuación.

> [!CAUTION]
> Volverás al Codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un Codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos entornos incluyen todos los requisitos previos necesarios para trabajar sin problemas. Si prefieres trabajar en local, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que usa Codespaces.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del taller y has iniciado el proceso de creación del Codespace que usarás para trabajar con el agente de GitHub Copilot en la nube.

## Siguiente paso

Añade instrucciones personalizadas que seguirá el agente en la nube. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crea un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
