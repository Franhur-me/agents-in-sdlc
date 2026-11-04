---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica: incidencias de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidad de manera autónoma. Cuando termina, genera una PR en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

Con el agente en la nube explorarás:

- cómo personalizar el entorno de generación de código;
- cómo garantizar que las operaciones se realicen de forma segura;
- por qué las incidencias deben tener un alcance claro;
- cómo asignar incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. El primer paso es añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para hacer pruebas de aceptación de la experiencia. No necesita una implementación perfecta, solo una primera versión. Aunque esta tarea bloquea el trabajo, hay otras incidencias de mayor prioridad.

Ambas son tareas que suelen posponerse y que pueden asignarse al agente en la nube. Este trabaja de forma asíncrona mientras tú te ocupas de otras tareas; después puedes volver y revisar su trabajo.

## Introducción al agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de manera parecida a un desarrollador. Por ejemplo, puedes [asignarle una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una pull request en borrador para seguir el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión durante el trabajo o cuando haya terminado. Cuando la solución esté lista para revisión, te mencionará en la pull request.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas que parecen sencillas pueden ser complejas, así que [define cuidadosamente el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como a un programador colaborativo de IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente, lee los archivos de instrucciones que incluye el repositorio para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, fíjate en la PR resultante: verás comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera derivados de estos archivos. Volveremos a señalarlo cuando revisemos las PR en una lección posterior.

## Configura el entorno de desarrollo del agente en la nube

Para crear código suele ser necesario configurar el entorno y ejecutar scripts de preparación. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente utiliza [GitHub Actions][github-actions] como entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Esto permite instalar las herramientas y dependencias necesarias. El flujo ya está configurado para este laboratorio e instala Node.js, las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y preparada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene dos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la pull request.
- El desencadenador `workflow_dispatch` permite iniciar el flujo manualmente desde la pestaña **Actions** del repositorio. Esto es útil para probar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar sin resolver, reduce la productividad y dificulta mantener el código o incorporar desarrolladores. Copilot puede ayudar a crear documentación, por lo que esta es una buena tarea para asignarle. Trabajará en segundo plano; en un ejercicio futuro volverás a revisar su trabajo.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Code lacks documentation` en **Title**.
5. Escribe este texto en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot** y, en el cuadro de asignación, selecciona **Assign**.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

  ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

8. Selecciona la pestaña **Pull Requests** y abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
9. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno (ejecuta el flujo que hemos visto), analizando el proyecto y determinando cómo abordar la tarea.

10. Revisa la lista de tareas previstas.
11. Desplázate por la cronología de la pull request hasta encontrar una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
12. Selecciona **View session**.

    ![Vista de sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

13. Observa la sesión en directo y cómo Copilot explora el código, analiza su estado, decide un plan y crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube es que puede trabajar de forma asíncrona mientras tú haces otras tareas. Aprovecharemos esa función para asignarle otra tarea y, al mismo tiempo, añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que permite repartir el trabajo: tú puedes centrarte en una tarea mientras él se ocupa de otra. Vamos a asignarle la sección de juegos relacionados.

1. Vuelve al repositorio en github.com y selecciona **Issues** > **New issue** > **Blank issue**.
2. Escribe `Show related games on the game details page` en **Title**.
3. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

4. Selecciona **Create** y, en el lado derecho, **Assign to Copilot**.
5. Selecciona **Assign**. Al poco tiempo, verás varios emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. No es necesario supervisarlo continuamente; revisa el resultado cuando termine. Ahora puedes pasar a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y ayudarte con deuda técnica, nuevas funcionalidades o migraciones de código.

Has aprendido:

- a personalizar el entorno de generación de código;
- a garantizar que las operaciones se realicen de forma segura;
- por qué las incidencias deben tener un alcance claro;
- a asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. También admite [servidores MCP][cloud-agent-mcp] y puede utilizar instrucciones personalizadas, que hemos visto en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
