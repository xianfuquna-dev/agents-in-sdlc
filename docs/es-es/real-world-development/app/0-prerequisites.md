---
title: "Lección 0 - Requisitos previos"
description: "Prepara el entorno para las lecciones de la aplicación GitHub Copilot: instala Node.js para el proyecto Tailspin Toys y crea tu propia copia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que sirve como centro de operaciones para Copilot y GitHub. Ofrece acceso rápido a incidencias y solicitudes de incorporación de cambios y permite desarrollar con GitHub Copilot. Durante este taller trabajarás en local con la aplicación Tailspin Toys, creada con Astro, mediante la aplicación GitHub Copilot. Antes de empezar, comprueba que Node.js esté instalado en local y, después, instala la aplicación Copilot.

En esta lección:

- instalarás Node.js para poder ejecutar las pruebas del proyecto en tu equipo.
- crearás tu propia copia del proyecto Tailspin Toys a partir de la plantilla.

## Instala Node.js

En varias lecciones se pide a un agente que implemente funciones y ejecute en local el conjunto de pruebas de Tailspin Toys, para lo que se necesita **[Node.js][nodejs]**, el único entorno de ejecución que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla en cualquier plataforma es usar el instalador oficial:

1. En tu sistema operativo, abre un terminal, como Windows Terminal, Terminal de macOS o el que utilices habitualmente.
2. Ejecuta estos comandos para comprobar las versiones instaladas de Node.js y npm:

    ```shell
    node --version
    npm --version
    ```

3. Si ambos comandos funcionan y Node.js cumple los requisitos indicados en el README y `package.json` del proyecto, pasa a la siguiente sección.

> [!TIP]
> Completa estos pasos solo si no tienes Node instalado o necesitas actualizarlo.

4. Abre la [página de descargas de Node.js][node-download].
5. Descarga la versión **LTS** para tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, mantén seleccionada **Add to PATH**.
7. Abre un terminal nuevo después de la instalación.
8. Comprueba la instalación en el nuevo terminal:

    ```bash
    node --version
    npm --version
    ```

9. Deberían aparecer las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en tu terminal habitual no garantiza que estén disponibles en la aplicación. Cierra completamente la aplicación y vuelve a abrirla después de instalar Node.js o cambiar `PATH`. Si alguno de los comandos sigue fallando en una sesión, sigue los [pasos de solución de problemas del entorno de sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree también necesita las dependencias del proyecto y Chromium de Playwright para las comprobaciones E2E. Sigue el README del repositorio Tailspin Toys al preparar un worktree y revisa cualquier solicitud de instalación antes de aprobarla.

## Configura el repositorio del taller

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala desde el [repositorio de plantilla][template-repository]. El nuevo repositorio contiene todos los archivos necesarios y lo conectarás cuando instales la aplicación.

1. Abre una ventana nueva del navegador y ve al repositorio de GitHub del taller: `https://github.com/github-samples/tailspin-toys`.
2. Crea una copia propia del repositorio seleccionando **Use this template** en la página del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template con Create a new repository seleccionado en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio completado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), porque la necesitarás más adelante.

> [!NOTE]
> Al crear el repositorio desde la plantilla, se genera automáticamente una lista de trabajo con incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, código de la aplicación, pruebas, una habilidad `quality-checks` y una extensión de Canvas existente. Personalizarás la habilidad y crearás un agente QA durante el taller. Si usas una copia antigua, consulta con el instructor si contiene los archivos necesarios.

## Resumen y pasos siguientes

Ya tienes el entorno preparado. En esta lección:

- has instalado Node.js para que el proyecto se pueda compilar y probar en tu equipo.
- has creado una copia propia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio y te familiarizarás con el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicia-una-sesion-desde-la-incidencia
