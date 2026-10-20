---
title: "Exercício 3 - Testar sua funcionalidade com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar a funcionalidade. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP adicionam ferramentas ao Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais potente quando pode acessar recursos além do editor. O Model Context Protocol (MCP) oferece uma maneira padronizada para o agente se comunicar com ferramentas e serviços externos.

![Diagrama da estrutura interna do modo de agente e sua interação com contexto, modelo de linguagem e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Com o MCP, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome usando ferramentas.

Os servidores MCP funcionam como pontes entre o agente de IA e as ferramentas e serviços externos, como APIs ou ferramentas locais instaladas por pacotes npm. Cada servidor oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp-server]**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece automação de navegador, como acessar páginas, preencher formulários e selecionar botões.

Há muitos outros servidores disponíveis. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis no espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá que o servidor `github` já está configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada vem do modelo do projeto e permitiu ao Copilot ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não é necessário instalar nada localmente: o VS Code conecta-se por HTTP e você autentica na primeira utilização de uma ferramenta.

## Adicionar o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione a entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o servidor `playwright` é **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. A opção `--headless` indica que o navegador será executado sem janela visível, o que é necessário em um codespace, sem área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que está faltando um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pergunta se você confia nele.

1. Em `.vscode/mcp.json`, selecione **Start**, exibido acima da entrada `playwright`, para iniciar o servidor e confirmar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para iniciá-lo.

Essa etapa manual é opcional. Se você não iniciar o servidor agora, o VS Code o iniciará e pedirá sua confirmação na primeira vez que o Copilot precisar das ferramentas.

## Testar a filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece um navegador real para o Copilot controlar. Em vez de navegar pelo aplicativo para testar o trabalho, o agente pode abrir uma página, aplicar filtros, ler os resultados e resumi-los.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. O agente trabalha com elementos estruturados e identificados, como botões, links e itens de lista, da mesma forma que uma tecnologia assistiva. Assim, uma verificação funcional rápida também ajuda a avaliar a acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e relatará o que encontrou. Compare o relatório com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas adicionais ou peça que o Copilot corrija o código antes de abrir um pull request.

> [!NOTE]
> Para executar o teste, o aplicativo precisa estar em `http://localhost:4321`. Se o servidor de desenvolvimento foi interrompido, inicie-o novamente antes de enviar o prompt. Na primeira utilização do MCP do Playwright, talvez seja necessário baixar um navegador. Se o Copilot informar que está faltando um navegador, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que confirmou que a funcionalidade funciona, abra um pull request (PR) para que a equipe possa revisá-la. Primeiro, publique a branch `filtering-vscode`.

1. No codespace, abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com ou pela interface de linha de comando do GitHub. Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR, encontre a issue relacionada e associe o PR a ela.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Na lista de agentes, confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot obtenha informações e execute ações. Na primeira utilização de uma ferramenta do GitHub, talvez seja solicitada autenticação. Siga as instruções.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link criado pelo Copilot para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP adicionam ferramentas ao Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente a filtragem no navegador antes de publicar.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas essa é apenas uma maneira de ampliar o Copilot. Em seguida, você vai [criar um agente personalizado][next-lesson] para agilizar tarefas direcionadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre ele?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
