---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o Playwright MCP para observar a experiência de filtragem no navegador. Agora, você combinará esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as descobertas em relação à issue de filtragem.

## Cenário

A Tailspin Toys quer uma revisão consistente dos requisitos, da qualidade do código, das verificações automatizadas, da cobertura de testes e do comportamento no navegador antes de abrir um pull request (PR). Um agente personalizado pode coordenar o processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até aqui têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O Playwright MCP fornece ferramentas de navegador. O perfil de QA informa ao Copilot como usar esses recursos para avaliar os requisitos e relatar as descobertas. Ele não substitui esses recursos nem exige uma conversa separada.

## Criar o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot que crie um perfil de QA reutilizável. O perfil definirá as verificações de QA e os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecionar o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot registrou o fluxo de QA e os limites desejados. Assim, você evita usar um agente incompleto ou amplo demais quando a intenção é apenas verificar o recurso.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas ajuda a dar um nome de exibição claro ao agente.
3. Leia as instruções do perfil e confirme que o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme que o QA relata evidências de apoio, pede autorização antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão que o revise antes de continuar.

## Executar o QA com base na issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Retome a mesma conversa sobre filtragem e selecione o QA para que ele possa usar a issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com o seguinte prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use o prompt a seguir para pedir que o agente de QA revise o recurso:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. O agente de QA começará o trabalho.
4. Quando terminar, leia o relatório.

## Resumo e próximas etapas

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou o resultado. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as descobertas com base na issue de filtragem.

Agora a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, você [reunirá esses itens em um PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
