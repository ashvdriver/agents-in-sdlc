---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Lección siguiente: modo agente →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, o hay información de fondo que Copilot debería conocer, conviene asegurarse de que ese contexto esté a su alcance. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que sirven para proporcionar esa guía sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta,
- generarás la primera parte de los datos para el filtrado (un asistente de editores) con las instrucciones *actuales*,
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`,
- volverás a ejecutar la misma indicación y comprobarás cómo el código generado adopta el nuevo estándar,
- confirmarás en `main` la actualización de las instrucciones y la parte del filtrado para que Copilot pueda usar la guía actualizada en el ejercicio siguiente.

> [!CAUTION]
> Es posible que el código generado se aparte de algunos de los estándares que establezcas, porque Copilot no es determinista. El objetivo de este ejercicio es ver la *tendencia* del cambio de comportamiento tras actualizar las instrucciones, no igualar el resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys dispone de un conjunto de directrices y requisitos para las prácticas de desarrollo. Entre ellas:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe estar en modo oscuro y tener un aspecto moderno.
- La documentación debe añadirse al código en forma de comentarios de documentación TSDoc.
- Debe añadirse un bloque de comentarios al principio de cada archivo que describa lo que hace.

Mediante los archivos de instrucciones te asegurarás de que Copilot dispone de la información adecuada para realizar las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que comprenda mejor tu estilo de programación y tus requisitos. Es una funcionalidad potente que ayuda a orientar a Copilot para obtener sugerencias y fragmentos de código más relevantes. Puedes especificar tus convenciones de programación preferidas, las bibliotecas e incluso el tipo de comentarios que te gusta incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos, a fin de dar contexto a nivel de tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto, es decir, contexto relevante para la mayoría de las solicitudes de chat o de CLI enviadas a Copilot. Puede incluir la pila tecnológica utilizada, una descripción general de lo que se está desarrollando, prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` pueden crearse para tareas o tipos de archivo concretos. Puedes usarlos para proporcionar directrices para lenguajes determinados (como TypeScript o Astro) o para tareas como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> Al trabajar en tu IDE, los archivos de instrucciones solo se usan para la generación de código en Copilot Chat, no para las finalizaciones de código ni para las sugerencias de la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto los archivos a nivel de repositorio como los archivos `*.instructions.md` (con el frontmatter `applyTo`) al generar código.
>
> Por último, Copilot [admite archivos de instrucciones que siguen otros estándares][custom-instructions-support], como los archivos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. No obstante, los ejemplos del proyecto de muestra reflejan un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en la guía a nivel de proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares de programación globales.
- Usa archivos `*.instructions.md` para dar instrucciones específicas para tipos de archivo (pruebas unitarias, componentes de Astro, capa de datos) o para tareas concretas.
- Usa lenguaje natural. Mantén la guía clara. Incluye ejemplos de cómo debe (y cómo no debe) ser el código.

