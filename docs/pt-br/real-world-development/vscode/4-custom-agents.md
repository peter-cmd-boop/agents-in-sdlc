---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou áreas específicas do fluxo de trabalho de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer instruções direcionadas, práticas recomendadas, padrões de código e conhecimento específico do domínio. As equipes podem registrar sua experiência em agentes reutilizáveis: um agente de acessibilidade que verifica a conformidade com as [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou, globalmente, em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm alguns pontos em comum: ambos são definidos principalmente por arquivos Markdown e orientam a IA na execução de tarefas. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e podem orquestrar skills — e até outros agentes — durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Como parte do trabalho, ele poderia chamar skills para executar um fluxo de pull request ou executar e gerenciar testes.

> [!NOTE]
> Não existe uma única maneira correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para descobrir o que funciona nos ambientes e cenários do projeto.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente das habilidades ou preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre texto e plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado ou desativado.

Como a acessibilidade é essencial, você quer implementar esse recurso rapidamente. Para isso, usará um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e, no terminal, crie uma branch a partir de `main` para o trabalho de acessibilidade. Assim, o PR da filtragem do Exercício 3 continuará separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` do frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que descrevem:
   - as principais responsabilidades ao gerar código para um site acessível.
   - as práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - erros e problemas comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior da exibição do Copilot Chat. Você pode selecionar um agente personalizado para limitar uma sessão às instruções e ferramentas daquele agente.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício — cada etapa indicará as opções relevantes.

1. Na lista de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na parte inferior da exibição Chat, selecione o seletor de agentes (por padrão, ele mostra **default**).
3. Na lista, selecione **Accessibility agent**.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. À medida que as edições forem aplicadas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot fará alterações reais no repositório: poderá editar componentes Astro, arquivos CSS e testes relacionados.

Você acompanhará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes especializados para tarefas e domínios específicos. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o trabalho do Copilot.

Você aprendeu:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você vai [acompanhar e orientar o trabalho do agente][next-lesson], revisar as alterações enquanto são feitas e adicionar um controle para o modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no seu IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar sua funcionalidade com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
