---
title: "Ejercicio 3 - Prueba tu funcionalidad con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo de agente. Antes de abrir una pull request, debes confirmar que funciona en el navegador. En vez de recorrer la aplicación manualmente, conectarás el **servidor Playwright MCP** para que Copilot controle un navegador real y pruebe la funcionalidad. Después publicarás la rama y abrirás una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadirás el servidor Playwright MCP al espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad de filtrado en un navegador;
- publicarás la rama y abrirás una pull request para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente puede hacer mucho más cuando accede a herramientas externas. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con ellas.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las herramientas proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Estas herramientas y recursos se proporcionan mediante un servidor MCP, que actúa como puente entre el agente de IA y los servicios externos. El servidor gestiona la comunicación con herramientas externas, como API existentes o herramientas locales en paquetes npm. Cada servidor ofrece al agente un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear repositorios, actualizarlos y gestionar incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador mediante Playwright, como navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar la búsqueda y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia. Antes de utilizar uno, revisa cuidadosamente su código, comprueba quién lo publica y valora las implicaciones de seguridad. Utiliza únicamente servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace. Deberías encontrar un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`, que permitió a Copilot leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor remoto de GitHub MCP][remote-github-mcp-server]; no tienes que instalarlo localmente. VS Code se conecta por HTTP y te autenticarás en GitHub la primera vez que Copilot utilice sus herramientas.

## Añade el servidor Playwright MCP

El [servidor Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en el equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, requisito para un codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, selecciona la acción **Start** que aparece sobre la entrada `playwright` para iniciar el servidor y verificar su configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

El inicio manual es opcional: si lo omites, VS Code lo iniciará y solicitará tu autorización cuando Copilot necesite las herramientas.

## Prueba la funcionalidad de filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot iniciará un navegador mediante Playwright MCP, realizará los pasos y te devolverá un informe. Compáralo con los criterios de aceptación de la incidencia. Si algo no funciona, haz preguntas o pide a Copilot que corrija el código antes de abrir la PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor, vuelve a iniciarlo. La primera vez que Copilot utilice Playwright MCP quizá tenga que descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una pull request

1. En el codespace, abre el panel **Source Control** y revisa los cambios de Copilot.
2. Prepara los cambios con el icono **+**.
3. Genera un mensaje de commit mediante el botón **Sparkle**.

   ![Panel Source Control con los cambios](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la pull request

Puedes crear una pull request en github.com o con GitHub CLI. En este ejercicio, Copilot buscará la incidencia correspondiente y asociará la PR.

1. En Copilot Chat, selecciona **New Chat**.
2. Comprueba que **Agent** está seleccionado para que Copilot pueda utilizar las herramientas de GitHub.
3. Envía esta solicitud:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice operaciones. La primera vez que utilice una herramienta de GitHub, puede que tengas que autenticarte.
5. Copilot buscará las incidencias, encontrará la adecuada y creará la PR.
6. Selecciona el enlace de Copilot para revisar la pull request, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- aprendiste qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot;
- añadiste el servidor Playwright MCP al espacio de trabajo;
- lo utilizaste para probar la funcionalidad de filtrado en un navegador antes de publicarla;
- publicaste la rama y abriste una pull request.

Has utilizado MCP para probar una funcionalidad, pero es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
