---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear filtros a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios necesitan un proceso más completo. GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo de una organización y ayudarnos a crear lo que necesitamos de la forma adecuada. En esta y las siguientes lecciones seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que funciona como se espera y, finalmente, lo fusionarás en el proyecto.

> [!NOTE]
> Seguirás usando la misma conversación y rama durante el flujo de trabajo de la funcionalidad. En general, usarías ramas o PR distintas para los diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- validarás manualmente la funcionalidad en un navegador reenviado.

Más adelante actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Situación

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editor. La incidencia del backlog describe la funcionalidad, pero hay detalles —como la combinación de categorías— que deben acordarse antes de programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA no cambia los fundamentos del desarrollo; de hecho, los hace aún más importantes. Un flujo de trabajo habitual suele ser:

1. Partir de una incidencia que describa el trabajo.
2. Planificar qué se debe crear.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Fusionar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen un patrón parecido.

Seguir este enfoque habitual garantiza que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla la autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente; este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha fusionado la PR de valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la solicitud y volver a la pestaña **Session**.

Observa que la solicitud ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o pull request (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir el enfoque para implementar una funcionalidad o realizar una tarea antes de delegársela a Copilot. Es buena idea dedicar tiempo a planificar las tareas complejas.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo de la solicitud muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque para elaborar un plan.

3. Puede que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Revisa el plan.
6. Pide a Copilot que cambie cualquier aspecto que no coincida con tus decisiones.

## Autoriza Autopilot

Una vez redactado y revisado el plan, toca implementarlo. Autopilot permite que Copilot itere hasta considerar que ha completado el trabajo.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Confirma que el indicador debajo de la solicitud muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo primero. Si Copilot informa de dependencias ausentes o conflictos de puertos, resuelve el problema de configuración antes de dar por completas las comprobaciones.

## Revisa y verifica la implementación

Como cualquier otro código, el código generado debe revisarse antes de fusionarlo.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Cuando termines, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y por editor, así como las combinaciones que acordaste en el plan.
4. Confirma que el restablecimiento y los resultados vacíos coincidan con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot a partir de una incidencia de GitHub;
- definiste requisitos en el modo Plan;
- implementaste la funcionalidad con el modo Autopilot;
- revisaste el código;
- validaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
