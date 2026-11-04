---
title: "Lección 12 - Publica el sitio con GitHub Pages"
description: "Usa la aplicación de GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y verificar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz para que cualquiera pueda acceder a él con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero tú debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages;
- revisarás el flujo de trabajo y la pull request antes de fusionarla;
- seguirás el despliegue y comprobarás el sitio publicado.

## Situación

Space Quiz ya está en un repositorio de GitHub, pero la gente necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él mediante una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación de Copilot para `space-quiz`, envía esta solicitud:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama de tres fases: una pull request de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz disponible en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Esta solicitud pide al agente que cree y fusione una pull request y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de despliegue y las solicitudes de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y verifica el despliegue

1. Confirma que el archivo README solo se ha añadido si el repositorio aún no tenía uno.
2. Revisa los cambios de la pull request, incluidos los pasos para compilar y desplegar del flujo de trabajo.
3. Fusiónala solo cuando se hayan superado las comprobaciones obligatorias de la pull request.
4. Sigue la ejecución del despliegue de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o fusionar la pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que el despliegue se complete correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio web y has verificado el despliegue en lugar de confiar únicamente en el informe del agente. Continúa con [Lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
