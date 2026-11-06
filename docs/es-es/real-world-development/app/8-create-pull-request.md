---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la PR de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión lee la solicitud y resuelve los impedimentos: corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y reorganiza la base cuando hace falta. Después la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa tras reiniciar la aplicación y se desactiva cuando la PR se ha combinado.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero su capacidad para editar código y combinar todavía requiere tu autorización explícita. Revisa las acciones que se le permiten y el trabajo antes de conceder permiso para combinar.

## Deja que Agent Merge gestione la PR

Ya está todo el código creado y revisado. Ahora permitirás que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge se inicia y:

- crea la solicitud de incorporación de cambios con un título y una descripción.
- si empezaste desde una incidencia, la menciona en el cuerpo de la descripción.
- reorganiza la base o gestiona posibles conflictos con la rama de destino.
- supervisa la integración continua para comprobar que se superan todas las verificaciones.
- supervisa la PR para detectar comentarios de desarrolladores o de la revisión de código de Copilot y realiza cambios para resolverlos.
- puede combinar automáticamente la PR cuando todo se haya completado correctamente.

Vamos a permitir que Agent Merge combine la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el proceso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**; que Agent Merge esté habilitado o activo no significa que la PR ya se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas, validación y gestión de la PR. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- has inspeccionado la PR completa de la función y las pruebas de verificación.
- has autorizado Agent Merge después de la revisión y confirmado que la PR se ha combinado.

A continuación, [usarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar otra forma de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
