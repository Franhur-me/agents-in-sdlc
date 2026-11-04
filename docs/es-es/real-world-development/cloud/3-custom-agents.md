---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados, adaptados a tareas o ámbitos concretos de tu flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del dominio para que realice determinados tipos de trabajo con mayor eficacia. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones de pruebas coherentes.

Los agentes personalizados se definen en archivos Markdown de la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML que incluye al menos `name` y `description`, seguido de una indicación en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos puntos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es el trabajador y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades (e incluso otros agentes) durante el trabajo. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; para hacerlo, podría utilizar habilidades como una para el flujo de pull requests o una que ejecute y gestione pruebas.

> [!NOTE]
> No existe una única forma «correcta» de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte a tus entornos y situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Con los agentes personalizados aprenderás a:

- definir agentes personalizados;
- asignar una tarea a un agente personalizado.

## Situación

Tailspin Toys se compromete a que todas las personas puedan utilizar su plataforma de financiación colectiva, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es fundamental, así que utilizarás un agente personalizado para generar esta funcionalidad lo antes posible.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y revisa el archivo del agente personalizado de accesibilidad:

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las prácticas recomendadas de accesibilidad;
   - ejemplos de código para HTML, CSS y JavaScript;
   - errores y problemas habituales.

## Crea una incidencia y asígnala

Mission Control es el lugar central para trabajar con todos los agentes de tu entorno. Puedes asignar tareas al agente en la nube de Copilot, supervisarlas e incluso redirigir una sesión en curso y ofrecer más orientación. Empecemos por asignarle la creación del modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Asignación del agente en la nube con el agente personalizado y Accessibility resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para determinadas tareas y ámbitos. Los agentes personalizados permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que guían a Copilot.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo se asigna una tarea a un agente personalizado.

Mientras Copilot trabaja en el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparación para utilizar agentes personalizados en tu organización][org-custom-agents]
- [Preparación para utilizar agentes personalizados en tu empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
