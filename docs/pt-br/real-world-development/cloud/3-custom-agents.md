---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou áreas específicas do fluxo de trabalho de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer instruções direcionadas, práticas recomendadas, padrões de código e conhecimento específico do domínio para orientar o Copilot. As equipes podem registrar sua experiência em agentes reutilizáveis: por exemplo, um agente de acessibilidade que verifica a conformidade com as [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo contém frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Diferenças entre agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm alguns pontos em comum: ambos são definidos principalmente em arquivos Markdown e orientam a IA na execução de tarefas. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills — e até outros agentes — durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site conforme as diretrizes de acessibilidade. Ele pode, por exemplo, chamar skills para executar um fluxo de pull request ou executar e gerenciar testes.

> [!NOTE]
> Não existe uma única maneira correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona no ambiente e no cenário do projeto.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- entender como agentes personalizados são definidos.
- atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Comentários recentes de usuários indicam que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre o texto e o plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado ou desativado.

Como a acessibilidade é essencial, queremos implementar essa funcionalidade rapidamente. Para isso, usaremos um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Vamos revisar seu conteúdo para entender como ele orienta o Copilot.

Volte ao codespace e abra o arquivo do agente personalizado:

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` do frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - as responsabilidades principais ao gerar código para um site acessível.
   - as práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - erros e problemas comuns.

## Criar e atribuir uma issue

O painel de agentes é o centro de controle do ambiente. Nele, você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar as tarefas e redirecionar sessões em andamento ou fornecer orientações adicionais. Vamos pedir ao Copilot que adicione o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Na lista, selecione **Accessibility agent**.

    ![Atribuição ao agente de nuvem com o agente personalizado e Accessibility destacados](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados para tarefas e domínios específicos. Com eles, é possível registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o trabalho do Copilot.

Você aprendeu:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, vamos [monitorar e orientar a sessão do agente][next-lesson] no painel de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
