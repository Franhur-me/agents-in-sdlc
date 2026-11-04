---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo agente. Antes de abrir um pull request, confirme se ele realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, você conectará o **servidor MCP do Playwright** e permitirá que o Copilot controle um navegador real para testar o recurso — depois, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot,
- adicionar o servidor MCP do Playwright ao workspace,
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem no navegador,
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) é como o Copilot faz isso: um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo agente e como ele interage com contexto, LLM e ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) oferece aos agentes de IA uma maneira de se comunicar com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Isso permite acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como APIs existentes ou ferramentas locais, por exemplo, pacotes NPM. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. O agente de IA pode, por exemplo, criar novos repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright. O agente de IA pode, por exemplo, navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que oferecem acesso a ferramentas e recursos diferentes. O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e a contribuição para o ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e avalie as implicações de segurança. Use apenas servidores MCP em que você confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

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

Essa entrada `github` vem do template do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é preciso instalar nada localmente — o VS Code se conecta por HTTP e você se autentica com o GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicionar o servidor MCP do Playwright

Agora você adicionará outro servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, ideal para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, deixando o arquivo assim:

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

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` faz o Playwright executar o navegador sem uma janela visível, o que é necessário em um codespace, onde não há uma área de trabalho para exibi-lo.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para os testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme que confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme que confia no servidor, revise a configuração e escolha a opção para confiar nele.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor — e pedirá que você confirme que confia nele — na primeira vez que o Copilot precisar das ferramentas no exercício seguinte.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] dá ao Copilot acesso a um navegador real. Em vez de navegar pelo aplicativo para testar seu trabalho, o agente pode abrir uma página, navegar pelo site, aplicar filtros, ler os resultados e resumir o que observou. É a maneira mais rápida de confirmar se um recurso funciona como esperado sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, em vez de capturas de tela. Isso significa que o agente processa elementos estruturados e rotulados (botões, links, itens de lista) como a tecnologia assistiva faria — portanto, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem criado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo não parecer certo, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que confirmou que o recurso funciona, você pode abrir um pull request (PR) para que a equipe o revise. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, incluindo o github.com e a interface de linha de comando do GitHub (CLI). Como você já está trabalhando com o GitHub Copilot, vamos deixar que ele crie o PR. Ele pode localizar a issue correspondente e associá-la ao PR.

1. Acesse o painel do Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Confirme que **Agent** está selecionado no menu suspenso de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot colete informações e execute as operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar com o GitHub — siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link fornecido pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot,
- adicionou o servidor MCP do Playwright ao workspace,
- usou o servidor para testar manualmente o recurso de filtragem no navegador antes de publicá-lo,
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para simplificar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Playwright da Microsoft][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de servidores MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
