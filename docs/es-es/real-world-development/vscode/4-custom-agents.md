---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados, adaptados a tareas o dominios concretos de tu flujo de desarrollo. Al definir agentes mediante archivos de Markdown en la carpeta `.github/agents` de tu repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimiento del dominio que lo guíen para realizar con mayor eficacia determinados tipos de trabajo. Los equipos pueden codificar su experiencia en agentes reutilizables: un agente de accesibilidad que exija el cumplimiento de las [WCAG][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones de prueba coherentes.

Los agentes personalizados se definen mediante archivos de Markdown en la carpeta `.github/agents` de tu proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con al menos un `name` y una `description`, seguido de una indicación en Markdown que define el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados frente a habilidades de agente

Entre los agentes personalizados y las [habilidades de agente][agent-skills-concept] hay cierto solapamiento lógico. Ambos se definen principalmente con archivos de Markdown e indican a una IA cómo realizar operaciones. La forma más clara de distinguirlos es esta: un **agente personalizado** es quien trabaja, y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están pensados para orquestar habilidades (e incluso otros agentes) durante su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; como parte de ese trabajo, podría llamar a habilidades como una de flujo de trabajo de solicitudes de incorporación de cambios o una que ejecute y gestione pruebas.

> [!NOTE]
> No existe una única forma "correcta" de crear un agente personalizado. Como con todo lo relacionado con la IA, prueba e itera para encontrar lo que funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se ha comprometido a que su plataforma de financiación colectiva sea accesible para todos los usuarios, con independencia de sus capacidades visuales o preferencias. Los comentarios recientes de los usuarios han puesto de manifiesto que a algunos les resulta difícil leer el tema oscuro actual por el contraste insuficiente entre el texto y los colores de fondo. Para abordar este problema de accesibilidad, el equipo de diseño ha solicitado implementar un modo de alto contraste que los usuarios puedan activar y desactivar.

Como la accesibilidad es fundamental, quieres asegurarte de que se implemente lo antes posible. Vas a utilizar un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- usarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado para ti un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace, abre un terminal y cámbiate a una rama nueva a partir de `main` para el trabajo de accesibilidad (mantendrás separada la PR de filtrado del Ejercicio 3):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Fíjate en el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio en los agentes personalizados.

3. A continuación, examina las secciones siguientes, que recogen:
   - Las responsabilidades principales al generar código para un sitio web accesible.
   - Prácticas recomendadas de accesibilidad.
   - Ejemplos de código para HTML, CSS y JavaScript.
   - Una lista de errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en la lista desplegable de agentes situada en la parte inferior de la vista de Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a las instrucciones y herramientas de ese agente.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y de modelo varía según el ejercicio; cada paso lo indica cuando es relevante.

1. Selecciona **Agent** en la lista desplegable de agentes de la vista Chat si todavía no está seleccionado.

   ![Captura de pantalla del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona la lista desplegable de agentes situada en la parte inferior de la vista de chat (muestra el agente activo; por defecto, **default**).
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía la indicación siguiente al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat se pondrá manos a la obra: explorará la base de código, propondrá cambios y aplicará ediciones a los archivos de tu proyecto. Cada edición aparecerá en línea en el chat con la ruta del archivo y unas diferencias que puedes revisar.
6. A medida que se apliquen las ediciones, el indicador **Files changed** se actualizará para que veas el conjunto de archivos que ha modificado el agente.

> [!NOTE]
> Este proceso probablemente tardará unos minutos. Copilot está haciendo cambios reales en tu repositorio: editará archivos existentes, como los componentes de Astro, el CSS y las pruebas relacionadas, a medida que trabaja.

Revisarás y dirigirás este trabajo en curso en el ejercicio siguiente.

## Resumen y pasos siguientes

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados y adaptados a tareas y dominios concretos. Con los agentes personalizados puedes codificar la experiencia y los estándares de tu equipo en agentes reutilizables que guían a Copilot para realizar con mayor eficacia determinados tipos de trabajo.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- el uso de un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se producen y añadirás un conmutador de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en tu IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar tu funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Lección siguiente: Supervisión y gestión de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
