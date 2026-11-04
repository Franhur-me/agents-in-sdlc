---
title: "Lição 9 - Comandos slash no GitHub Copilot CLI"
description: "Explore comandos slash para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas à nuvem opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos slash para interagir com ele. Esses comandos expõem funcionalidades avançadas, informações internas e opções de configuração adicionais. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para explorar os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho em paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal do CLI. Agora, vamos explorar alguns recursos adicionais — gerenciar o contexto, alternar entre modelos, compartilhar sessões e, opcionalmente, delegar trabalho ao [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, você pode atingir o limite da janela de contexto do modelo. Quando necessário, o Copilot CLI compacta a conversa automaticamente. Você também pode inspecionar e compactar o contexto com comandos slash.

1. Volte ao Codespace e, se ainda não estiver aberto, inicie o Copilot CLI na raiz do repositório.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez não haja uma grande diferença se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela está cheia. Use `/compact` para escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação com a conversa atual e iniciar uma conversa nova.

## Escolher um modelo

Os modelos têm pontos fortes diferentes, assim como os desenvolvedores têm preferências diferentes. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro modelo ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

O trabalho em equipe e o compartilhamento de aprendizados ajudam todos a aprimorar o uso de ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e revise as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretenda compartilhar.

## Opcional: ampliar o trabalho ou delegar

O workshop principal está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir tarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma worktree isolada para uma tarefa separada.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Comece uma tarefa nova e bem definida antes de experimentá-los e revise o resultado pelo fluxo de trabalho habitual. Se quiser se aprofundar no trabalho assíncrono com agentes, continue com o [workshop de agentes de nuvem][cloud-workshop].

## Resumo e próximas etapas

Os comandos slash do Copilot CLI permitem configurar a ferramenta, compartilhar sessões e ver o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho em paralelo, worktrees e delegação ao agente de nuvem.

Há muitos outros comandos slash disponíveis para explorar. Vamos encerrar esta jornada [revisando o que aprendemos][next-lesson] e vendo as próximas etapas.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
