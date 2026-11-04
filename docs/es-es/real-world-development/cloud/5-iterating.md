---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades: documentar el código, crear una funcionalidad de juegos relacionados para que el equipo de diseño pueda probarla e implementar mejoras de accesibilidad con modos de alto contraste y claro. Revisemos los cambios y, si es necesario, demos indicaciones a Copilot para mejorarlos.

### Situación

Como ya hemos indicado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de habilitar la revisión para el resto del equipo, revisaremos las propuestas de GitHub Copilot para la documentación, la funcionalidad de juegos relacionados y las mejoras de accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de manera asíncrona y sin supervisión, se han establecido restricciones para que el proceso siga siendo seguro:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabaja.
- El agente en la nube se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar la ejecución de los flujos de trabajo de GitHub Actions.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera pull request (PR) del agente en la nube: añadir documentación al código. Utilizarás la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que aparezca una advertencia que indique que el firewall bloquea a GitHub Copilot. Es lo esperado: el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com y selecciona **Pull Requests**.
2. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

3. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

4. Explora el código actualizado, que incluye nuevos comentarios TSDoc y otra documentación. Los cambios exactos variarán. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los comentarios de cabecera que proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Si algún flujo está pendiente de aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

7. En la sección de comprobaciones de la PR, deberían aparecer los flujos en cola. Comprueba que se superen las comprobaciones del proyecto para la aplicación Astro. El proceso puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

El trabajo con Copilot en una PR no es unidireccional. Puedes mencionarlo en comentarios de la PR, igual que a otros miembros del equipo, o añadir comentarios en línea sobre el código. Copilot verá los comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay una solicitud específica que debas usar. Por ejemplo, podrías pedirle que:

- añada al principio de cada archivo de código comentarios que describan brevemente su contenido;
- añada comentarios TSDoc a archivos TypeScript y Astro;
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot**. Puedes elegir una de las ideas anteriores u otra mejora.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una nueva sesión para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un nuevo commit en la PR. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, convierte la PR de borrador en lista para revisión y fusiónala en la rama principal.

![Cambiar la PR a lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Ve al repositorio en GitHub.com, selecciona **Pull Requests** y abre la PR cuyo título sea parecido a **Show related games on the game details page**.
2. Selecciona **Files changed** y revisa el código generado.
3. Cuando termines, vuelve a **Conversation**. Si hay flujos pendientes, selecciona **Approve and run workflows** y comprueba que las comprobaciones del proyecto Astro se completen correctamente.
4. **Opcional:** cambia a la rama en Codespaces para probar manualmente la funcionalidad. En el terminal, sustituye `<branch-name>` por la rama de Copilot, por ejemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes iterar con el agente en la nube y, por ejemplo, ajustar el número de juegos mostrados o pedir que añada comentarios de cabecera y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade las solicitudes como comentarios en la pestaña **Conversation** para que Copilot inicie otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisa las mejoras de accesibilidad implementadas con el agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde Mission Control en el ejercicio 4.

1. En GitHub.com, abre **Pull Requests** y selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.
2. Si Copilot sigue trabajando, espera a que termine (la PR tendrá la marca **[WIP]**).
3. Selecciona **Files changed** y revisa la implementación. Presta atención a:
   - los controles para cambiar de modo;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios CSS o de estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - el código JavaScript/TypeScript que gestiona los modos.
4. Vuelve a **Conversation** y, si hay flujos pendientes, selecciona **Approve and run workflows**. Comprueba que las verificaciones del proyecto se superen.
5. **Opcional:** cambia a la rama en Codespaces para probar las funciones de accesibilidad:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que conservan las preferencias al recargar la página.

Observa cómo el agente personalizado de accesibilidad orientó a Copilot para implementar estas funciones según las prácticas recomendadas. Si detectas problemas o mejoras de accesibilidad, puedes mencionar a **@copilot** en un comentario y pedir una actualización.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puede ocuparse de elementos reales del backlog mientras tú trabajas en otra cosa. Para practicarlo, crea algunas incidencias más y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para indicar el interés de los patrocinadores en la página de detalles del juego;
- implementar paginación en la página con la lista de juegos;
- añadir validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo del agente.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y utilizaste un agente personalizado** de accesibilidad para añadir modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como Mission Control** para supervisar y dirigir la sesión de accesibilidad.
- **Revisaste e iteraste sobre las pull requests del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has terminado el itinerario del agente en la nube. Para seguir explorando, puedes probar otros entornos:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para una pila tecnológica diferente.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
