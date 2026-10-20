---
title: "Lição 10 - Revisão e próximos passos"
description: "Recapitule o fluxo de trabalho com o Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo de trabalho contínuo da Tailspin Toys. Nesta trilha, você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem do backlog.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e integrou manualmente seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, desenvolveu-a no modo Autopilot e revisou-a no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e a usou para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou a alteração completa da filtragem e autorizou o Agent Merge no PR correspondente.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizada:

1. **Avaliações por estrelas:** exibir o valor existente de `starRating` e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las à funcionalidade, personalizar o relatório de `quality-checks`, criar um perfil de QA e incluir os testes relacionados.

Desde o planejamento da filtragem até a abertura do PR, você usou a mesma conversa e a mesma branch. Reunimos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias formas: executou testes automatizados, conferiu o site no navegador e pediu ao Copilot que o explorasse com o MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. O QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes antes da abertura do PR.

Os testes adicionados devem preencher lacunas reais; se uma execução de QA concluir que não são necessários novos testes, isso também pode estar correto. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas após as alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho que ele realiza. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e adapte-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados estabelecem funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados reais das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para definir uma abordagem antes de desenvolver, **Interactive** para acompanhar mudanças pontuais e **Autopilot** para tarefas com escopo claro. Escolha um modelo mais rápido para alterações de rotina e um modelo mais capaz para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever com clareza *o que* precisa ser feito, *por que* e *como* altera significativamente o resultado.

## Outros recursos para explorar

Você concluiu o fluxo principal. Há alguns outros recursos da CLI que vale a pena conhecer:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para discutir um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar outra tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de se familiarizar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você queria criar há anos. Compartilhe o que aprendeu com a equipe e aprenda com ela. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
