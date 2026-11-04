---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer —, é importante garantir que esse contexto esteja disponível. Há várias maneiras de fornecer contexto específico ao Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos de instruções com escopo por caminho,
- gerar a primeira parte dos dados para a filtragem (um helper de editoras) usando as instruções *atuais*,
- adicionar um novo padrão válido para todo o repositório a `.github/copilot-instructions.md`,
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão,
- fazer commit das atualizações de instruções e da parte inicial da filtragem em `main` para que o Copilot possa usar as novas orientações no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança de comportamento depois de atualizar as instruções, e não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter um visual moderno.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários deve ser adicionado ao início de cada arquivo para descrever o que ele faz.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso permite orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de programação, bibliotecas e até os tipos de comentários que prefere incluir no código. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, oferecendo contexto no nível da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações no nível do projeto — contexto relevante para a maioria das solicitações enviadas ao Copilot por chat ou pelo CLI. Pode incluir a stack usada, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer orientações para linguagens como TypeScript ou Astro, ou para tarefas como criar um componente de UI ou um novo conjunto de testes unitários.

> [!NOTE]
> Ao trabalhar no IDE, os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para autocompletar nem para sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções no nível do repositório quanto os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também oferece suporte a arquivos de instruções que seguem outros padrões, incluindo `AGENTS.md` e `CLAUDE.md`.[Saiba mais sobre o suporte a arquivos de instruções][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está além do escopo deste workshop. Ainda assim, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações no nível do projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões globais de programação.
- Use arquivos `*.instructions.md` para fornecer instruções específicas para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Dê exemplos de como o código deve — e não deve — ser escrito.

Não há uma única maneira certa de criar arquivos de instruções ou de usar IA. Com a experimentação, você descobrirá o que funciona melhor para o seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefas, como [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece esse recurso de uma maneira diferente — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição da interface que você está usando abordará isso quando for relevante.
>
> Está procurando modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um `copilot-instructions.md` principal e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe a orientação aninhada sobre **GitHub Actions Workflows**. Essas informações se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, para a camada de dados com Drizzle, para testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript, por exemplo, um que corresponda a `**/*.test.ts`, será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos a seguir na geração de código. (Os caminhos são relativos ao arquivo de instruções, e não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário no nível do arquivo.

## Criar uma branch para as alterações

Vamos seguir algumas boas práticas e criar uma branch para as alterações.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam de acordo com o exercício — cada etapa indicará o que selecionar quando necessário.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar o resultado.

1. Feche todas as guias de editor abertas nos exercícios anteriores para que o Copilot use apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deverá ficar.
3. Selecione **Agent** no menu suspenso de agentes da visualização Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, muitas vezes ao helper e aos respectivos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras — isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (que se aplica a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários de documentação mesmo sem receber essa instrução. Se isso acontecer, tudo bem — o objetivo continua sendo observar a melhoria de *consistência* depois da atualização das instruções.

## Adicionar um novo padrão ao repositório

Como destacado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de programação do projeto, mas ainda não inclui uma regra para documentação no código — por isso, o helper gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo após os padrões existentes para instruir o Copilot a incluir cabeçalhos de comentário no arquivo e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o local adequado para padrões válidos em todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a alteração

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o histórico.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com id e nome.
    *
    * @param db - O cliente de banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com um array de objetos de editora.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real, que será usado como base no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são recursos do projeto e devem ser gerenciados com o mesmo processo de controle de versão usado para qualquer outro arquivo. Vamos fazer commit das alterações e enviar a branch ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações nas instruções e no helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit das atualizações nas instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e integrar um pull request

Com a branch enviada, vamos abrir um pull request e vinculá-lo à issue sobre o padrão de documentação no backlog. Em vez de fazer isso manualmente, podemos pedir ao Copilot que encontre a issue, crie o PR para resolvê-la e, em seguida, faça o merge.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O template do projeto já conecta as ferramentas do GitHub ao workspace, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma ferramenta do GitHub, o VS Code poderá solicitar que você entre na conta. Siga as instruções para permitir o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização do Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo tanto as atualizações nas instruções quanto a nova base do helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando o Copilot solicitar autorização para executar ações do GitHub em seu nome, revise o comando e selecione **Allow** se for apropriado.
5. Depois que o PR for criado, peça ao Copilot que faça o merge do PR e volte para `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Quando o Copilot solicitar autorização para executar ações do GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow** se for apropriado.

Você acaba de criar e integrar um pull request com a ajuda do GitHub Copilot.

## Resumo e próximas etapas

Você explorou como o Copilot recebe contexto dos arquivos de instruções deste projeto e, em seguida, usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*,
- adicionar um novo padrão válido para todo o repositório a `.github/copilot-instructions.md`,
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão,
- fazer commit das atualizações nas instruções e da base do helper em `main` para que o próximo exercício possa usá-las.

Em seguida, você aplicará essas instruções ao [implementar uma funcionalidade][next-lesson] do backlog.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
