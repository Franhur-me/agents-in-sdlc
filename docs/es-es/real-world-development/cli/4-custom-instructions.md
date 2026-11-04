---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe realizarse de una forma concreta, esa orientación debe estar disponible para Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizarás el archivo de instrucciones para que se sigan los estándares de código;
- observarás el efecto de los archivos de instrucciones en el código.

## Situación

Tailspin Toys cuenta con directrices y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones no obvias, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar el contrato `Props`; los comentarios deben mantenerse actualizados cuando cambie el código correspondiente.
- Se deben conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos. Puedes especificar convenciones de código, bibliotecas y los comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información pertinente para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos con directrices para lenguajes, tipos de archivos o tareas concretos.

> [!NOTE]
> La disponibilidad de otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Revisa su contenido antes de hacer cambios para ver el efecto que producen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces (no en el terminal), abre `.github/copilot-instructions.md`.
3. Revisa la descripción breve del proyecto y las directrices de código. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora los archivos de `.github/instructions`. Hay instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo` del principio, que define un patrón glob para seleccionar los archivos a los que se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y archivos del proyecto. Así puedes dividir las instrucciones extensas en archivos pequeños y reutilizables, y señalar ejemplos que Copilot puede seguir.

## Actualiza los archivos de instrucciones según las directrices del equipo

Los archivos actuales son un buen comienzo, pero falta una directriz. Modifica el archivo principal `copilot-instructions.md` para que TypeScript nuevo incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las nuevas directrices

Copilot CLI carga las instrucciones del repositorio cuando empieza una conversación. Cierra y reanuda la conversación de filtrado para que Copilot cargue las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código con precisión.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas;
- actualizaste el archivo de instrucciones para que se siguieran los estándares de código;
- observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar lint y pruebas de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
