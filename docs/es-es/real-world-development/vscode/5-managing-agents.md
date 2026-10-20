---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas GitHub Copilot en modo agente, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como ese trabajo se realiza en tu máquina y en tiempo real, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición de archivo a medida que ocurren. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios individuales y orientar la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- orientarás al agente durante la sesión para añadir un selector de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha pedido también un selector de modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de empezar otra conversación desde cero, orientarás la sesión actual para ampliar la funcionalidad.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas. Aparecerán lecturas y ediciones de archivos y los comandos de terminal que necesiten ejecutarse.
3. Cuando el agente proponga una edición, las diferencias aparecerán en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertir el cambio y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para consultar todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará la opción **Allow**. Autoriza únicamente los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al acabar, mostrará un mensaje que resume los cambios.

## Pide al agente que añada el modo claro

Ahora que está listo el modo de alto contraste, amplía la misma conversación para añadir un selector de modo claro. Como continuarás el mismo chat, el agente conservará el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilos y el uso del almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente continuará donde lo dejó, revisará los cambios existentes y propondrá las modificaciones para añadir el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los selectores de modo de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviarle una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos selectores»).

## Revisa los archivos modificados

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando termines la revisión, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso. Revisaste cada edición a medida que aparecía, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- supervisaste el trabajo del agente en tiempo real.
- aceptaste y rechazaste ediciones individuales.
- orientaste una sesión en curso con prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios que haya producido el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
