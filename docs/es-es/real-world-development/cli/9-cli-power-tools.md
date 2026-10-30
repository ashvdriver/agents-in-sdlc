---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora los comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de línea de comandos, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos dan acceso a funcionalidad avanzada, a información interna y a opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar algunos más que resultan útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para explorar los modelos disponibles.
- aprenderás cómo `/share` puede exportar o compartir una sesión.
- explorarás comandos opcionales para trabajo en paralelo, árboles de trabajo y delegación en el agente en la nube.

## Escenario

Has completado el flujo principal de la CLI. Ahora veamos algunas capacidades adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y, opcionalmente, delegar trabajo en el [agente en la nube de Copilot][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas más grandes o complejas, puedes alcanzar la ventana de contexto máxima del modelo. Copilot CLI compacta la conversación automáticamente cuando es necesario, y tú también puedes inspeccionarla o compactarla con comandos de barra.

1. Vuelve a tu codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, en el uso actual de tokens y en cómo se reparte el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` de nuevo y compara el resultado. Puede que no haya un cambio drástico si la conversación ya es pequeña.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` cuando quieras elegir el momento. Usa `/clear` o `/new` cuando cambies a una tarea no relacionada y quieras una conversación nueva.

## Elige tu modelo

Cada modelo tiene sus puntos fuertes y cada desarrollador sus preferencias. Copilot CLI permite enumerar y seleccionar el modelo que quieres usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Trabajar en equipo y compartir lo aprendido ayuda a todos a mejorar su uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo de Markdown o HTML, crear un vínculo para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión que tengas instalada.
2. Si quieres compartir esta sesión, elige el destino que más te convenga, como `/share file` para una exportación local en Markdown.
3. Revisa el contenido exportado antes de enviarlo a alguien o de publicarlo. Las exportaciones de sesión pueden incluir indicaciones, respuestas y detalles del proyecto.

Publicar un vínculo o un gist es opcional. No publiques contenido del repositorio ni de la conversación que tu equipo no tenga intención de compartir.

## Opcional: escalar o delegar

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea independiente.
- `/delegate` puede enviar una tarea al agente en la nube de Copilot, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Inicia una tarea nueva y bien acotada antes de probarlos y revisa el resultado con tu flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Los comandos de barra de Copilot CLI permiten configurarla, compartir sesiones y ver lo que ocurre internamente. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajo en paralelo, árboles de trabajo y delegación en el agente en la nube.

Hay más comandos de barra disponibles y mucho más que explorar con Copilot CLI. Cerremos nuestro recorrido [repasando lo aprendido][next-lesson] y con algunos pasos siguientes para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente en la nube de Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
