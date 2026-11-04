---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos de tu flujo de desarrollo. Al definir agentes mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de código y conocimientos del dominio. Los equipos pueden convertir su experiencia en agentes reutilizables; por ejemplo, un agente de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML que incluye al menos `name` y `description`, seguido de una indicación Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen puntos en común: ambos se definen principalmente mediante archivos Markdown y explican a una IA cómo realizar operaciones. La diferencia más sencilla es que un **agente personalizado** es el trabajador y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este laboratorio, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; podría utilizar habilidades para gestionar pull requests o ejecutar pruebas.

> [!NOTE]
> No existe una única manera correcta de crear un agente personalizado. Prueba e itera para encontrar la mejor opción para tu entorno y tus necesidades.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Situación

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades o preferencias visuales. Los comentarios de usuarios indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, utilizarás un agente personalizado para implementar rápidamente esta función.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- lo utilizarás en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisaremos su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y, en un terminal, crea una rama basada en `main` para el trabajo de accesibilidad (mantendrás separada la PR de filtrado del ejercicio 3):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones que describen las responsabilidades principales, las prácticas recomendadas, ejemplos de HTML, CSS y JavaScript, y errores habituales al generar código para un sitio web accesible.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes de Copilot Chat. Al seleccionar uno, la conversación utiliza las instrucciones y herramientas de ese agente.

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.

1. En el menú de agentes de Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agente de Chat](../../../_images/shared-chat-mode-selector.png)

2. En la parte inferior de la vista, abre el menú desplegable del agente; de forma predeterminada, muestra **default**.
3. Selecciona **Accessibility agent**.
4. Envía esta solicitud:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos Astro, CSS y posiblemente pruebas relacionadas.

En el siguiente ejercicio revisarás y orientarás este trabajo mientras está en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba tu funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
