---
title: "Lição 10 - Conclusão e próximos passos"
description: "Recapitule o fluxo do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo da Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem pré-criada.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez manualmente o merge do seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, criou o recurso no modo Autopilot e o revisou no modo Interactive.
- orientou o agente com instruções personalizadas, depois personalizou a skill `quality-checks` existente e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor MCP (Model Context Protocol) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou a alteração completa de filtragem e autorizou o Agent Merge para o PR de filtragem.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch a partir da `main` atualizada:

1. **Avaliações por estrelas:** exibir o `starRating` existente e um estado explícito de sem avaliação nos cards dos jogos.
2. **Fluxo de filtragem e qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório do `quality-checks`, criar um perfil de QA e incluir os testes associados.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e a mesma branch. Combinamos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, sua própria inspeção no navegador e a exploração do navegador feita pelo Copilot por meio do MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e os relatou no seu novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes antes do PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não precisa de novos testes pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de alterações.

## Práticas recomendadas

O contexto e as ferramentas que você fornece ao Copilot moldam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da sua equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Combine o **modo e o modelo** com a tarefa. Use o **Plan** para refletir sobre uma abordagem antes de construir, o **Interactive** para acompanhar de perto alterações focadas e o **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua importando tanto quanto a infraestrutura. Descrever com clareza *o que* você quer construir, *por quê* e *como* altera significativamente o resultado.

## Mais para explorar

Você cobriu o fluxo principal. Alguns outros recursos da CLI que valem uma olhada:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para orquestrar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa separada.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a! Use-a em código de produção, em código de hobby, no aplicativinho que você tem em mente há anos, mas nunca chegou a criar. Compartilhe o que aprendeu com sua equipe e aprenda com o que ela descobriu. E, como sempre, explore a documentação.

Se quiser estender a Tailspin Toys pelo terminal, continue com a [série opcional do Backer Concierge com o Foundry][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
