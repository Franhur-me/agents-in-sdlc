---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, prepara todo lo necesario. Crearás una copia propia del repositorio Tailspin Toys, iniciarás un [codespace][codespaces] para trabajar y comprobarás que GitHub Copilot Chat funciona en el editor.

## Configura el repositorio del laboratorio

Crearás una instancia del repositorio a partir de la [plantilla][template-repository]. Incluirá todos los archivos necesarios para el laboratorio.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Selecciona **Use this template** y, después, **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante.

> [!NOTE]
> **El backlog está listo**
>
> Al crear el repositorio a partir de la plantilla, se generará automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante el taller; no tienes que crear ninguna.

## Crea un codespace

Usarás un codespace para completar los ejercicios. [GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un IDE completo con compatibilidad con varios lenguajes, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un Codespace.

    ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, pero será mucho más rápida que instalar todos los servicios manualmente. Mientras esperas, puedes explorar otras funciones de GitHub Copilot.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo][dev-containers] local. Ambos incluyen los requisitos necesarios para facilitar el trabajo. Para ejecutarlo localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usa GitHub Copilot Chat y el modo de agente

Para usar el modo de agente de GitHub Copilot Chat, debes tener instalada la extensión GitHub Copilot Chat en el IDE. Si utilizas un GitHub Codespace, ya debería estar instalada.

> [!TIP]
> Si no tienes la extensión, [instálala desde Visual Studio Code Marketplace][copilot-chat-extension] o abre la vista Extensions de Visual Studio Code, busca **GitHub Copilot Chat** y selecciona **Install**.

Después de instalarla, puede que tengas que autenticarte con tu cuenta de GitHub.

1. Vuelve al codespace.
2. Si Copilot Chat no aparece a la derecha del editor, selecciona el icono **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa <kbd>Enter</kbd>.
4. Si aún no te has autenticado, se te pedirá que inicies sesión en tu cuenta de GitHub. Sigue las instrucciones.

    ![Solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, aparecerá la ventana de Copilot Chat.

## Resumen

¡Enhorabuena! Has creado una copia del repositorio del laboratorio y has iniciado la creación del codespace que utilizarás para programar.

## Siguiente paso

Empecemos a trabajar con Copilot. Continúa con el [ejercicio 1: Instrucciones personalizadas][next-lesson], donde aprenderás a explicarle las convenciones del proyecto.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
