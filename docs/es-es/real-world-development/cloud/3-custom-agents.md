---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones, buenas prácticas, patrones de programación y conocimiento del dominio para orientar su trabajo. Los equipos pueden convertir su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, como mínimo, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar tareas. La distinción más clara es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad y podría usar habilidades, por ejemplo, para crear una PR o ejecutar pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar la que mejor se adapte al entorno y al escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio:

- aprenderás cómo se definen los agentes personalizados.
- asignarás una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de su visión o preferencias. Algunos usuarios han indicado que el tema oscuro actual es difícil de leer porque hay poco contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, usarás un agente personalizado para implementar la funcionalidad cuanto antes.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa el archivo para entender cómo guiará a Copilot.

Vuelve a tu codespace y abre `.github/agents/accessibility.md`.

1. Fíjate en los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

2. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio accesible.
   - las buenas prácticas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Asigna una incidencia

Mission Control es el centro para trabajar con agentes: puedes asignar tareas al agente en la nube de Copilot, supervisarlas y orientar las sesiones. Asígnale una tarea para crear el modo de alto contraste.

1. Ve al repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** y, después, **Blank issue**.
4. Escribe `Add high contrast mode to website` en **Title**.
5. Escribe este texto en **Description**:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecciona **Create** para crear la incidencia.
7. En el lado derecho, selecciona **Assign to Copilot**.
8. Selecciona **Accessibility agent** en la lista de agentes personalizados.

   ![Asignación al agente en la nube con el agente personalizado de accesibilidad resaltado](../../../_images/ex5-select-custom-agent.png)

9. Selecciona **Assign**.
10. Copilot empezará a trabajar en segundo plano.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Permiten convertir la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has aprendido:

- cómo se definen los agentes personalizados.
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, aprenderás a [supervisar y orientar la sesión del agente][next-lesson] desde Mission Control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar la organización para usar agentes personalizados][org-custom-agents]
- [Preparar la empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
