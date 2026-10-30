---
title: "Exercício 5 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora sua base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code oferece uma visão contínua de cada chamada de ferramenta e de cada edição de arquivo à medida que acontecem. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um alternador de modo claro junto com o alto contraste.

## Cenário

O agente personalizado de acessibilidade que você usou no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design voltou com uma solicitação adicional: eles também gostariam de um alternador de modo claro para os usuários que acham o tema padrão escuro demais. Em vez de iniciar uma nova conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Monitorar o progresso do Copilot

A visualização do Copilot Chat mostra cada etapa que o agente executa: arquivos que ele explora, comandos que executa e edições que propõe. Cada edição de arquivo aparece em linha no chat, com um diff e os controles **Keep** / **Undo**.

1. Volte à visualização do Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role o chat para acompanhar cada chamada de ferramenta. Você verá leituras de arquivos, edições de arquivos e quaisquer comandos de terminal que ele peça para executar.
3. Quando o agente propõe uma edição de arquivo, o diff aparece em linha:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para reverter a alteração e deixar o agente tentar novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver o conjunto completo de trabalho que o agente modificou durante esta sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove apenas os comandos com os quais você se sente confortável.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Você deve ver uma mensagem final resumindo as alterações feitas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você ampliará a mesma conversa para adicionar um alternador de modo claro. Como você está continuando a mesma sessão de chat, o agente mantém todo o contexto do trabalho anterior, incluindo os componentes que alterou, a abordagem de estilo que usou e o padrão de armazenamento local que configurou.

1. Na entrada do Copilot Chat, envie o prompt de acompanhamento a seguir:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente retomará de onde parou, explorando as alterações que já fez e propondo acréscimos para o modo claro.
3. Revise cada nova edição à medida que ela for aplicada, como você fez para o alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção que você não quer (por exemplo, se tentar refatorar a interface do alternador de uma forma de que você não goste), você pode interrompê-lo com o botão **Stop** ao lado da entrada do chat e, em seguida, enviar uma mensagem de acompanhamento com esclarecimentos.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), corrigir o rumo ("don't add a third toggle, integrate light mode into the existing settings panel") ou ampliar o escopo ("also add keyboard shortcuts for both toggles").

## Revisar o conjunto de trabalho

Antes de fazer o commit do trabalho, faça uma passada rápida por tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deve ver atualizações nos componentes Astro, nos estilos e em quaisquer testes relacionados.
3. Abra alguns dos arquivos alterados e percorra os diffs. Confirme que os padrões de acessibilidade do agente personalizado estão refletidos: atributos ARIA, navegação pelo teclado, HTML semântico e persistência por meio do armazenamento local.
4. Quando estiver satisfeito, faça o stage e o commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento, revisando cada edição de arquivo à medida que era aplicada, aceitando ou rejeitando alterações e enviando prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- o monitoramento do trabalho de um agente em tempo real.
- a aceitação e a rejeição de edições de arquivo individuais.
- a orientação de uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará seu trabalho de acessibilidade como um pull request e revisará as alterações que o agente personalizado local produziu ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
