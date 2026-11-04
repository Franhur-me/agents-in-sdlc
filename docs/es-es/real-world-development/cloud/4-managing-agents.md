---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente en la nube de Copilot que se encargue de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente está diseñado para trabajar de manera autónoma y asíncrona, es importante poder supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de los agentes que tienen pull requests (PR) abiertas, explorar las operaciones realizadas e incluso dirigir una sesión en curso para orientarla.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Situación

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que sería un buen momento para añadir también un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo la funcionalidad de alternancia, parecía lógico incluirlo. Dirigirás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado actual de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa las tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación de la base de código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Observa también el panel de pull request (PR) que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la creación del modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta solicitud:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la solicitud.
5. Observa cómo Copilot reconoce la solicitud y la incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir tu mensaje.

Este proceso también tardará varios minutos; puedes aprovecharlos para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo realizado e incluso redirigir tareas en curso para cambiar su alcance o añadir indicaciones.

Has aprendido:

- a utilizar Mission Control y la página de agentes para supervisar las tareas del agente en la nube;
- a redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las pull requests de Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
