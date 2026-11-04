---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. Un desarrollador suele localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a modificar el código. El proceso puede alargarse.

El modo de agente de Copilot ayuda a automatizar este trabajo en el IDE. Explora la estructura del proyecto, realiza cambios en varios archivos, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. En este ejercicio lo usarás para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE, con ciclos rápidos de interacción. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- pedir al modo de agente que explore el proyecto, identifique archivos y coordine cambios;
- implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados antes de fusionarlos.

## Situación

El catálogo de juegos está creciendo y quieres que sus visitantes puedan filtrar por categoría y editor. En el ejercicio anterior añadiste una función auxiliar para los editores; ahora completarás la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para entender su funcionalidad. Es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro acotados que utilizan las API DOM estándar.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo Astro.

1. Vuelve al Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, aparecerá una URL parecida a `http://localhost:4321`.
5. Si un cuadro de diálogo te pregunta si quieres abrir esa URL, ciérralo seleccionando **x**. Para abrir el sitio, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost del terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del codespace.

La página de inicio muestra juegos de mesa con título, imágenes y descripciones. Al seleccionar un juego, se abre su página de detalles con información adicional, incluido el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, abre Copilot Chat en el codespace y selecciona **New Chat**. La selección de modo y modelo varía según el ejercicio.

La primera versión del sitio funciona, pero queremos añadir capacidades. Al crear el proyecto a partir de la plantilla se generó un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de Chat, selecciona **Agent**, que puede planificar e implementar cambios en varios archivos, ejecutar comandos del terminal e invocar herramientas.
2. Selecciona **Claude Sonnet 4.5** en la lista de modelos.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Durante su elaboración se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente coherentes, pero las respuestas pueden variar porque los modelos de lenguaje son probabilísticos.

> [!NOTE]
> Copilot puede utilizar un comando MCP distinto debido a la naturaleza probabilística de los modelos, pero debería completar la tarea.

3. Envía esta solicitud:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se enumeren todas las incidencias.
5. Revisa la lista y la priorización que ha generado Copilot.

## Revisa los archivos de instrucciones

Antes de iniciar el agente, revisa el archivo de instrucciones que proporcionará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa la arquitectura, los principios de diseño, las pruebas y la accesibilidad, además de los enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones; esto permite reutilizar, dividir y complementar instrucciones con ejemplos.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios:

- añadir o ajustar la lógica de filtrado de la capa de datos en `src/lib/`;
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página con la lista de juegos para incluir la interfaz de filtrado.

Las pruebas deben ejecutarse y superarse antes de fusionar los cambios.

1. Continúa la conversación actual con Copilot o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui**.

   ![Selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que está seleccionado **Agent**.
4. Confirma que sigue seleccionado **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad conforme a la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza la capa de datos, la interfaz y las pruebas, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Puede que Copilot tarde unos minutos en explorar, editar y ejecutar pruebas. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo**. No hace falta seleccionarlos hasta que el trabajo haya terminado.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.
8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.
9. Revisa el código generado.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador y explora la nueva funcionalidad.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

Has utilizado el modo de agente de GitHub Copilot para añadir funcionalidad al sitio de Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y la capa de datos;
- explorar el proyecto, localizar archivos y coordinar cambios;
- revisar los cambios y las pruebas generados antes de fusionarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una pull request.

### Ejercicio de exploración adicional: implementar paginación

Con el aumento de la lista de juegos, será necesario añadir paginación. Utiliza lo aprendido y pide a Copilot que implemente esta función. Sigue las instrucciones existentes, decide si quieres que el usuario seleccione el tamaño de página o si debe ser fijo, y proporciona a Copilot el contexto necesario. Puede que tengas que iterar y añadir más indicaciones: es normal cuando trabajas con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
