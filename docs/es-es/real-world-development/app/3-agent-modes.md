---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo existente de una organización y ayudarnos a crear lo adecuado de la forma correcta. En esta lección y las siguientes seguirás un proceso de desarrollo habitual dirigido por agentes: partirás de una incidencia, desarrollarás una funcionalidad, comprobarás el código y el comportamiento y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de la funcionalidad. Normalmente usarías sesiones o PR distintas para los diferentes tipos de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una nueva sesión de agente desde una incidencia de GitHub.
- definirás los requisitos en modo **Plan**.
- implementarás la funcionalidad con **Autopilot**.
- revisarás el código.
- validarás manualmente la función en un lienzo de navegador.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan acotar los juegos por categoría y editor. La incidencia del backlog describe la función, pero hay que acordar detalles como la combinación de categorías antes de programar. Usarás Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Abrir una incidencia que detalle lo que debe hacerse.
2. Crear un plan de lo que debe desarrollarse.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se revise y se supere la integración continua.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos siguen alguna variante de este modelo.

Este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de validación que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: tú y el agente trabajáis en equipo. El agente propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que se ha combinado la PR de valoraciones por estrellas y que la rama `main` local está actualizada.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente de la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

   ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, comprueba el `PATH` de la aplicación o la configuración del gestor de versiones y, después, cierra completamente y vuelve a abrir la aplicación. Reintenta la operación y las comprobaciones; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la función de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editor, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios al plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación lo cambia. Confirma que el indicador muestra **Autopilot**.

Copilot comenzará a trabajar: seguirá el plan, generará código y ejecutará pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y verifica la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editores. Comprueba que sigue las instrucciones existentes del repositorio.
3. Examina los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Corrige los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local; confirma que el servidor probado pertenece a este worktree y no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Pide a Copilot que inicie la aplicación y abra la página en el lienzo:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona un editor y confirma los resultados.
4. Combina filtros de categoría y editor y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin resultados y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Al terminar, pide a Copilot que detenga el servidor iniciado para esta sesión:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, worktree y rama para las Lecciones 4 a 8. A continuación aplicarás instrucciones personalizadas al código y añadirás mejoras de calidad y de QA antes de crear y combinar la PR en la Lección 8. Puedes dejar la sesión inactiva después de detener su servidor y volver a ella en la siguiente lección.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para desarrollar y revisar una función. En esta lección:

- has iniciado una sesión de agente desde una incidencia de GitHub.
- has definido los requisitos en modo **Plan**.
- has implementado la función con **Autopilot**.
- has revisado el código.
- has validado manualmente la función en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
