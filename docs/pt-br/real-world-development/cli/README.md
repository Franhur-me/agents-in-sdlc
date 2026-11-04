---
slug: pt-br/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Crie, verifique e entregue duas alterações na Tailspin Toys enquanto aprende sobre modos, personalizações, ferramentas MCP e automação de pull requests no Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI][about-copilot-cli]** coloca o GitHub Copilot no terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você mantenha o foco sem alternar para um editor gráfico.

O workshop acompanha um fluxo de trabalho contínuo na Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o ambiente.
2. Faça uma alteração focada nas avaliações por estrelas, revise-a no navegador e integre manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo Plan, implemente no modo Autopilot e revise no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill `quality-checks` existente e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem no navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar os requisitos, a cobertura e as evidências de verificação.
8. Revise toda a alteração de filtragem e use o Agent Merge no PR do recurso.
9. Explore comandos slash úteis para gerenciar o contexto, os modelos e o compartilhamento e, opcionalmente, delegar tarefas à nuvem.

Para manter o foco do workshop, você criará dois PRs: um para as avaliações por estrelas e outro para a filtragem, junto com as atualizações de instruções, a atualização da skill, o perfil de QA e os testes. O fluxo de filtragem e qualidade usa a mesma conversa e branch para que você possa aproveitar o trabalho anterior enquanto explora cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
| ------ | ----- | ----------- |
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu Codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI e conheça o ambiente |
| [2. Adicionar avaliações por estrelas: um ganho rápido][ex2] | Primeira alteração | Exiba as avaliações existentes e o estado sem avaliação; depois, integre seu primeiro PR |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje o recurso com base na issue, implemente com Autopilot e revise no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalizar e usar uma skill de quality-checks][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute a skill |
| [6. Validar a funcionalidade com o Playwright MCP][ex6] | Observação no navegador | Configure o MCP no CLI e inspecione o comportamento dos filtros |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna as evidências finais de verificação |
| [8. Criar e integrar o PR do recurso][ex8] | Revisão e merge | Revise toda a alteração, crie o PR e use o Agent Merge |
| [9. Comandos slash no GitHub Copilot CLI][ex9] | Recursos do CLI | Explore o contexto, os modelos, o compartilhamento e a delegação opcional ao agente de nuvem |
| [10. Recapitulação e próximas etapas][ex10] | Resumo | Revise o fluxo de trabalho, as personalizações reutilizáveis e outros recursos |
| [Opcional: Incorporar o Foundry][foundry] | Agentes hospedados | Prepare um modelo, implante um agente com contexto do catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano **Copilot Student, Pro, Pro+, Business ou Enterprise** ativo
- [ ] Permissão para criar um repositório e um Codespace
- [ ] Familiaridade básica com operações de terminal ou linha de comando

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][student-plan]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e os recursos do Copilot CLI usados neste workshop.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, confirme se o administrador habilitou o Copilot CLI.

## Comece

**[Comece pelos pré-requisitos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
