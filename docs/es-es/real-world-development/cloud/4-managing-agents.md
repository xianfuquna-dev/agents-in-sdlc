---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores asignaste al agente en la nube tres tareas distintas para mejorar la experiencia y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas que tienen PR abiertas, inspeccionar las operaciones realizadas e incluso orientar una sesión activa.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión activa para solicitar una funcionalidad adicional.

## Escenario

Después de pedir al agente que creara un modo de alto contraste, el equipo decidió añadir también un modo claro. Como ya estaba trabajando en actualizar los estilos del sitio y añadir controles, parece razonable incluirlo. Orientarás el trabajo para que incorpore ambos modos.

## Revisa las tareas del agente en la nube

Comprueba el estado de las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas del panel principal y del lateral. Deberías ver las tareas que has asignado, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea activa y revisa las operaciones que ha realizado Copilot, como:
   - obtener el código del repositorio.
   - configurar el entorno.
   - configurar servidores MCP.
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado de la tarea y el enfoque utilizado por Copilot.

4. Revisa también el panel de PR de la derecha para ver la PR y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre que le haya dado Copilot y del estado de la tarea.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que terminó la configuración y empezó a trabajar.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Sesión del agente en la nube en la página de agentes con el cuadro para orientar la sesión resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo añade al trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando. La tarea tardará varios minutos; puedes aprovechar para repasar lo aprendido.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y modificar el alcance de una tarea en curso.

Has aprendido:

- a usar Mission Control y la página de agentes para supervisar tareas.
- a redirigir una sesión activa para solicitar otra funcionalidad.

A continuación, [iterarás sobre las PR creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
