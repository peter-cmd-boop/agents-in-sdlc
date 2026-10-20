---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma pequena atualização em um aplicativo pode exigir alterações em vários arquivos e a execução de tarefas, como testes. O processo costuma envolver localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo ficou de fora e repetir as etapas. O modo de agente do Copilot pode ajudar nesse fluxo.

O modo de agente foi criado para trabalhar de forma mais autônoma no IDE. Ele explora a estrutura existente do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos usá-lo para adicionar uma funcionalidade ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no seu IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo de agente implementa funcionalidades na interface e na camada de dados.
- revisar as alterações e os testes gerados antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem jogos por categoria e editora. No exercício anterior, você criou um helper de editoras. Agora, concluirá com o modo de agente o trabalho restante na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site para entender os recursos atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir os detalhes de cada um. Trata-se de um único aplicativo Astro, que renderiza as páginas como HTML estático durante o build. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o ORM Drizzle; não há API de back-end separada nem framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`; a interatividade é implementada com um pequeno elemento `<script>` Astro com escopo próprio e APIs DOM padrão.

### Iniciar o site

O comando `npm run dev` inicia o único servidor de desenvolvimento Astro. Execute-o no GitHub Codespaces:

1. Volte ao codespace. Depois do exercício anterior, você deverá estar na `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um banner com a URL, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo solicitando que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor Web executado no codespace.

### Explorar o site

Os principais recursos do site são:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página com mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de continuar, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indicará as opções necessárias.

A implementação inicial do site funciona, mas queremos adicionar recursos. Comece revisando o backlog de issues criado automaticamente quando você configurou o projeto a partir do modelo.

1. Na lista de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e usa ferramentas.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criar o workshop para tentar manter as sugestões relativamente consistentes. Como os modelos de linguagem são probabilísticos, as sugestões podem ser diferentes das descritas aqui. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Envie este prompt ao Copilot para consultar o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o Copilot liste todas as issues.
5. Revise a lista gerada.

Observe como o Copilot também prioriza os itens com base no que considera mais útil para quem usa o produto.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. O arquivo [de instruções da interface][ui-instructions] explica como abordar alterações na funcionalidade do site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções de tipos específicos, como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar orientações, dividir instruções complexas em arquivos menores e fornecer exemplos e modelos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## Implementar a filtragem

A filtragem exige pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento da filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam passar antes do merge. O modo de agente do Copilot pode executar essas tarefas.

1. Continue a conversa atual com o Copilot ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como o arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Na lista de agentes da exibição Chat, confirme que **Agent** está selecionado.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que o modelo **Claude Sonnet 4.5** continua selecionado.
5. Use este prompt para pedir a implementação com base na issue do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo helpers da camada de dados, interface e testes. Em seguida, ele modificará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas: explorará o projeto, modificará arquivos e executará testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os controles **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá um controle **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas em alguns testes durante o processo. O Copilot alterna entre gerar código e executar testes até concluir a tarefa e não detectar erros.

   ![Sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador onde o site está aberto e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- implementar funcionalidades na interface e na camada de dados.
- explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados antes de integrar o código.

Agora, vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício adicional – Implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu para pedir ao Copilot que atualize o site. Considere:

- seguir as práticas existentes, inclusive usar os arquivos de instruções.
- decidir como a paginação funcionará: por exemplo, se a pessoa poderá escolher o tamanho da página ou se ele será fixo.
- fornecer no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
