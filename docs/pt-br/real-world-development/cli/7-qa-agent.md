---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combina os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem em um navegador. Agora você reunirá esses recursos em um agente personalizado com um processo de QA claramente definido.

Nesta lição, você vai:

- explorar como um agente personalizado funciona com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as conclusões dele em relação à issue de filtragem.

## Cenário

A Tailspin Toys quer uma revisão consistente de requisitos, qualidade do código, verificações automatizadas, cobertura de testes e comportamento no navegador antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis do agente. Neste workshop, você definirá uma função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações que você usou têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` empacota verificações repetíveis. O MCP do Playwright fornece as ferramentas de navegador. O perfil de QA diz ao Copilot como usar esses recursos para avaliar requisitos e relatar conclusões. Ele não os substitui nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir o PR do recurso, você pedirá ao Copilot que crie um perfil de QA reutilizável. O perfil definirá tanto as verificações que o QA executa quanto os limites que ele deve respeitar.

1. Volte ao seu Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecionar o perfil

Antes de usar o novo agente, revise o perfil dele para confirmar que o Copilot capturou o fluxo de QA e os limites pretendidos. Isso evita que um agente incompleto ou amplo demais altere o recurso quando você quer apenas verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. O campo `description` é obrigatório; `name` é opcional, mas incluí-lo dá ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme que o QA parte dos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme que o QA relata evidências de apoio, pergunta antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não contemplar alguma dessas responsabilidades ou desses limites, peça ao agente padrão que o revise antes de continuar.

## Executar o QA em relação à issue

O Copilot CLI carrega os agentes do projeto quando uma conversa é iniciada. Retome a mesma conversa de filtragem depois de criar o perfil e, em seguida, selecione o QA para que ele use a issue e as decisões de planejamento que já estão no contexto.

1. Ative o agente usando o prompt a seguir:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, ele pode não aparecer na lista de agentes disponíveis. Ele existe, e o comando acima o ativará.

2. Use o prompt a seguir para pedir ao agente de QA que revise o recurso:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. O agente de QA começa a trabalhar!
4. Leia o relatório que ele fornece quando terminar o trabalho!

## Resumo e próximos passos

Você adicionou ao fluxo uma função especializada reutilizável e revisou o trabalho dela. Nesta lição, você:

- explorou como um agente personalizado funciona com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as conclusões dele em relação à issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você [reunirá tudo em um PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
