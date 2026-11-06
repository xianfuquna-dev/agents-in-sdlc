---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le pediste que documentara el código, creara una función de juegos relacionados para que el equipo de diseño pudiera probarla e implementara mejoras de accesibilidad, incluidos los modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de habilitar la revisión para el resto del equipo, revisa las propuestas de Copilot para la documentación, los juegos relacionados y la accesibilidad.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se han establecido límites de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions, en un entorno efímero independiente.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de ejecutarlo.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) creada por el agente en la nube: la documentación del código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que veas un aviso de que el firewall bloqueó a GitHub Copilot. Es **normal**: Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario: proceden de las instrucciones personalizadas que revisaste al principio del Ejercicio 2.
6. Cuando termines la revisión, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en las comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, como harías con cualquier miembro del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay un prompt único. Puedes pedirle, por ejemplo:

- que añada encabezados breves al principio de cada archivo de código.
- que añada comentarios TSDoc a archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**. Puedes elegir uno de los ejemplos anteriores u otra mejora.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios. Puedes seguir iterando hasta quedar conforme. Después, convierte la PR en lista para revisión y combínala.

   ![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora vuelve a la PR que Copilot creó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando termines, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca el indicador de flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que las comprobaciones del proyecto para la aplicación Astro se superan; puede tardar unos minutos.
9. **Opcional:** cambia a la rama de esta PR en tu Codespace para probar manualmente la función. Sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Puedes seguir iterando con el agente en la nube, por ejemplo, para cambiar el número de juegos que se muestran o pedir encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Añade los cambios como un comentario nuevo en la pestaña **Conversation** para iniciar otra sesión.

## Revisa las mejoras de accesibilidad

Por último, revisa las mejoras de accesibilidad implementadas por el agente personalizado. La PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde Mission Control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Abre la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación, en especial:
   - los componentes de interfaz para alternar los modos.
   - el uso del almacenamiento local para guardar las preferencias.
   - los cambios de CSS o estilos para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación por teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.
6. Cuando termines, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca el indicador de flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que las comprobaciones del proyecto para la aplicación Astro se superan; puede tardar unos minutos.
10. **Opcional:** cambia a la rama de la PR en tu Codespace para probar manualmente la accesibilidad. Sustituye `<branch-name>` por la rama creada por Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones según buenas prácticas. Si detectas problemas de accesibilidad o mejoras posibles, menciona a **@copilot** en un comentario para solicitar cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para asignarle tareas reales del backlog y ocuparte de otra cosa mientras trabaja. Crea más incidencias en tu repositorio y asígnaselas a Copilot. Por ejemplo:

- crear un formulario de interés para mecenas en la página de detalles de un juego.
- añadir paginación a la lista de juegos.
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones del repositorio** para reconocer su efecto en los resultados del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como Mission Control** para supervisar y orientar una sesión activa.
- **Has revisado e iterado sobre las PR del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y pasos siguientes

Has terminado el recorrido del agente en la nube. Para seguir explorando, puedes probar los otros recorridos:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente QA personalizado, Playwright MCP, Agent Merge y la opción de delegar al agente en la nube con `/delegate`.

En tu propio repositorio, también puedes:

- asignar al agente una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otro ámbito.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas del flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Supervisar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
