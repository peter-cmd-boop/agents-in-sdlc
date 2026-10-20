---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o áreas concretas del flujo de trabajo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, buenas prácticas, patrones de programación y conocimientos del dominio para orientar a Copilot y ayudarle a realizar determinados tipos de trabajo con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con al menos los campos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar determinadas tareas. La forma más sencilla de distinguirlos es esta: un **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes para completar su trabajo. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad. Como parte de su trabajo, podría usar habilidades como una para gestionar solicitudes de incorporación de cambios u otra para ejecutar y administrar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba y ajusta el agente para encontrar lo que mejor se adapte a tu entorno y a tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función cuanto antes. Usarás un agente personalizado para generarla.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace, abre un terminal y crea una rama nueva a partir de `main` para trabajar en la accesibilidad. Así mantendrás separado el trabajo del filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que presentan:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas frecuentes.

## Usa el agente personalizado en Copilot Chat

La vista Copilot Chat de VS Code muestra en el menú desplegable de agentes todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para que sus instrucciones y herramientas se apliquen a la sesión de chat.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección del modo y del modelo varía según el ejercicio; cada paso lo indica cuando corresponde.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** si todavía no está seleccionado.

   ![Captura de pantalla del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes situado en la parte inferior de la vista de chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes.
4. Envía este prompt al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat junto con la ruta del archivo y las diferencias para que puedas revisarlas.
6. A medida que se apliquen los cambios, se actualizará el indicador **Files changed** para que puedas ver todos los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot está haciendo cambios reales en el repositorio: editará archivos existentes, como componentes de Astro, CSS y pruebas relacionadas.

Revisarás y orientarás este trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y áreas concretas. Con ellos, puedes convertir los conocimientos y los estándares del equipo en agentes reutilizables que orienten a Copilot.

En concreto:

- aprendiste cómo se definen los agentes personalizados.
- usaste un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se apliquen y añadirás un selector de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crea agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba tu funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
