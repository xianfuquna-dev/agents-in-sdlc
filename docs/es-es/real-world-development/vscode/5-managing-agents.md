---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos.
- orientarás la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, pensado para quienes consideran demasiado oscuro el tema predeterminado. En lugar de empezar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver sus llamadas a herramientas, lecturas y ediciones de archivos y comandos del terminal.
3. Cuando proponga una edición:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que lo intente otra vez.
4. También puedes abrir **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso, por ejemplo para ejecutar un comando de terminal, el agente se detendrá y mostrará **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que termine el modo de alto contraste. Aparecerá un mensaje final con un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora amplía la misma conversación para añadir un control de modo claro. El agente conserva el contexto de su trabajo anterior, incluidos los componentes, los estilos y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente retomará el trabajo, revisará los cambios existentes y propondrá los cambios para el modo claro.
3. Revisa cada edición nueva y selecciona **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar los controles de forma no deseada—, deténlo con **Stop** junto al cuadro de chat y envía una aclaración.

> [!TIP]
> Orientar una sesión activa es más rápido que empezar de nuevo. Pide ajustes como «simplifica el CSS y usa variables CSS en vez de duplicar selectores», corrige el rumbo («no añadas un tercer control; integra el modo claro en el panel existente») o amplía el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa los archivos modificados.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión activa: has revisado las ediciones, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has aprendido:

- a supervisar el trabajo de un agente en tiempo real.
- a aceptar y rechazar ediciones individuales.
- a orientar una sesión activa con prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los resultados del agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
