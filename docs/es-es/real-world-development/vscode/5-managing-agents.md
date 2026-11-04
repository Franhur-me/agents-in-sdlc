---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo de agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición. Puedes revisar las diferencias, aceptar o rechazar cambios concretos y orientar la conversación con solicitudes adicionales sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios de archivos propuestos;
- orientarás al agente durante la sesión para que añada un control de modo claro junto al de alto contraste.

## Situación

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, pensado para quienes consideran demasiado oscuro el tema predeterminado. En vez de empezar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: archivos que explora, comandos que ejecuta y cambios que propone. Las ediciones aparecen en línea con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos del terminal que solicita ejecutar.
3. Cuando proponga una edición:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. Abre el indicador **Files changed**, en la parte superior del chat, para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta necesita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que termine el modo de alto contraste. Aparecerá un mensaje final que resume los cambios.

## Pide al agente que añada el modo claro

Ahora ampliarás la misma conversación para añadir un control para el modo claro. Como continuarás en la misma sesión, el agente conservará el contexto de su trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde los cambios existentes y propondrá los cambios necesarios para el modo claro.
3. Revisa cada cambio nuevo y selecciona **Keep** o **Undo**.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, intenta reorganizar el control de alternancia—, deténlo con el botón **Stop** junto al campo de chat y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Puedes pedir ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y posiblemente pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se han aplicado las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: revisaste cada edición, aceptaste o rechazaste cambios y enviaste solicitudes de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has aprendido:

- a supervisar el trabajo de un agente en tiempo real;
- a aceptar y rechazar ediciones individuales;
- a orientar una sesión en curso mediante solicitudes de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad como pull request y revisarás los cambios del agente local personalizado.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
