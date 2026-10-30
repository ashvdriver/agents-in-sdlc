---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando pones GitHub Copilot en modo agente, trabaja de forma autónoma: explora tu base de código, propone cambios, edita archivos y ejecuta comandos. Como ese trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code te ofrece una vista continua de cada llamada a una herramienta y de cada edición de archivo a medida que ocurren. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios concretos y dirigir la conversación con indicaciones de seguimiento para refinar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- dirigirás al agente a mitad de sesión para añadir un conmutador de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras el trabajo está en curso, el equipo de diseño ha vuelto con una petición adicional: también quieren un conmutador de modo claro para los usuarios a quienes el tema predeterminado les resulta demasiado oscuro. En lugar de iniciar una conversación nueva desde cero, dirigirás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista de Copilot Chat muestra cada paso que da el agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición de archivo aparece en línea en el chat con una diferencia y los controles **Keep** y **Undo**.

1. Vuelve a la vista de Copilot Chat del Ejercicio 4.
2. Mientras el agente trabaja, desplázate por el chat para ver cada llamada a una herramienta. Verás lecturas de archivos, ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente propone una edición de archivo, la diferencia aparece en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertir el cambio y dejar que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** situado en la parte superior del chat para ver el conjunto completo de archivos que el agente ha modificado durante esta sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos con los que te sientas cómodo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final con un resumen de los cambios realizados.

## Dirige al agente para añadir el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un conmutador de modo claro. Como continúas la misma sesión de chat, el agente conserva todo el contexto de su trabajo anterior, incluidos los componentes que modificó, el enfoque de estilos que utilizó y el patrón de almacenamiento local que configuró.

1. En el cuadro de entrada de Copilot Chat, envía la indicación de seguimiento siguiente:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo donde lo dejó: explorará los cambios que ya hizo y propondrá adiciones para el modo claro.
3. Revisa cada edición nueva a medida que se aplique, igual que hiciste con el alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente toma una dirección que no quieres (por ejemplo, intenta refactorizar la interfaz del conmutador de una forma que no te gusta), puedes detenerlo con el botón **Stop** situado junto al cuadro de entrada del chat y, después, enviar un mensaje de seguimiento aclaratorio.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa indicaciones de seguimiento para pedir ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), para corregir el rumbo ("don't add a third toggle, integrate light mode into the existing settings panel") o para ampliar el alcance ("also add keyboard shortcuts for both toggles").

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, repasa rápidamente todo lo que ha tocado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones en los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre un par de los archivos modificados y repasa las diferencias. Confirma que se reflejan los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y dirigir una sesión en curso: revisaste cada edición de archivo a medida que se aplicaba, aceptaste o rechazaste cambios y enviaste indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- la supervisión del trabajo de un agente en tiempo real.
- la aceptación y el rechazo de ediciones de archivos concretas.
- la dirección de una sesión a mitad de camino con indicaciones de seguimiento.

En la [lección siguiente][next-lesson] publicarás tu trabajo de accesibilidad como una solicitud de incorporación de cambios y revisarás los cambios que ha producido el agente personalizado local a lo largo de este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Lección siguiente: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
