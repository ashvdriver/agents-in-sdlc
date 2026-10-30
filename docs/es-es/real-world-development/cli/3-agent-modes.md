---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para desarrollar el filtrado a partir de una incidencia y el modo Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más grandes exigen un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo existente de una organización, de modo que se desarrolle lo correcto de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso típico de desarrollo dirigido por agentes: partirás de una incidencia para generar una funcionalidad nueva, comprobarás que el código es válido y que la funcionalidad se comporta como se espera, y, por último, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma conversación y la misma rama a lo largo del flujo de la funcionalidad. Lo habitual sería tener ramas o solicitudes de incorporación de cambios distintas para cada tipo de archivo, pero tomaremos un atajo para centrarnos en los conceptos principales.

Para empezar, en esta lección:

- iniciarás una conversación nueva con Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en modo Plan.
- implementarás la nueva funcionalidad en modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador con reenvío de puertos.

A medida que avances con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás la validación con MCP, crearás un agente de control de calidad (QA) y abrirás la solicitud de incorporación de cambios de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan acotar los juegos por categoría y por editor. La incidencia de la lista de trabajo pendiente describe la funcionalidad, pero hay detalles, como la combinación de categorías, que conviene acordar antes de programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA a tu flujo de desarrollo no cambia los fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un flujo parecido a este:

1. Partir de una incidencia registrada que describe lo que hay que hacer.
2. Crear un plan de lo que se va a desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando el código se haya revisado y el proceso de integración continua finalice correctamente, combinar el código.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría son variaciones de este esquema.

Si te ciñes a este enfoque estándar, te aseguras de que el código generado por IA cumple los requisitos establecidos y pasa por el mismo proceso de verificación que el código escrito a mano.

## Modos de conversación

El **modo de conversación** controla cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre modos:

- **Interactive**: tú y el agente trabajáis conjuntamente. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en modo Plan, revisa el plan y después usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar con el filtrado, vuelve a tu codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de las valoraciones por estrellas está combinada.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Con las teclas de flecha, resalta la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la indicación y volver a la pestaña **Session**.

Fíjate en que la indicación ahora empieza por `#7` (o un número similar). El carácter `#` permite incorporar al contexto una incidencia o una solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir el enfoque para implementar una funcionalidad o realizar tareas antes de pasar el trabajo a Copilot. Siempre es buena idea dedicar un tiempo a planificar cualquier tarea compleja. Vamos a cambiar al modo Plan y a pedir a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador de modo situado debajo de la indicación muestra **Plan**.
2. A continuación de la referencia a la incidencia añadida en el paso anterior, escribe la indicación siguiente:

   ```plaintext
   Create a plan for implementing this feature.
   ```

¡Copilot se pone manos a la obra con el plan! Empezará explorando el proyecto y después determinará el mejor enfoque.

3. Durante el proceso, Copilot puede hacerte preguntas sobre cómo deben funcionar las capacidades de filtrado. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando el plan esté completo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate hacia arriba y hacia abajo para revisar el plan.
6. Pide a Copilot que modifique cualquier parte del plan que no coincida con tus decisiones.

## Aprueba Autopilot

Con el plan escrito y revisado, es el momento de implementarlo. Dejemos que Copilot haga su trabajo con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)**, o la opción con una etiqueta similar en la versión que tengas instalada.
2. Confirma que el indicador de modo situado debajo de la indicación muestra **Autopilot**.
3. Observa cómo Copilot avanza por el plan establecido, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa antes el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas.

## Revisa y verifica la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Vamos a revisar el código y a ejecutar el sitio para asegurarnos de que todo es correcto.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa el resultado de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Iniciemos la aplicación como hicimos antes y abramos el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editor y las combinaciones que acordaste en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y con tus decisiones.
5. Vuelve a tu codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y pasos siguientes

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva con Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en modo Plan.
- has implementado la nueva funcionalidad en modo Autopilot.
- has revisado el código.
- has validado la funcionalidad manualmente en un navegador con reenvío de puertos.

A continuación, profundicemos un poco más en cómo se genera el código y en cómo asegurar que sigue las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
