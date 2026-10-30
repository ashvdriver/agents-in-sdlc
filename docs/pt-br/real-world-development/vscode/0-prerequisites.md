---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, você precisa deixar tudo pronto. Você criará sua própria cópia do repositório Tailspin Toys, iniciará um [codespace][codespaces] para trabalhar e confirmará que o GitHub Copilot Chat está instalado e funcionando no seu editor.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório para o código que você desenvolverá, crie uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório, e você a usará ao longo dos exercícios.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver fazendo o workshop como parte de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções fornecidas pelas pessoas mentoras. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Preencha as configurações do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que você criou (**organization-or-user-name/repository-name**), pois você o usará mais adiante no laboratório.

> [!NOTE]
> **Seu backlog está pronto**
>
> Quando você cria seu repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente para você. Você trabalhará a partir dessas issues ao longo do workshop, então não há nada para registrar por conta própria.

## Criar um codespace

Em seguida, você usará um codespace para concluir os exercícios do laboratório.

Os [GitHub Codespaces][codespaces] são um ambiente de desenvolvimento baseado na nuvem que permite escrever, executar e depurar código diretamente no navegador. Eles oferecem um IDE completo, com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione o botão verde **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, depois, o botão **+** para criar um novo Codespace.

    ![Crie um novo codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace levará vários minutos, embora ainda seja muito mais rápida do que instalar manualmente todos os serviços! Mesmo assim, você pode aproveitar esse tempo para explorar outros recursos do GitHub Copilot, que veremos a seguir.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado dentro de um codespace ou de um [dev container][dev-containers] local. Ambos garantem que o ambiente tenha todos os pré-requisitos necessários instalados para uma experiência tranquila. Se preferir executá-lo localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo dev container usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, é preciso ter a extensão GitHub Copilot Chat instalada no IDE, o que já deve ser o caso se você estiver usando um GitHub Codespace.

> [!TIP]
> Se você não tem a extensão GitHub Copilot Chat instalada, pode [instalá-la pelo Visual Studio Code Marketplace][copilot-chat-extension]. Ou abra a visualização Extensions no Visual Studio Code, pesquise por **GitHub Copilot Chat** e selecione **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar-se com sua conta do GitHub para habilitá-la.

1. Volte ao seu codespace.
2. Se você ainda não vir o Copilot Chat no lado direito do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem como "Hello world" na janela do Copilot Chat e pressione Enter. Isso deve ativar o Copilot Chat.
4. Como alternativa, se você não estiver autenticado, será solicitado que entre na sua conta do GitHub. Siga as instruções para se autenticar.

    ![Exemplo do prompt de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Após a autenticação, você deve ver a janela do Copilot Chat.

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório! Você também iniciou a criação do seu codespace, que usará quando começar a escrever código.

## Próximo passo

Vamos começar a colocar o Copilot para trabalhar. Continue para o [Exercício 1 - Instruções personalizadas][next-lesson], em que você ensinará ao Copilot as convenções do seu projeto.

## Recursos

- [Visão geral dos GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução aos Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
