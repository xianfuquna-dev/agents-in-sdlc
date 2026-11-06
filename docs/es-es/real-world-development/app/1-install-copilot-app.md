---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, y automatizas tareas repetitivas en lugar de hacerlo todo por tu cuenta. Con Node.js instalado y la copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- te familiarizarás con el espacio de trabajo y la lista de trabajo inicial de la plantilla.
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar una lista creciente de trabajo pendiente. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

La aplicación está disponible para Windows, macOS y Linux. Instálala, autentícate y añade el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si durante la configuración se te pide que selecciones un repositorio, elige el de Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadirlo después.
6. Cuando se te solicite, elige un tema y termina la configuración.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no hace falta volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, **Add GitHub repository**. Usa el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En esta distribución, carpetas como `tailspin-toys` se agrupan bajo **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

La barra lateral organiza la aplicación en varias áreas:

- **New**: inicia una nueva sesión de chat con Copilot.
- **Pull requests**: explora y filtra tus solicitudes de incorporación de cambios, comprueba el estado de CI y revísalas sin salir de la aplicación.
- **Issues**: explora y filtra incidencias e inicia una sesión a partir de una de ellas.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Sirven para gestionar listas de tareas, realizar mantenimiento periódico u otras tareas repetitivas. El repaso final las ofrece como siguiente paso, no como ejercicio del taller.
- **Customize**: añade funciones a la aplicación mediante servidores MCP, plugins, habilidades y otros componentes. La utilizarás para configurar Playwright MCP.
- **Chats**: conversaciones ligeras para preguntas o lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, por lo que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás la primera sesión al añadir valoraciones por estrellas.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede orientarte.

### Localiza la lista de trabajo inicial

Al crear el repositorio desde la plantilla se generó una lista de incidencias. Localiza las siguientes por el título, sin dar por hecho su número:

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer los detalles. Cada incidencia permite iniciar una sesión de agente. Partirás de la incidencia de filtrado después de realizar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** muestran automáticamente solo elementos de los repositorios que has añadido a la aplicación. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Los chats rápidos permiten hacer una pregunta o plantear ideas sin crear una rama o un worktree. Son adecuados para consultas breves que no necesitan una sesión.

1. Selecciona **+** junto a **Chats** en la barra lateral. Si **Chats** aún no aparece, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta. Cada sesión se ejecuta en su propio worktree de Git aislado, lo que permite ejecutar varios agentes en paralelo sin conflictos. Puedes continuar la conversación o iniciar otra cuando quieras.

## Resumen y pasos siguientes

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y localizar la lista de trabajo inicial en **Issues**.
- usar un chat rápido para formular una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
