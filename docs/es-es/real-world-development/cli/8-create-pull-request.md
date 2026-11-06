---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios. Combinaste por tu cuenta la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás todo el cambio de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- habilitarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de filtrado has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para combinar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los impedimentos de la PR —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y vuelve a aplicar los cambios sobre la rama base si es necesario— y, después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa los cambios y confirma que están listos antes de habilitarlo.

## Deja que Agent Merge gestione la PR

Ya está todo el código; revísalo, crea la PR y deja que Agent Merge gestione el resto.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y la combinará cuando todo se haya completado correctamente.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas, validación y gestión de la PR. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- has inspeccionado todo el cambio de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has habilitado Agent Merge después de revisar los cambios y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
