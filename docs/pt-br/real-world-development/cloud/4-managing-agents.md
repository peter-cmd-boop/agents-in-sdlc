---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] do GitHub.com. Nesse painel de controle, você pode ver todas as tarefas de agente com pull requests (PRs) abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como já havia trabalho para atualizar o estilo do site e adicionar controles de alternância, pareceu natural incluir essa funcionalidade. Você vai orientar o agente para que ele implemente também o modo claro.

## Revisar as tarefas do agente de nuvem do Copilot

Vamos verificar o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar o modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações já executadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar diferentes etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam conforme o andamento do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel do pull request à direita. Nele, você pode acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à criação do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do andamento da tarefa.

    ![Sessão de acessibilidade no painel de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes às mostradas no workshop.
3. Na caixa de diálogo **Steer active session while Copilot is working**, insira este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo de orientação destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer o prompt e incluí-lo no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa que estava executando quando você enviou a mensagem.

Isso levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre tudo o que aprendeu até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para acompanhar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nela, é possível acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou fornecer outras orientações.

Você:

- explorou o painel de controle e a página de agentes para monitorar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar uma funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
