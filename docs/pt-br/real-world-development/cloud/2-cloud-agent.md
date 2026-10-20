---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se houver alguma, não enfrentam problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um PR de rascunho para que uma pessoa desenvolvedora revise o trabalho. Isso permite delegar tarefas demoradas, acelerar o desenvolvimento e reservar tempo para questões mais amplas.

Neste exercício, você explorará:

- a personalização do ambiente para gerar código.
- a execução segura de operações.
- a importância de issues com escopo claro.
- a atribuição de issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas tarefas de dívida técnica. Os prestadores de serviços que criaram a primeira versão do site não documentaram o código. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. A página de detalhes de cada jogo deve exibir jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. A equipe quer uma implementação inicial para testar a experiência, mas essa tarefa está bloqueada e tem prioridade menor que outras issues.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e que são adequadas para o agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, enquanto você se dedica a outras atividades. Depois, você revisará o trabalho e confirmará que tudo está correto.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] pode executar tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Há várias formas de iniciar o trabalho, inclusive [atribuindo uma issue do GitHub ao Copilot][assign-issue]. Depois que recebe a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura o ambiente e começa a trabalhar. Você pode acompanhar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não faz mágica: não é possível pular a etapa de pensar. Mesmo operações aparentemente simples podem se tornar complexas quando analisadas em detalhes. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático, viu `.github/copilot-instructions.md` em ação. Caso contrário, aproveite para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções do repositório sem alterá-los para perceber o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções, que você revisará novamente ao examinar PRs em um exercício futuro.

## Configurar o ambiente de desenvolvimento do agente de nuvem

A criação de código normalmente exige um ambiente específico e scripts de configuração para instalar ferramentas e dependências. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de forma semelhante a uma pessoa engenheira de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de execução. Você pode personalizar esse ambiente com um [workflow especial de configuração][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Esse workflow disponibiliza ferramentas e dependências necessárias. O projeto já está configurado para este laboratório: o Copilot terá acesso ao Node.js, às dependências, aos navegadores do Playwright para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Use as permissões mínimas necessárias para as etapas. O Copilot recebe um token próprio para as operações dele.
    permissions:
      # Se as etapas precisarem clonar o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Caso contrário, o Copilot clonará o repositório automaticamente quando as etapas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL).
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

      # Migra o banco de dados SQLite local e insere dados para builds e testes.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns detalhes importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executá-lo manualmente na guia **Actions** do repositório. Isso é útil para confirmar que o workflow funciona sem esperar que o Copilot o execute.

## Adicionar documentação

Embora a documentação seja importante, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica reduz a produtividade e dificulta a manutenção do código ou a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação, por isso essa é uma boa issue para atribuir ao agente de nuvem. Ele trabalhará em segundo plano; em um exercício futuro, você revisará o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e determinando a melhor abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role pela linha do tempo do pull request; você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

    ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em andamento para acompanhar como o Copilot resolve o problema: ele explora o código, entende o contexto, pausa para decidir a abordagem e cria código.

Essa tarefa pode levar vários minutos. Um dos principais benefícios do agente de nuvem é trabalhar de forma assíncrona, liberando você para outras tarefas. Vamos aproveitar esse recurso: enquanto o agente trabalha, atribua outra tarefa a ele e volte a desenvolver funcionalidades do aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Como destacado, uma das principais vantagens do agente de nuvem é permitir que as pessoas dividam o trabalho. A seção de jogos relacionados talvez não demore muito, mas delegá-la libera tempo para outras tarefas. Vamos atribuí-la ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

    - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
    - Show a "Related games" section on the game details page that uses the helper
    - Handle the case where a game has no related games
    - There should be unit tests created for the new helper
    - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em breve, você verá um emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O emoji de olhos indica que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente: basta revisar o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para que ele as resolva de forma assíncrona. Você pode delegar tarefas de dívida técnica, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou:

- a personalização do ambiente para gerar código.
- a execução segura de operações.
- a importância de issues com escopo claro.
- a atribuição de issues ao Copilot.

Enquanto o agente trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas exploradas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

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
