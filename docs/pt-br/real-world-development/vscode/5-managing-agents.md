---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho é feito em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que ocorrem. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivo propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz uma solicitação adicional: quer também uma opção de modo claro para usuários que consideram o tema padrão escuro demais. Em vez de começar uma nova conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente — os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição de arquivo aparece em linha no chat, com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição de arquivo, o diff aparecerá em linha:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente pausará e exibirá uma solicitação **Allow**. Aprove apenas os comandos que você considera seguros.

5. Aguarde até que o agente termine de implementar o modo de alto contraste. Você verá uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar um controle de modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior — incluindo os componentes alterados, a abordagem de estilo e o padrão usado no armazenamento local.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição quando aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, se tentar refatorar a interface dos controles de uma maneira de que você não goste —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer o que pretende.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois controles").

## Revisar os arquivos alterados

Antes de fazer commit do trabalho, revise rapidamente todos os arquivos alterados pelo agente.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá encontrar atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns dos arquivos alterados e percorra os diffs. Confirme se eles seguem os padrões de acessibilidade do agente personalizado — atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver tudo pronto, prepare e faça commit das alterações na visualização Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo agente do Copilot Chat para acompanhar e orientar uma sessão em andamento — revisando cada edição, aceitando ou rejeitando alterações e enviando prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou como:

- acompanhar o trabalho do agente em tempo real.
- aceitar e rejeitar edições de arquivo individuais.
- orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
