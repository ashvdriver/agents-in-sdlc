---
title: "Ejercicio 3 - Probar tu funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: modo agente][previous-lesson] · [Lección siguiente: agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, debes confirmar que funciona realmente en el navegador. En lugar de recorrer la aplicación tú mismo, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la funcionalidad por ti; después publicarás tu rama y abrirás la solicitud.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con herramientas nuevas,
- añadirás el servidor MCP de Playwright a tu espacio de trabajo,
- pedirás a Copilot que lo use para probar manualmente en un navegador tu funcionalidad de filtrado,
- publicarás tu rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente resulta mucho más potente cuando puede llegar más allá de tu editor. Model Context Protocol (MCP) es la forma en que Copilot lo consigue: es una manera estándar de que el agente se comunique con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento interno del modo agente y cómo interactúa con el contexto, el LLM y las herramientas, incluidas las herramientas aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) ofrece a los agentes de IA una forma de comunicarse con herramientas y servicios externos. Gracias a MCP, los agentes de IA pueden comunicarse con ellos en tiempo real. Esto les permite acceder a información actualizada (mediante recursos) y realizar acciones en tu nombre (mediante herramientas).

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP se encarga de gestionar la comunicación entre el agente de IA y las herramientas externas (como API existentes o herramientas locales como paquetes de NPM). Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente de IA.

Algunos servidores MCP populares son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: este servidor da acceso a un conjunto de API para gestionar tus repositorios de GitHub. Permite al agente de IA realizar acciones como crear repositorios nuevos, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: este servidor ofrece capacidades de automatización del navegador mediante Playwright. Permite al agente de IA realizar acciones como ir a páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles que dan acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> En cuanto a la seguridad, trata los servidores MCP como cualquier otra dependencia de tu proyecto. Antes de usar un servidor MCP, revisa con atención su código fuente, verifica al editor y valora las implicaciones de seguridad. Usa solo servidores MCP en los que confíes y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Deberías ver un servidor `github` ya configurado:

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

Esta entrada `github` se incluye en la plantilla del proyecto, y por eso Copilot pudo leer tu lista de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], por lo que no hay nada que instalar en local: VS Code se conecta a él mediante HTTP y te autenticas con GitHub la primera vez que Copilot use una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] da a Copilot un navegador que puede controlar, que es justo lo que necesitas para probar tu funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

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

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. La marca `--headless` indica a Playwright que ejecute el navegador sin ventana visible, algo necesario dentro de un codespace, donde no hay un escritorio en el que mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para sus pruebas de extremo a extremo, por lo que el navegador que necesita Playwright suele estar ya instalado. Si más adelante Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` e inténtalo de nuevo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **a demanda**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json` aparece una acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar el servidor ahora y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y elige confiar en él para que pueda iniciarse.

Iniciarlo a mano es opcional: si lo omites, VS Code iniciará el servidor (y te pedirá que confíes en él) la primera vez que Copilot necesite sus herramientas en el paso siguiente.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] da a Copilot un navegador real que puede controlar. En lugar de que tú recorras la aplicación para comprobar tu trabajo, el agente puede abrir una página, navegar, aplicar filtros y devolverte el resultado, y después resumir lo que ha visto. Es la forma más rápida de confirmar que una funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright trabaja a partir del [árbol de accesibilidad][playwright-mcp-server] de la página y no a partir de capturas de pantalla. Esto significa que el agente razona sobre elementos estructurados y etiquetados (botones, vínculos, elementos de lista) igual que lo hacen las tecnologías de asistencia, de modo que una comprobación funcional rápida sirve también como una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que ponga a prueba la funcionalidad de filtrado que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, recorrerá cada paso e informará de lo que ha encontrado. Lee su resumen y compáralo con los criterios de aceptación de la incidencia; si algo parece incorrecto, haz preguntas de seguimiento o devuélvele la tarea para que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para esta prueba. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot use el servidor MCP de Playwright, puede que tenga que descargar un navegador; si indica que falta uno, pídele que ejecute `npx playwright install chromium` e inténtalo de nuevo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Ahora que has confirmado que la funcionalidad es correcta, ya puedes abrir una solicitud de incorporación de cambios (PR) para que tu equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios realizados por Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, entre ellas github.com y la interfaz de línea de comandos (CLI) de GitHub. Pero, como ya trabajas con GitHub Copilot, dejemos que la cree por ti. Puede localizar la incidencia relevante y crear la PR asociada a ella.

1. Ve al panel de Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en la lista desplegable de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR por ti:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot realice las tareas necesarias para recopilar información y ejecutar operaciones. La primera vez que use una herramienta de GitHub, es posible que se te pida autenticarte en GitHub; sigue las indicaciones para permitirlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la adecuada y crea la PR.
6. Selecciona el vínculo generado por Copilot para revisar tu solicitud de incorporación de cambios, pero **no la combines todavía**.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has:

- aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con herramientas nuevas,
- añadido el servidor MCP de Playwright a tu espacio de trabajo,
- usado el servidor para probar manualmente en un navegador tu funcionalidad de filtrado antes de publicarla,
- publicado tu rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las formas de ampliar Copilot. A continuación, [crearemos un agente personalizado][next-lesson] para agilizar tareas concretas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de él?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
