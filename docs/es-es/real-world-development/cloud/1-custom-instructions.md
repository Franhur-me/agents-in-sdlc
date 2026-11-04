---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de fondo, debes asegurarte de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas de rutas;
- añadirás un nuevo estándar al repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás una solicitud *antes y después*. El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, así que es más difícil demostrar su efecto en tiempo real. Verás cómo influyen las instrucciones más adelante, cuando revises las pull requests del agente.

## Archivos de instrucciones

### Situación

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe utilizar un tema oscuro y tener un aspecto moderno.
- Se debe documentar el código mediante comentarios TSDoc.
- Al principio de cada archivo se debe añadir un bloque de comentarios que describa su función.

Con los archivos de instrucciones, te asegurarás de que Copilot dispone de la información necesaria para trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor tu estilo de programación y tus requisitos. Puedes especificar convenciones, bibliotecas y los comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las conversaciones o solicitudes de CLI, como la pila tecnológica, una descripción general del proyecto y las prácticas recomendadas.
- `.github/instructions/*.instructions.md`, archivos que ofrecen directrices para tareas o tipos de archivos concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código con Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tienen frontmatter `applyTo`.
>
> Copilot también admite archivos de instrucciones de otros estándares, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Este taller no aborda en profundidad cómo crear archivos de instrucciones, pero el proyecto de ejemplo muestra un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices generales del proyecto, como su descripción, estructura y estándares globales de código.
- Utiliza archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas.
- Escribe en lenguaje natural, con directrices claras y ejemplos de código correcto e incorrecto.

No hay una única manera de crear archivos de instrucciones ni de utilizar la IA. La experimentación te ayudará a encontrar la opción adecuada para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para distintos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente indicará cuándo utilizarla.
>
> Para encontrar plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para distintas tareas.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del principio. Define un patrón glob relativo a la raíz del repositorio; en este caso, se aplica a archivos de pruebas TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y ve al final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos del proyecto. Así puedes dividir conjuntos extensos de instrucciones en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de código del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás normas para los comentarios TSDoc y los comentarios iniciales de archivo.

## Añade un estándar al repositorio

La siguiente es la única edición que harás en este ejercicio: añadir una norma de proyecto para que la documentación se incluya en el código mediante comentarios TSDoc y un comentario inicial de archivo. El agente en la nube la tendrá en cuenta cuando trabaje en incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así también confirmarás los cambios correctamente.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares de código, pero todavía no incluye reglas para la documentación en el código.
4. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden aplicarse a todo el repositorio (`.github/copilot-instructions.md`) o a lenguajes, tipos de archivo o tareas concretos (`*.instructions.md`). El archivo del repositorio es el lugar adecuado para estándares globales como la norma de comentarios que acabas de añadir.

## Confirma, publica y fusiona la actualización

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, partirá de `main`; por eso, los cambios de instrucciones deben llegar a `main`.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una pull request desde `custom-instructions-cloud` a `main` y fusiónala. En el siguiente ejercicio, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama sirve para imitar la forma de trabajar en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones del proyecto y has añadido un nuevo estándar general a `.github/copilot-instructions.md`. Durante el resto de este itinerario verás cómo se aplica a las pull requests del agente en la nube.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
