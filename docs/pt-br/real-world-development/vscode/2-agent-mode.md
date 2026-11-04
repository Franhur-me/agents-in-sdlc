---
title: "Exercício 2 - Adicionar funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e a execução de operações, como testes. O fluxo de trabalho típico de um desenvolvedor envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, identificar o arquivo que faltou atualizar, fazer outra alteração... e assim por diante.

É aí que entra o modo agente do Copilot.

O modo agente do Copilot foi criado para trabalhar de forma mais autônoma no IDE. Ele funciona de maneira semelhante a um desenvolvedor: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como você pode usar o modo agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente executa tarefas no IDE, o que permite ciclos rápidos de feedback e interação. O agente de nuvem foi criado para trabalhar de forma assíncrona como um integrante da equipe, interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo agente do Copilot antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes de cada um. O site é um aplicativo Astro único que renderiza suas páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o ORM Drizzle — não há uma API de backend separada nem uma estrutura de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno elemento `<script>` do Astro com escopo limitado e APIs padrão do DOM.

### Iniciar o site

O comando `npm run dev` inicia o único servidor de desenvolvimento do Astro e facilita a execução do site. Siga estas etapas para executá-lo no GitHub Codespace:

1. Volte ao codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela para trabalhar na filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento do Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, ao selecionar o link da URL localhost no terminal, você será redirecionado automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explorar o site

Com o site em execução, você pode explorar os recursos disponíveis. Entre eles:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam de acordo com o exercício — cada etapa indicará o que selecionar quando necessário.

A implementação inicial do site está funcionando, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do template, você recebeu automaticamente um backlog de issues. Peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** no menu suspenso de agentes da visualização Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em particular. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e, por isso, o incluímos nas instruções. Esperamos que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot que consulte o backlog de issues enviando este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens de acordo com o que considera mais útil para o usuário.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, é uma boa hora para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades. Elas incluem:
   - Uma visão geral da arquitetura.
   - Princípios de design de componentes, testabilidade e acessibilidade.
   - Links para arquivos de instruções específicos para vários tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso facilita a reutilização, a divisão de conjuntos de instruções maiores em partes menores e mais fáceis de gerenciar e o uso de exemplos e templates.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações separadas no aplicativo:

- Adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- Adicionar ou atualizar testes do comportamento dos filtros.
- Atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes do merge no código do projeto. O modo agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Você pode continuar na conversa atual com o Copilot ou selecionar **New Chat** para iniciar outra conversa.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela da seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog usando este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorando o projeto e localizando os arquivos relacionados à funcionalidade. Ele deverá encontrar a camada de dados, a interface e os testes. Em seguida, começará a alterar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Você verá o Copilot executar várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá um botão **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela da caixa de diálogo do painel Copilot Chat solicitando a confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e até falhas em alguns testes durante o processo. Não há problema: o Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar mais erros.

   ![Captura de tela de uma sessão completa do Copilot Chat no modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, você explorou como usar o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- o modo agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar alterações e testes gerados pelo modo agente do Copilot antes de integrá-los à base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra – Implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Use as habilidades aprendidas neste exercício e peça ao Copilot que atualize o site para incluir paginação. Algumas considerações:

- siga as boas práticas existentes, incluindo o uso dos arquivos de instruções disponíveis.
- decida como a paginação deverá funcionar: se o usuário poderá selecionar o tamanho da página ou se ele será definido no código.
- ao criar o prompt, forneça as orientações necessárias para que o Copilot faça a implementação desejada.
- talvez seja necessário fazer iterações com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agente do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
