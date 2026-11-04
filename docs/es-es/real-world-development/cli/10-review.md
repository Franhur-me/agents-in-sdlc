---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo de trabajo completo para Tailspin Toys. En este taller:

- preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado del backlog;
- añadiste valoraciones con estrellas, revisaste el resultado en un navegador reenviado y fusionaste manualmente tu primera pull request (PR);
- empezaste por la incidencia de filtrado, definiste el enfoque en el modo Plan, lo implementaste en Autopilot y lo revisaste en Interactive;
- orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la utilizaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadiste el servidor Playwright Model Context Protocol (MCP) y lo utilizaste para explorar el filtrado en un navegador real;
- creaste y seleccionaste un agente personalizado de aseguramiento de la calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- revisaste todos los cambios del filtrado y autorizaste Agent Merge para la PR;
- exploraste comandos de barra para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Lo que has publicado

El taller tiene dos hitos de pull request; cada uno se creó en su propia rama a partir de `main` actualizado:

1. **Valoraciones con estrellas:** muestra el `starRating` existente y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, utilizaste la misma conversación y rama. Combinamos el trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Comprobaste el código de varias maneras: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, e informó de los resultados en el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y de la cobertura de las pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA no requiere pruebas adicionales, puede ser correcto. Antes de autorizar la fusión, revisa el código y las pruebas, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen estándares; las habilidades describen tareas repetibles; los agentes personalizados definen roles especializados, y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para cada tarea. Usa **Plan** para definir un enfoque antes de implementarlo, **Interactive** para mantener el control de cambios concretos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más potente para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* cambia significativamente el resultado.

## Más cosas que explorar

Has completado el flujo de trabajo principal. Estas funciones de CLI también pueden interesarte:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar una tarea distinta;
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con una herramienta es seguir utilizándola: para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que aprendas con el equipo y aprovecha sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación de GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
