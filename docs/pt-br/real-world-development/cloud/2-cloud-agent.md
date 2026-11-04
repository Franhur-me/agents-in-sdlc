---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se houver alguma — não enfrentem dificuldades com dívida técnica. Pode ser a resolução de problemas de segurança, a atualização de código legado ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR de rascunho para que um desenvolvedor possa revisá-lo. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais abrangentes.

Neste exercício, você vai explorar como:

- personalizar o ambiente para a geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo bem definido.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo exiba jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar explorando. Ainda não é necessário ter uma implementação refinada; a equipe quer apenas algo que possa usar para testes de aceitação da experiência do usuário. Essa tarefa está bloqueada, mas há issues de prioridade mais alta no momento.

Esses são dois exemplos de tarefas que podem ser adiadas com facilidade e que são ótimas candidatas para atribuir ao agente de nuvem do Copilot. O agente pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras tarefas e, depois, pode voltar para revisar o trabalho e confirmar que está tudo como esperado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a um desenvolvedor humano. Assim como no trabalho com outro desenvolvedor, há várias maneiras de usá-lo, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode acompanhar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para sua revisão, ele marcará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica — você ainda precisa pensar. Até operações aparentemente simples ficam complexas quando analisadas a fundo. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos neste repositório para que você possa identificar seus efeitos mais tarde.

Abra os arquivos a seguir na interface web do GitHub do seu repositório ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que define o escopo dessas instruções como `**/*.test.ts`.

Quando você atribuir ao agente de nuvem a issue `Code lacks documentation` na próxima seção, observe se o pull request resultante inclui comentários TSDoc, segue as convenções de TypeScript e adiciona cabeçalhos de comentário. Essas alterações vêm dos arquivos de instruções que você acabou de analisar. Vamos destacá-las novamente ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja em boas condições. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar a trabalhar. Isso permite disponibilizar as ferramentas e dependências necessárias. O workflow já está configurado no laboratório para simplificar o exercício e oferecer essa experiência de aprendizado. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no menor nível necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer workflow do GitHub, mas contém alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o acionador `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona sem esperar o Copilot executá-lo.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores. Felizmente, o Copilot é muito útil para criar documentação, e esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, que terá um título parecido com `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Isso inclui explorar o código e entender o estado do projeto, pausar para pensar e decidir sobre o plano adequado e criar código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é permitir que ele execute tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar justamente esse recurso: enquanto atribuímos outra tarefa ao agente de nuvem, você começará a trabalhar em um recurso para o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Como vimos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho, permitindo que você se concentre em um conjunto de tarefas enquanto ele cuida de outro. Talvez a inclusão de uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas esse tempo ainda pode ser usado para outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver uma sequência de 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando diligentemente na segunda solicitação. O agente de nuvem do Copilot funciona de forma semelhante a um engenheiro de software, então você não precisa acompanhá-lo ativamente: basta revisar o trabalho quando ele for concluído. Vamos voltar nossa atenção para a criação e o uso de agentes personalizados.

## Resumo e próximas etapas

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para que ele as resolva de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura para outra.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam executadas com segurança.
- compreender a importância de issues com escopo bem definido.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos nos concentrar em criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e pode receber instruções personalizadas, que exploramos em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