No existe una única forma de crear archivos de instrucciones, igual que no existe una única forma de usar la IA. Descubrirás mediante la experimentación qué funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, verás que hay archivos para numerosos tipos de tareas, incluidas las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada superficie lo expone de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección de la superficie que estés usando lo indicará cuando sea relevante.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio lleno de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para diversas tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa la guía anidada **GitHub Actions Workflows**. Todo ello es aplicable a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y echa un vistazo. Observa que hay instrucciones para archivos de Astro, la capa de datos de Drizzle, las pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. Aquí, cualquier archivo de pruebas de TypeScript (por ejemplo, uno que coincida con `**/*.test.ts`) coincidirá.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los vínculos a otros archivos de instrucciones (como `unit-tests.instructions.md`) y a archivos existentes del proyecto. Esto permite dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y dirigir a Copilot hacia ejemplos que seguir al generar código. (Las rutas que aparecen allí son relativas al archivo de instrucciones y no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y las cabeceras de comentarios de archivo.

## Crea una rama para los cambios

Sigamos las prácticas recomendadas y creemos una rama para nuestros cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y de modelo varía según el ejercicio; cada paso lo indica cuando es relevante.

Para ver el efecto de las instrucciones personalizadas, empieza enviando una indicación con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar la misma indicación para ver la diferencia.

1. Cierra las pestañas del editor que sigan abiertas de ejercicios anteriores para que Copilot reciba solo el contexto que quieres.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ubicarse el asistente.
3. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat para que Copilot pueda aplicar cambios en los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía la indicación siguiente:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica actualizaciones de código, que a menudo abarcan el archivo del asistente y sus pruebas.
6. Observa que el asistente propuesto es una función con tipos que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observa que en el código propuesto **faltan** los comentarios de documentación TSDoc y una cabecera de comentarios a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, es posible que añada comentarios de documentación aunque no se le haya indicado. Si ocurre, no pasa nada: lo importante sigue siendo la mejora de *coherencia* tras actualizar las instrucciones.

## Añade un nuevo estándar de repositorio

Como se ha indicado, `.github/copilot-instructions.md` está pensado para proporcionar información a nivel de proyecto a Copilot. Asegurémonos de que los estándares de programación del repositorio estén documentados para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto, pero todavía no tiene ninguna regla de documentación en el código, y por eso el asistente generado no tenía comentarios de documentación.
3. Añade las líneas de Markdown siguientes justo debajo de los estándares existentes para indicar a Copilot que añada cabeceras de comentarios de archivo y comentarios de documentación TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, los archivos de instrucciones pueden crearse a nivel de repositorio (`.github/copilot-instructions.md`) para una guía global, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo a nivel de repositorio es el lugar adecuado para estándares de todo el proyecto, como la regla de comentarios de documentación que acabas de añadir.

## Vuelve a ejecutar la indicación y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar el búfer.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía **la misma indicación** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real sobre el que se construirá el ejercicio siguiente.

## Confirma las instrucciones y envía la rama

Los archivos de instrucciones son como cualquier otro recurso del repositorio, es decir, se gestionan con el mismo enfoque de control de código fuente que usarías con cualquier otro elemento. Así que vamos a confirmar los cambios y a enviar la rama a nuestro repositorio.

1. Abre una ventana de terminal nueva pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. En el terminal, comprueba que la actualización de las instrucciones y los cambios del asistente están presentes ejecutando:

   ```bash
   git status
   ```

3. En el terminal, prepara y confirma la actualización de las instrucciones y todos los archivos que Copilot haya cambiado para la base del asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. En el terminal, envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama enviada, debemos crear una solicitud de incorporación de cambios y vincularla a la incidencia sobre el estándar de documentación de tu lista de trabajo pendiente. Podríamos hacerlo manualmente, pero Copilot puede hacerlo por nosotros con las herramientas de GitHub que ya están conectadas a tu proyecto. Pidamos a Copilot que localice la incidencia, cree la solicitud que la cierre y la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub por ti. La plantilla del proyecto ya conecta las herramientas de GitHub a tu espacio de trabajo, así que no hay nada que configurar; la primera vez que Copilot use una, VS Code puede pedirte que inicies sesión en GitHub. Sigue las indicaciones para permitirlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat dentro de tu codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y asegúrate de que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que localice la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una solicitud de incorporación de cambios desde la rama actual que describa tanto las actualizaciones de las instrucciones como la nueva base del asistente de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y a crear la solicitud.
5. Cuando se te pida **Allow** para que Copilot realice acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si procede.
6. Cuando se haya creado la solicitud, pide a Copilot que la combine y que devuelva tu rama a main con la indicación siguiente:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando se te pida **Allow** para que Copilot realice acciones de GitHub y ejecute comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con la ayuda de GitHub Copilot!

## Resumen y pasos siguientes

Has explorado cómo recoge Copilot el contexto de los archivos de instrucciones de este proyecto y, después, has usado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar lo que produce Copilot con las instrucciones *existentes*,
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`,
- volver a ejecutar la misma indicación y comprobar cómo el código propuesto adopta el nuevo estándar,
- confirmar en `main` la actualización de las instrucciones y la base del asistente para que el ejercicio siguiente pueda partir de ellas.

A continuación, pondrás esas instrucciones en práctica en el [modo agente][next-lesson], mientras Copilot añade una funcionalidad nueva en toda la base de código.

## Recursos

- [Archivos de instrucciones para la personalización de GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
