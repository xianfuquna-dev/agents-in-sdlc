---
title: "Ejercicio 2 - Añadir funciones con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos suelen afectar a varios archivos y requieren tareas como ejecutar pruebas. El flujo habitual de desarrollo incluye localizar los archivos, hacer los cambios, ejecutar las pruebas, depurar, encontrar el archivo que faltaba y volver a modificarlo. El **modo agente de Copilot** puede encargarse de este trabajo en el IDE: explora el proyecto, realiza cambios coordinados, ejecuta tareas como las pruebas y corrige los errores que detecta. En este ejercicio lo usarás para añadir una función al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción. El agente en la nube trabaja de forma asíncrona, como parte del equipo, y se comunica mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente explora el proyecto, encuentra archivos pertinentes y realiza cambios coordinados.
- el modo agente implementa funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior creaste un helper de editores; ahora completarás con el modo agente el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de cambiar nada, explora el sitio para entender cómo funciona.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar los juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con scripts `<script>` de Astro pequeños y acotados, usando las API estándar del DOM.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y selecciona una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar un enlace localhost desde el terminal se redirige a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado al servidor del codespace.

### Explora el sitio

Cuando el sitio esté en marcha, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea importante.

La aplicación funciona, pero queremos añadir capacidades. Pide a Copilot que muestre las incidencias del backlog que se creó automáticamente al generar el proyecto desde la plantilla.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal y utiliza herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no expresan una preferencia por un modelo concreto. Se incluye Claude Sonnet 4.5 porque se utilizó al crear el taller y se espera que así las sugerencias sean relativamente coherentes. Sin embargo, los modelos de lenguaje son probabilísticos, por lo que las sugerencias pueden diferir de los ejemplos. Es normal.

> [!NOTE]
> Copilot puede utilizar un comando MCP distinto, pero debería completar la tarea.

3. Envía este prompt a Copilot:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada. Copilot también habrá priorizado los elementos según el valor que considera que aportan al usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa las instrucciones que proporcionan contexto. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas generales sobre cómo añadir funciones. Incluyen:
   - una descripción de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar otros archivos de instrucciones y archivos del proyecto. Las rutas son relativas al archivo de instrucciones; así se pueden reutilizar pautas, dividir instrucciones complejas y proporcionar ejemplos o plantillas.

## Implementa el filtrado

Para completar el filtrado, hay que actualizar la lógica de la capa de datos en `src/lib/`, añadir o ajustar las pruebas y actualizar la página de lista de juegos con la interfaz de filtrado. Las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas.

1. Continúa la conversación actual con Copilot o selecciona **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.
4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la función según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los archivos relacionados con la función y encuentra los helpers de datos, la interfaz y las pruebas. Después modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot puede tardar varios minutos, según la complejidad de la tarea y del código. Mientras trabaja, puede que aparezcan los controles **Keep** y **Undo**. Cuando termine, aparecerán para todos los cambios; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.
8. Puede haber pausas y algunas pruebas pueden fallar mientras Copilot itera entre la generación de código y las pruebas. Espera a que termine y no detecte errores.

   ![Sesión completada de Copilot en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado.

> [!CAUTION]
> Revisa siempre el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y prueba la nueva función.
11. Cuando hayas comprobado el funcionamiento y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has usado el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y la capa de datos.
- explorar el proyecto, encontrar archivos pertinentes y coordinar cambios.
- revisar los cambios y pruebas generados antes de combinarlos.

A continuación, [probarás la función con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

Al crecer la lista de juegos, será necesario añadir paginación. Usa lo aprendido para pedir a Copilot que la implemente. Sigue las instrucciones existentes, decide si el usuario podrá elegir el tamaño de página o si será fijo y proporciona el contexto necesario. Es posible que debas iterar con Copilot; eso es normal.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
