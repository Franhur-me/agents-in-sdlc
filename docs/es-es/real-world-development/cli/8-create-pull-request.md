---
title: "Lección 8 - Crea y fusiona la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de aseguramiento de la calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos en conjunto y abrir una pull request. Fusionaste manualmente la primera PR; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sola conversación y rama; todo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de la revisión y confirmarás la fusión.

## Situación

Durante el flujo de trabajo de filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una pull request. Al habilitarlo, Copilot resuelve los bloqueos de la PR —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a establecer la base si es necesario— y, después, habilita la fusión automática de GitHub cuando el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de habilitar Agent Merge.

## Utiliza Agent Merge para gestionar la PR

Con todo el código creado, revisémoslo en conjunto, creemos la PR y dejemos que Agent Merge gestione el resto.

1. Vuelve a tu Codespace.
2. Abre el cuadro de agentes escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, realizará la fusión.
7. Confirma que aparece un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- inspeccionaste todos los cambios de la funcionalidad y las pruebas de verificación;
- creaste la PR de filtrado;
- habilitaste Agent Merge solo después de revisar los cambios y confirmaste que la PR se había fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
