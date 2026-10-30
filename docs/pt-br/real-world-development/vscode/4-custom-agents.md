---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados, adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes por meio de arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de código e conhecimento específico do domínio que o orientam a executar determinados tipos de trabalho com mais eficácia. As equipes podem codificar sua experiência em agentes reutilizáveis: um agente de acessibilidade que garante a conformidade com as [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem um frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt em Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados em comparação com skills de agente

Existe uma sobreposição lógica entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente por arquivos Markdown e dizem a uma IA como executar operações. A forma mais clara de separá-los: um **agente personalizado** é quem trabalha, e as **skills** são as ferramentas.

Os agentes personalizados têm a própria janela de contexto e foram criados para orquestrar skills (e até outros agentes) como parte do trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade; nesse trabalho, ele poderia chamar skills como uma skill de fluxo de pull request ou uma que executa e gerencia testes.

> [!NOTE]
> Não existe uma única maneira "certa" de criar um agente personalizado. Como em qualquer coisa relacionada à IA, teste e itere para descobrir o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está empenhada em garantir que sua plataforma de financiamento coletivo seja acessível a todos os usuários, independentemente das habilidades ou preferências visuais. Comentários recentes dos usuários destacaram que algumas pessoas acham o tema escuro atual difícil de ler devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para tratar essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, você quer garantir que isso seja implementado o mais rápido possível. Você usará um agente personalizado para gerar a funcionalidade.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao seu codespace, abra um terminal e mude para uma nova branch a partir da `main` para o trabalho de acessibilidade (você manterá o PR de filtragem do Exercício 3 separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. A partir daí, percorra e revise as próximas seções, que destacam:
   - As responsabilidades principais ao gerar código para um site acessível.
   - Práticas recomendadas de acessibilidade.
   - Exemplos de código em HTML, CSS e JavaScript.
   - Uma lista de armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da visualização do Copilot Chat. Você pode selecionar um agente personalizado para limitar uma sessão de chat às instruções e às ferramentas desse agente.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e de modelo varia por exercício; cada etapa indica isso quando for importante.

1. Selecione **Agent** na lista suspensa de agentes na visualização do Chat, se ainda não estiver selecionado.

   ![Captura de tela do seletor de agentes na visualização do Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista suspensa de agentes na parte inferior da visualização do chat (ela mostra o agente ativo, que por padrão é **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o prompt a seguir ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: ele explorará a base de código, proporá alterações e aplicará edições nos arquivos do seu projeto. Cada edição aparecerá em linha no chat, com o caminho do arquivo e um diff que você pode revisar.
6. À medida que as edições forem aplicadas, o indicador **Files changed** será atualizado para que você veja o conjunto de trabalho que o agente modificou.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no seu repositório: ele editará arquivos existentes, como os componentes Astro, o CSS e quaisquer testes relacionados, enquanto trabalha.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Esta lição explorou os [agentes personalizados][custom-agents] no GitHub Copilot, assistentes de IA especializados, adaptados a tarefas e domínios específicos. Com os agentes personalizados, você pode codificar a experiência e os padrões da sua equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficácia.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- o uso de um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que acontecem e adicionando um alternador de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no seu IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitoramento e gerenciamento de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
