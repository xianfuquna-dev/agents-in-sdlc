---
title: "Ejercicio 3 - Probar la función con el servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En vez de probar la aplicación manualmente, conectarás el **servidor Playwright MCP** para que Copilot controle un navegador real. Después publicarás la rama y abrirás una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadirás el servidor Playwright MCP al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente la función en un navegador.
- publicarás la rama y abrirás una PR para el filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente resulta más potente cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el modelo de lenguaje y las herramientas, incluidas las proporcionadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente y gestiona la comunicación con los servicios externos, como API existentes o herramientas locales (por ejemplo, paquetes npm). Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona API para gestionar repositorios de GitHub, crear repositorios y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador con Playwright, como navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub mantiene un [registro de MCP](https://github.com/mcp) para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos o acciones sensibles.

## Revisa la configuración MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Ya debería estar configurado el servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

La plantilla incluye la entrada `github`, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor remoto de GitHub MCP][remote-github-mcp-server]. No hay que instalarlo localmente: VS Code se conecta por HTTP y la primera vez que Copilot usa sus herramientas te autenticas con GitHub.

## Añade el servidor Playwright MCP

Añade un segundo servidor. [Playwright MCP][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` se ejecuta localmente: VS Code lo inicia con `npx @playwright/mcp@latest`. La opción `--headless` ejecuta el navegador sin ventana visible, algo necesario en un codespace sin escritorio.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Inicia y confía en el servidor

VS Code inicia los servidores MCP **cuando los necesita**. La primera vez que Copilot usa sus herramientas, VS Code inicia el servidor y te pide confirmar que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece encima de la entrada `playwright`, para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y te pedirá confirmación la primera vez que Copilot necesite sus herramientas.

## Prueba la función de filtrado

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la función:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante Playwright MCP, seguirá los pasos y te informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pregunta o pide a Copilot que corrija el código antes de crear la PR.

El servidor Playwright MCP usa el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. El agente analiza elementos etiquetados y estructurados (botones, enlaces y elementos de lista), como hacen las tecnologías de asistencia. Por eso, esta comprobación funcional también sirve como una revisión básica de accesibilidad.

> [!NOTE]
> Para esta prueba, la aplicación debe estar ejecutándose en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo. La primera vez que Copilot use Playwright MCP quizá tenga que descargar un navegador; si falta, pídele que ejecute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has comprobado que la función funciona. Ahora publicarás la rama para que el equipo pueda revisarla.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Selecciona el icono **+** para preparar los cambios.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una PR desde github.com, la interfaz de GitHub o la CLI. En este ejercicio, pide a Copilot que localice la incidencia pertinente y cree una PR vinculada.

1. Abre el panel Copilot Chat y selecciona **New Chat**.
2. Comprueba que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice acciones. La primera vez que use una herramienta de GitHub, quizá debas autenticarte; sigue las instrucciones.
5. Observa cómo Copilot busca la incidencia correcta y crea la PR.
6. Abre el enlace generado para revisar la PR, pero **no la combines todavía**.

## Resumen y pasos siguientes

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- has añadido Playwright MCP al espacio de trabajo.
- lo has usado para probar la función de filtrado en el navegador antes de publicarla.
- has publicado la rama y abierto una PR.

Has usado MCP para probar una función, pero es solo una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de GitHub MCP][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
