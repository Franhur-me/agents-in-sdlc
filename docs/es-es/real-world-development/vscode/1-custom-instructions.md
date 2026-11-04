---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta, debes asegurarte de que Copilot tenga acceso a las indicaciones pertinentes. Los [archivos de instrucciones][instruction-files] proporcionan directrices para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas;
- generarás la primera parte de datos para el filtrado (una función auxiliar para los editores) con las instrucciones actuales;
- añadirás un nuevo estándar al repositorio en `.github/copilot-instructions.md`;
- volverás a enviar la misma solicitud y observarás el efecto de la nueva directriz;
- confirmarás los cambios de instrucciones y la función de filtrado en `main` para que Copilot pueda utilizar las nuevas indicaciones en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot es probabilístico. El objetivo es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo código.

## Archivos de instrucciones

### Situación

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- Se debe documentar el código mediante comentarios TSDoc.
- Al principio de cada archivo se debe añadir un bloque de comentarios que describa su función.

Los archivos de instrucciones ayudarán a que Copilot realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor el estilo de programación y los requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos con directrices para lenguajes, tipos de archivos o tareas concretos.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código con Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones de otros estándares, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas

- Mantén `copilot-instructions.md` centrado en las directrices generales, como la descripción del proyecto, su estructura y los estándares globales de código.
- Utiliza `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes Astro o la capa de datos, o para tareas concretas.
- Escribe en lenguaje natural, con directrices claras y ejemplos de código correcto e incorrecto.

No hay una única manera de crear archivos de instrucciones ni de utilizar IA; la experimentación te ayudará a encontrar la opción adecuada.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto hay archivos para distintos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a crear archivos de instrucciones. En VS Code, por ejemplo, puedes usar **Configure Chat → Generate Agent Instructions**; en Copilot CLI, `/init`. El ejercicio correspondiente indicará cuándo utilizar esta función.
>
> Explora [awesome-copilot][awesome-copilot] para encontrar plantillas y ejemplos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

El proyecto incluye un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para tareas diferentes.

1. Abre `.github/copilot-instructions.md` y revisa la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas instrucciones se aplican a cualquier interacción con Copilot.
2. Explora `.github/instructions`; hay archivos para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
3. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, que define un patrón glob para seleccionar los archivos a los que se aplican las instrucciones.
4. Revisa las instrucciones específicas para crear pruebas unitarias.
5. Abre `.github/instructions/drizzle.instructions.md` y examina sus enlaces a otros archivos de instrucciones y del proyecto. Las rutas son relativas al propio archivo de instrucciones.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares del proyecto, pero aún no exige documentación dentro del código. Añadirás comentarios TSDoc y comentarios de cabecera más adelante.

## Crea una rama

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio.

Primero enviarás una solicitud con las instrucciones actuales; después actualizarás las instrucciones y volverás a enviar la misma solicitud.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto deseado.
2. Abre `src/lib/publishers.ts` para indicar dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de Chat, selecciona **Agent**.

   ![Selector de agente de Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía esta solicitud:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo en el archivo auxiliar y en las pruebas.
6. La función auxiliar propuesta suele ser una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores, de acuerdo con las convenciones de `.github/instructions/drizzle.instructions.md` para `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni comentarios de cabecera del archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se lo hayas pedido. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar al repositorio

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones del repositorio son adecuadas para los estándares generales, como la norma de comentarios que acabas de añadir. Utiliza archivos `*.instructions.md` para indicaciones de lenguajes, tipos de archivo o tareas concretos.

## Vuelve a enviar la solicitud y observa los cambios

1. En Copilot Chat, selecciona **New Chat**.
2. Vuelve a `src/lib/publishers.ts` y envía la misma solicitud:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

3. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

4. Observa que la función propuesta incluye un comentario TSDoc parecido a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Comprueba los cambios:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y los cambios de la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una pull request

Después de publicar la rama, puedes crear una pull request y asociarla a la incidencia del backlog que trata las instrucciones. Pide a Copilot que localice la incidencia, cree una PR que la cierre y, después, la fusione.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya configura las herramientas de GitHub en el entorno. La primera vez, VS Code puede pedirte que inicies sesión. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat y asegúrate de que está seleccionado **Agent**.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat.
3. Pide a Copilot que encuentre la incidencia de actualización de instrucciones y cree una PR desde la rama actual, describiendo también la función auxiliar de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Cuando Copilot te solicite permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
5. Cuando cree la PR, pídele que la fusione en `main` y que actualice la rama local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revisa la solicitud de permisos antes de permitir que Copilot realice acciones de GitHub y ejecute comandos de shell.

¡Has creado y fusionado una pull request con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y, con Copilot Chat en VS Code:

- enviaste una solicitud de generación de código y observaste el resultado con las instrucciones existentes;
- añadiste un estándar general a `.github/copilot-instructions.md`;
- volviste a enviar la solicitud y observaste cómo el código propuesto adoptaba el nuevo estándar;
- confirmaste las instrucciones y la función auxiliar en `main` para que el siguiente ejercicio pudiera utilizarlas.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
