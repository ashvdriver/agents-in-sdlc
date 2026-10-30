---
title: "Ejercicio 2 - Añadir funcionalidad nueva con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, tu flujo habitual consiste en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo se ha pasado por alto, hacer otra actualización... y así sucesivamente.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para actuar con más autonomía en tu IDE. Se comporta de forma similar a un desarrollador: empieza explorando la estructura del proyecto existente, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Vamos a explorar cómo puedes usar el modo agente para introducir funcionalidad nueva en tu sitio.

> [!NOTE]
> Aunque los nombres son parecidos, el modo agente y el agente en la nube están pensados para dos tipos de experiencias distintas. El modo agente realiza sus tareas en tu IDE, lo que permite ciclos de respuesta rápidos e interacción. El agente en la nube está diseñado como un compañero de programación que trabaja de forma asíncrona como un miembro más del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar tu proyecto, identificar los archivos relevantes y realizar cambios coordinados.
- el modo agente de GitHub Copilot puede implementar funcionalidades nuevas en la interfaz de usuario y en la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de incorporarlos a tu base de código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y por editor. En el ejercicio anterior ya añadiste un asistente de editores; ahora terminarás con el modo agente de Copilot el resto del trabajo de la capa de datos, de la interfaz de usuario y de las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, vamos a explorar el sitio web de Tailspin Toys para conocer su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite a los usuarios publicar juegos y ver sus detalles. Es una única aplicación de Astro que representa sus páginas como HTML estático en tiempo de compilación. Las páginas consultan directamente una base de datos SQLite local en su frontmatter mediante Drizzle ORM: no hay una API de back-end independiente ni un marco de interfaz de usuario en el cliente. Los asistentes reutilizables de acceso a datos residen en `src/lib/`, y cualquier interactividad se añade con un pequeño `<script>` de Astro de ámbito limitado que usa las API estándar del DOM.

### Iniciar el sitio web

Para facilitar la ejecución del sitio web, se ha proporcionado una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu codespace de GitHub con estos pasos:

1. Vuelve a tu codespace. Deberías estar de nuevo en `main` tras el ejercicio anterior.
2. Abre una ventana de terminal nueva dentro del codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el comando siguiente para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, deberías ver un banner con la URL, similar al siguiente:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo que te propone abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la dirección del servidor de Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> Cuando se usa un codespace, al seleccionar un vínculo con la URL localhost desde el terminal del codespace se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu codespace, que ahora aloja tu servidor web.

### Explorar el sitio web

Con el sitio web en ejecución, puedes explorar su funcionalidad. Las funciones principales del sitio son:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accedes a una página de detalles con más información sobre él, como su título, descripción, editor y categoría.

## Explora la lista de trabajo pendiente con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y de modelo varía según el ejercicio; cada paso lo indica cuando es relevante.

La implementación inicial del sitio web es funcional, pero queremos mejorarla añadiendo nuevas capacidades. Empecemos revisando la lista de trabajo pendiente. Cuando configuraste el proyecto a partir de la plantilla, se creó automáticamente una lista de incidencias; pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat. El agente **Agent** planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no manifiestan preferencia por ningún modelo. Al crear el taller se utilizó Claude Sonnet 4.5, por lo que se incluye en las instrucciones. La intención es que las sugerencias de código que recibas sean relativamente coherentes para garantizar una buena experiencia. Sin embargo, como los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, es posible que las sugerencias difieran de las que se indican en el taller. Es algo perfectamente normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, es posible que Copilot utilice un comando de MCP distinto, pero debería poder completar la tarea igualmente.

3. Pregunta a Copilot por la lista de incidencias pendientes enviándole la indicación siguiente:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa cómo Copilot incluso ha priorizado los elementos por ti, según los que considera más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de poner en marcha el agente para generar el código, conviene revisar el archivo de instrucciones que usarás para proporcionar contexto a Copilot en su trabajo. Aprovecharás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contiene contexto sobre cómo abordar la adición de funcionalidad al sitio web.

1. En tu codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en la guía general sobre cómo abordar la adición de funcionalidad. Incluye:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Vínculos a archivos de instrucciones específicos para distintos tipos de archivo, entre ellos:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a archivos de tu proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite la reutilización, dividir instrucciones complejas en fragmentos más manejables y proporcionar ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado hay que realizar, como mínimo, tres actualizaciones distintas en la aplicación:

- Añadir o refinar la lógica de filtrado restante en la capa de datos (`src/lib/`)
- Añadir o actualizar las pruebas del comportamiento del filtrado
- Actualizar la página de listado de juegos para introducir la interfaz de filtrado

Además, las pruebas deben ejecutarse (y superarse) antes de incorporar todo a tu base de código. El modo agente de Copilot puede realizar estas tareas por ti. Vamos a añadir la funcionalidad.

1. Puedes continuar en la conversación actual con Copilot o iniciar una nueva seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla con un ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en la lista desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de tu lista de trabajo pendiente con la indicación siguiente:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot empieza explorando el proyecto y localizando los archivos asociados a la funcionalidad deseada. Deberías verlo encontrar tanto los asistentes de la capa de datos y la interfaz de usuario como las pruebas. Después empieza a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Verás que Copilot realiza varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos según la complejidad de la tarea y de la base de código. Durante ese proceso, es posible que aparezcan los botones **Keep** y **Undo** en el editor de código. Cuando Copilot termine, tendrás un **Keep** o un **Undo** para todos los cambios, por lo que no es necesario seleccionarlos mientras el trabajo está en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel de Copilot Chat que pide al usuario confirmar que está de acuerdo con ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que se produzcan algunas pausas e incluso que fallen algunas pruebas durante el proceso. No pasa nada. Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión de chat completa con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado en busca de posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que generan Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio web en ejecución. ¡Explora la nueva funcionalidad!
11. Cuando hayas confirmado que todo funciona y hayas revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio hemos explorado cómo usar el modo agente de GitHub Copilot para añadir nuevas capacidades al sitio web de Tailspin Toys. Hemos aprendido cómo:

- el modo agente de GitHub Copilot puede implementar funcionalidades nuevas en la interfaz de usuario y en la capa de datos.
- el modo agente de Copilot puede explorar tu proyecto, identificar los archivos relevantes y realizar cambios coordinados.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de incorporarlos a tu base de código.

Ahora [probemos tu funcionalidad con el servidor MCP de Playwright][next-lesson] y abramos una solicitud de incorporación de cambios para ella.

### Ejercicio adicional de exploración: implementar la paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Con las habilidades que has aprendido en este ejercicio, pide a Copilot que actualice el sitio para implementar la paginación. Algunas consideraciones para el código:

- sigue las prácticas recomendadas existentes, incluido el uso de los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: si el usuario puede elegir el tamaño de página o si va fijado en el código.
- al crear la indicación, proporciona a Copilot la guía necesaria para crear la implementación como deseas.
- puede que tengas que iterar con GitHub Copilot, pidiendo cambios y aportando contexto. Es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
