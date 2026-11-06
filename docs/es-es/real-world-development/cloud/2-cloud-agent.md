---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Instrucciones personalizadas][previous-lesson]

Pocas organizaciones están libres de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de recursos. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una PR en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y dedicarte a cuestiones de mayor alcance.

En este ejercicio explorarás cómo usar el agente en la nube de Copilot para:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. El primer paso será añadir comentarios TSDoc a todas las funciones exportadas.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para explorar la experiencia de usuario y probar su aceptación. No necesita una implementación refinada todavía. Es una tarea bloqueante, aunque hay otras incidencias con mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente al final de la lista, por lo que son buenas candidatas para el agente en la nube. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar su trabajo.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una PR en borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes inspeccionar la sesión mientras sigue activa o después de que termine. Cuando la solución esté lista para revisión, te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no es magia: sigue haciendo falta pensar. Incluso las tareas aparentemente sencillas pueden ser complejas. Ten presente cómo defines el alcance de las tareas para el [agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores aprendiste cómo las instrucciones personalizadas guían a Copilot. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones del repositorio para poder reconocer su efecto en los resultados.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas indicaciones proceden de los archivos de instrucciones. Volveremos a revisarlas al inspeccionar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente en la nube

Crear código suele requerir un entorno específico y scripts de configuración. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para configurar su entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puede acceder a las herramientas y dependencias necesarias. El taller ya incluye esta configuración para instalar Node.js, las dependencias, los navegadores de pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend y aplicación: Node.js (ahora toda la aplicación es Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero hay dos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todos reconocen la importancia de la documentación, muchos proyectos tienen información obsoleta o carecen de ella. Esa deuda técnica ralentiza el trabajo y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede generar documentación, por lo que esta incidencia es ideal para asignarla al agente en la nube. En un ejercicio posterior volverás para revisar el resultado.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe este texto en **Description**:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

   ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando cómo abordar la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR hasta ver que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Explora la sesión en directo y observa cómo Copilot explora el código, analiza el estado, decide un plan y crea código.

El trabajo puede tardar varios minutos. Una ventaja del agente en la nube es que trabaja de forma asíncrona y te permite ocuparte de otras tareas. Aprovecharemos esa característica asignándole otra tarea y, mientras tanto, nos centraremos en añadir funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

El agente en la nube permite dividir el trabajo: tú puedes centrarte en unas tareas mientras él se ocupa de otras. Aunque añadir una sección de juegos relacionados no lleve mucho tiempo, puedes dedicar ese tiempo a otras tareas. Asígnasela al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. Escribe este texto en **Description**:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot** y, después, **Assign**.

Poco después aparecerá el emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como trabaja de forma parecida a un ingeniero de software, no hace falta supervisarlo continuamente; revisa el resultado cuando termine. Ahora veremos cómo crear y usar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y resuelva deuda técnica, cree funcionalidades o ayude a migrar código a otro framework.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, pasarás a crear y usar agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y recibe las instrucciones personalizadas que vimos en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas del flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
