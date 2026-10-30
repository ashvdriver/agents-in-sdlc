---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: modo de agente][previous-lesson] · [Próxima lição: agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, é bom confirmar que ele realmente funciona no navegador. Em vez de percorrer o aplicativo manualmente, você conectará o **servidor MCP do Playwright** e deixará o Copilot controlar um navegador real para testar o recurso por você. Depois, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam o Copilot com novas ferramentas,
- adicionar o servidor MCP do Playwright ao seu workspace,
- pedir ao Copilot que o use para testar manualmente o seu recurso de filtragem em um navegador,
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando consegue ir além do editor. O Model Context Protocol (MCP) é como o Copilot faz isso: é uma forma padrão de o agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e como ele interage com o contexto, o LLM e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) oferece aos agentes de IA uma forma de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Isso permite que acessem informações atualizadas (usando recursos) e executem ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP é responsável por gerenciar a comunicação entre o agente de IA e as ferramentas externas (como APIs existentes ou ferramentas locais, como pacotes NPM). Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP populares já existentes são:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: esse servidor dá acesso a um conjunto de APIs para gerenciar seus repositórios do GitHub. Ele permite que o agente de IA execute ações como criar novos repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: esse servidor fornece recursos de automação de navegador usando o Playwright. Ele permite que o agente de IA execute ações como navegar para páginas da web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis que dão acesso a diferentes ferramentas e recursos. O GitHub hospeda um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições para o ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use apenas servidores MCP em que você confia e tenha cautela ao conceder acesso a recursos ou operações sensíveis.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no seu codespace.
2. Você deve ver um servidor `github` já configurado:

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

Essa entrada `github` acompanha o modelo do projeto, e é por isso que o Copilot conseguiu ler seu backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não há nada para instalar localmente: o VS Code se conecta a ele por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usar uma das ferramentas dele.

## Adicionar o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar seu recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

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

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, o `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` instrui o Playwright a executar o navegador sem janela visível, o que é necessário dentro de um codespace, onde não há área de trabalho para exibi-lo.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador de que o Playwright precisa normalmente já está instalado. Se o Copilot informar mais tarde que falta um navegador, peça que ele execute `npx playwright install chromium` e tente novamente.

## Iniciar o servidor e confiar nele

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme que confia nele.

1. Em `.vscode/mcp.json`, uma ação **Start** aparece acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme que confia no servidor, revise a configuração e escolha confiar nele para que o servidor possa ser iniciado.

Iniciá-lo manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor (e pedirá que você confie nele) na primeira vez que o Copilot precisar das ferramentas dele na próxima etapa.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot um navegador real para controlar. Em vez de você percorrer o aplicativo para verificar seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros e relatar o resultado, resumindo o que viu. É a maneira mais rápida de confirmar que um recurso se comporta como você espera sem sair da conversa.

Por baixo dos panos, o servidor MCP do Playwright trabalha a partir da [árvore de acessibilidade][playwright-mcp-server] da página, e não de capturas de tela. Isso significa que o agente raciocina sobre elementos estruturados e rotulados (botões, links, itens de lista) da mesma forma que as tecnologias assistivas, então uma verificação funcional rápida também funciona como uma verificação leve de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot iniciará um navegador por meio do servidor MCP do Playwright, percorrerá cada etapa e relatará o que encontrou. Leia o resumo dele em relação aos critérios de aceitação da issue; se algo parecer errado, faça perguntas de acompanhamento ou peça que ele corrija o código antes de você abrir um pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você interrompeu o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, ele pode precisar baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que você confirmou que o recurso funciona, está pronto para abrir um pull request (PR) para que sua equipe o revise. A primeira etapa é publicar a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Faça o stage das alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para fazer o push da branch para o seu repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, incluindo pelo github.com e pela interface de linha de comando (CLI) do GitHub. Mas, como você já está trabalhando com o GitHub Copilot, vamos deixar que ele crie o PR para você! Ele pode encontrar a issue relevante e criar o PR associado a ela.

1. Acesse o painel do Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR para você:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot execute as tarefas necessárias para coletar informações e realizar operações. Na primeira vez que ele usar uma ferramenta do GitHub, pode ser solicitado que você se autentique no GitHub. Siga as instruções para permitir.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link gerado pelo Copilot para revisar seu pull request, mas **não faça o merge dele ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam o Copilot com novas ferramentas,
- adicionou o servidor MCP do Playwright ao seu workspace,
- usou-o para testar manualmente o seu recurso de filtragem em um navegador antes de entregá-lo,
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das formas de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [What the heck is MCP and why is everyone talking about it? (em inglês)][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
