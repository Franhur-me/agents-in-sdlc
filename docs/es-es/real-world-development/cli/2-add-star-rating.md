---
title: "Lección 2 - Añadir valoraciones con estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y fusiónalo en tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los juegos ya tienen una valoración con estrellas en sus datos, pero las tarjetas de la página de inicio aún no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo fusionarás como tu primera pull request.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad;
- pedirás al agente que haga un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación y comprobarás el cambio en un navegador reenviado;
- abrirás y fusionarás tu primera pull request.

## Situación

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en su página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, el editor y la descripción. Para empezar, pedirás al agente que muestre en cada tarjeta la valoración existente: es un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación de Copilot, una conversación normal de CLI utiliza el repositorio y la rama que estén activos en el terminal, en lugar de crear un worktree dedicado. Las conversaciones guardadas te permiten volver a la misma conversación, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tus solicitudes y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está abierto, ejecuta:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con `/new` y pulsa <kbd>Enter</kbd>.
5. Solicita el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa las diferencias

Revisa todos los cambios generados por IA antes de fusionarlos, aunque sean pequeños.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que las tarjetas muestran la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija cualquier fallo.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico, no determinista; el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por sentado que funciona. Pide a Copilot que inicie el sitio para examinar la interfaz de usuario (UI) en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran su valoración sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primera pull request

Ya has creado la funcionalidad. Ahora abrirás una pull request (PR) para fusionar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear la PR y mostrará la URL.
3. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación centrada en una rama de funcionalidad;
- pediste al agente un pequeño cambio en las tarjetas de juegos;
- revisaste el cambio con `/diff`;
- ejecutaste la aplicación y comprobaste la valoración en un navegador reenviado;
- abriste y fusionaste tu primera pull request.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
