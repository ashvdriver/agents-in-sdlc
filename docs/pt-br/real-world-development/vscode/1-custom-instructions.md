---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de determinada maneira, ou se há informações de contexto que o Copilot deveria conhecer, você quer garantir que esse contexto esteja ao alcance dele. Há várias formas de compartilhar contexto específico com o Copilot. Entre elas, as principais são os [arquivos de instruções][instruction-files], que fornecem essa orientação sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo de caminho,
- gerar a primeira parte dos dados para a filtragem (um auxiliar de distribuidoras) com as instruções *atuais* em vigor,
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`,
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão,
- fazer commit da atualização das instruções e da parte de filtragem na `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode divergir de alguns dos padrões que você definiu. O Copilot não é determinístico. O objetivo deste exercício é ver a *tendência* de mudança de comportamento depois de atualizar as instruções, e não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código na forma de comentários de documentação TSDoc.
- Um bloco de comentários deve ser adicionado ao início de cada arquivo, descrevendo o que o arquivo faz.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações certas para executar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot, para que ele entenda melhor seu estilo de código e seus requisitos. É um recurso poderoso que ajuda a direcionar o Copilot para obter sugestões e trechos de código mais relevantes. Você pode especificar suas convenções de código preferidas, as bibliotecas e até os tipos de comentário que gosta de incluir. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, oferecendo contexto no nível da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações do repositório. Esse arquivo deve conter informações no nível do projeto, ou seja, contexto relevante para a maioria das solicitações de chat ou da CLI enviadas ao Copilot. Isso pode incluir a pilha de tecnologia usada, uma visão geral do que está sendo criado, práticas recomendadas e outras orientações globais.
- Arquivos `.github/instructions/*.instructions.md` podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens específicas (como TypeScript ou Astro) ou para tarefas como criar um componente de interface ou um novo conjunto de testes unitários.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados apenas para a geração de código no Copilot Chat, e não para conclusões de código nem para sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto os arquivos no nível do repositório quanto os arquivos `*.instructions.md` (com o front matter `applyTo`) ao gerar código.
>
> Por fim, o Copilot [oferece suporte a arquivos de instruções que usam outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre como criar arquivos de instruções está além do escopo do workshop. No entanto, os exemplos fornecidos no projeto de exemplo mostram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações no nível do projeto, como a descrição do que está sendo criado, a estrutura do projeto e os padrões de código globais.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou para tarefas específicas.
- Use linguagem natural. Mantenha as orientações claras. Dê exemplos de como o código deve (e não deve) ser.

Não existe uma única maneira específica de criar arquivos de instruções, assim como não existe uma única maneira específica de usar IA. Você descobrirá, por meio de experimentação, o que funciona melhor para o seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Ao explorar os deste projeto, você pode notar que há arquivos para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções para você. Cada superfície expõe isso de uma forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI); a lição da superfície em que você está indicará isso quando for relevante.
>
> Procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório cheio de arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções que acompanham este repositório: há um arquivo principal `copilot-instructions.md` e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo, observando a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe a orientação aninhada **GitHub Actions Workflows**. Elas se aplicam a todas as interações que você tiver com o Copilot.
3. Abra a pasta `.github/instructions` e dê uma olhada. Observe que há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início, que define um glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Aqui, qualquer arquivo de teste TypeScript (por exemplo, um que corresponda a `**/*.test.ts`) será correspondido.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar para o Copilot exemplos a seguir ao gerar código. (Os caminhos ali são relativos ao arquivo de instruções, e não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários de documentação TSDoc e cabeçalhos de comentário nos arquivos.

## Criar uma branch para as alterações

Vamos seguir algumas práticas recomendadas e criar uma branch para as nossas alterações.

1. Volte ao seu codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma nova branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção de modo e de modelo varia por exercício; cada etapa indica isso quando for importante.

Para ver o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual em vigor. Mais tarde, você o atualizará e enviará o mesmo prompt novamente para ver a diferença.

1. Feche todas as guias do editor abertas em exercícios anteriores para que o Copilot capte apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para que o Copilot saiba onde o auxiliar deve ficar.
3. Selecione **Agent** na lista suspensa de agentes na visualização do Chat para que o Copilot possa aplicar alterações nos arquivos.

   ![Captura de tela do seletor de agentes na visualização do Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e aplica atualizações de código, que muitas vezes abrangem o arquivo do auxiliar e os testes dele.
6. Observe que o auxiliar proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de distribuidoras. Isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários de documentação TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, é possível que ele adicione comentários de documentação mesmo sem ser instruído a fazê-lo. Se isso acontecer, tudo bem: a melhoria na *consistência* depois da atualização das instruções continua sendo o objetivo.

## Adicionar um novo padrão ao repositório

Como destacado antes, `.github/copilot-instructions.md` foi criado para fornecer informações no nível do projeto ao Copilot. Vamos garantir que os padrões de código do repositório estejam documentados para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de código do projeto, mas ainda não tem uma regra para documentação no código, e é por isso que o auxiliar gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo abaixo dos padrões existentes para instruir o Copilot a adicionar cabeçalhos de comentário nos arquivos e comentários de documentação TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos. O arquivo no nível do repositório é o lugar certo para padrões de todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Executar o prompt novamente e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o buffer.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo certo.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real, no qual o próximo exercício se baseará.

## Fazer commit das instruções e push da branch

Os arquivos de instruções são como qualquer outro ativo do repositório, ou seja, são gerenciados com a mesma abordagem de controle de versão que você usaria para qualquer outro item. Então vamos fazer commit e push da branch para o nosso repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. No terminal, confirme que a atualização das instruções e as alterações do auxiliar estão presentes executando:

   ```bash
   git status
   ```

3. No terminal, faça o stage e o commit da atualização das instruções e de cada arquivo que o Copilot alterou para a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. No terminal, faça o push da branch para o repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e fazer o merge de um pull request

Com a branch enviada, devemos criar um pull request e associá-lo à issue do padrão de documentação no seu backlog. Poderíamos fazer isso manualmente, mas o Copilot pode fazê-lo por nós usando as ferramentas do GitHub que já estão conectadas ao seu projeto. Vamos pedir ao Copilot que encontre a issue, crie o PR para fechá-la e faça o merge dele.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot age no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao seu workspace, então não há nada para configurar: na primeira vez que o Copilot usar uma delas, o VS Code pode solicitar que você entre no GitHub. Siga as instruções para permitir. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat dentro do seu codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual que descreva tanto as atualizações das instruções quanto a nova base do auxiliar de distribuidoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a trabalhar para encontrar a issue e criar o PR.
5. Quando for solicitado que você **Allow** o Copilot a executar ações do GitHub em seu nome, revise o comando e selecione **Allow**, conforme apropriado.
6. Depois que o PR for criado, peça ao Copilot que faça o merge dele e retorne sua branch para a main usando o prompt a seguir:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando for solicitado que você **Allow** o Copilot a executar ações do GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow**, conforme apropriado.

Você acabou de criar e fazer o merge de um pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e, depois, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o que o Copilot produz com as instruções *existentes*,
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`,
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão,
- fazer commit da atualização das instruções e da base do auxiliar na `main` para que o próximo exercício possa se basear neles.

Em seguida, você colocará essas instruções em prática no [modo de agente][next-lesson], enquanto o Copilot adiciona um novo recurso em toda a base de código.

## Recursos

- [Arquivos de instruções para a personalização do GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot (em inglês)][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot: uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
