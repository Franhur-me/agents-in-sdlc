---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Veamos otros que pueden ser útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- utilizarás `/model` para ver los modelos disponibles;
- aprenderás a exportar o compartir una sesión con `/share`;
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar en el agente en la nube.

## Situación

Has terminado el flujo principal de CLI. Ahora explorarás algunas funciones adicionales: la gestión del contexto, el cambio de modelo, el uso compartido de sesiones y la delegación opcional en el [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas puedes alcanzar el tamaño máximo de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando lo necesita, y los comandos de barra te permiten inspeccionar o compactar el contexto manualmente.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a ejecutar `/context` y compara el resultado. Si la conversación ya es pequeña, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si vas a cambiar a una tarea no relacionada y quieres iniciar una conversación nueva.

## Elige el modelo

Cada modelo tiene sus puntos fuertes y cada desarrollador sus preferencias. Copilot CLI permite mostrar los modelos disponibles y seleccionar uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Conserva el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y los aprendizajes ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de tu versión.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir solicitudes, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede dividir una tarea en subtareas independientes, asignarlas a subagentes y ejecutarlas en paralelo;
- `/worktree` puede crear un worktree de Git aislado para una tarea distinta;
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una pull request.

Estos comandos son opcionales porque pueden crear worktrees adicionales o iniciar trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien definida y revisa el resultado mediante el flujo de trabajo habitual. Para aprender más sobre el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y ver qué ocurre en segundo plano. En esta lección:

- utilizaste `/context` y `/compact` para explorar la gestión del contexto;
- utilizaste `/model` para ver los modelos disponibles;
- aprendiste que `/share` puede exportar o compartir una sesión;
- exploraste comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas en el agente en la nube.

Copilot CLI tiene más comandos de barra que puedes explorar. Para terminar el recorrido, [repasa lo que has aprendido][next-lesson] y descubre los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
