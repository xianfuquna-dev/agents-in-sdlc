---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información de fondo, ese contexto debe estar a su alcance. Los [archivos de instrucciones][instruction-files] proporcionan indicaciones para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones del repositorio y los archivos aplicables a rutas concretas.
- añadirás un estándar general nuevo a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones cuando revises las solicitudes de incorporación de cambios que genere en los ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su función.

Con los archivos de instrucciones, proporcionarás a Copilot la información necesaria para que sus tareas sigan estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor el estilo de programación y los requisitos. Permiten definir convenciones, bibliotecas e incluso el tipo de comentarios que se prefieren. Puedes crear instrucciones para todo el repositorio o para determinados tipos de archivo.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, buenas prácticas y otras pautas globales.
- `.github/instructions/*.instructions.md`, archivos que ofrecen pautas para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, pero no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo:

- Limita `copilot-instructions.md` a las pautas generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas concretas.
- Escribe en lenguaje natural. Expón las pautas con claridad e incluye ejemplos de cómo debería (y cómo no debería) ser el código.

No hay una única manera correcta de crear archivos de instrucciones ni de usar la IA. La experimentación permitirá encontrar lo que funciona mejor para cada proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. En este proyecto encontrarás archivos para distintos tipos de tareas, como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente lo indicará cuando sea pertinente.
>
> Para buscar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en el repositorio: el archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas distintas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`, que contiene instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` define un patrón glob relativo a la raíz del repositorio; por ejemplo, coincide con archivos de prueba TypeScript como `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Contiene enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así se pueden dividir instrucciones amplias en archivos reutilizables más pequeños y enlazar ejemplos (las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero todavía no exige documentación dentro del código. A continuación, añadirás reglas para comentarios TSDoc y encabezados de comentario en los archivos.

## Añade un estándar para todo el repositorio

Solo editarás un archivo en este ejercicio: añadirás una regla general para que la documentación se incluya en el código mediante comentarios TSDoc y encabezados de archivo. El agente en la nube aplicará esta regla a las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero así también confirmarás los cambios de forma ordenada.

1. Desde el terminal del codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade a las pautas de programación los estándares necesarios.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, cerca de la línea 27. Aún no incluye reglas de documentación en el código, por eso el helper generado no tenía comentarios.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden aplicarse a todo el repositorio (`.github/copilot-instructions.md`) o a lenguajes, tipos de archivo y tareas concretos (`*.instructions.md`). El archivo del repositorio es el lugar adecuado para estándares globales como la regla de comentarios que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. En el siguiente ejercicio, Copilot partirá de `main`; por tanto, los cambios deben llegar a `main` para que el agente los aplique.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así el agente en la nube podrá leer las instrucciones cuando trabaje en incidencias que le asignes.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar el cambio directamente en `main`. La rama permite practicar un flujo de trabajo más parecido al de un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo obtiene contexto Copilot de los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las PR que genere el agente en la nube durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para redactar mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Awesome Copilot: recopilación de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
