---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] fornecem contexto sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de instruções com escopo por caminho.
- gerar o primeiro helper de dados para a filtragem (um helper de editoras) com as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código seguir o novo padrão.
- integrar as instruções e o helper de filtragem à `main` para que o Copilot possa usar a orientação atualizada no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar uma tendência de mudança depois de atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve ser documentado com comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Os arquivos de instruções fornecem ao Copilot o contexto necessário para alinhar o trabalho a essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo de código e os requisitos do projeto. Você pode especificar convenções, bibliotecas e comentários esperados. É possível criar instruções para todo o repositório ou para tipos de arquivo e tarefas específicos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, enviado ao Copilot em todas as solicitações do repositório. Ele contém informações aplicáveis à maioria das solicitações, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`, com orientações para linguagens como TypeScript ou Astro, ou para tarefas como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a outros padrões de instruções, como AGENTS.md e CLAUDE.md, conforme descrito em [formatos de instrução compatíveis][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Os exemplos do projeto demonstram uma abordagem representativa:

- Mantenha `copilot-instructions.md` concentrado nas orientações gerais do projeto, como a descrição, a estrutura e os padrões globais de código.
- Use arquivos `*.instructions.md` para tipos específicos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras, com exemplos do que fazer e do que evitar.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única forma correta de usar IA. Experimente para descobrir o que funciona no projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Neste projeto, há orientações para tarefas como [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar instruções. Cada interface oferece essa opção de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções do projeto

O projeto já inclui vários arquivos de instruções. Vamos examiná-los antes de fazer alterações para observar o efeito.

1. Volte ao Codespace.
2. No editor do Codespaces, e não no terminal, abra `.github/copilot-instructions.md`.
3. Observe a descrição do projeto e as orientações de código. Elas se aplicam a todas as interações com o Copilot no repositório.
4. Abra a pasta `.github/instructions` e examine as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no frontmatter define um padrão glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e arquivos do projeto. Esses links ajudam a dividir conjuntos maiores em arquivos menores e reutilizáveis e a apresentar exemplos ao Copilot.

## Criar uma branch

Vamos seguir as práticas recomendadas e criar uma branch para as alterações.

1. Volte ao Codespace.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de continuar, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa indicará as opções necessárias.

Para observar o efeito das instruções, envie um prompt antes de alterá-las. Depois, atualize as instruções e envie o mesmo prompt novamente.

1. Feche as guias abertas no editor para que o Copilot use apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na lista de agentes da exibição Chat, selecione **Agent** para que o Copilot possa aplicar alterações nos arquivos.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará as alterações, possivelmente no helper e nos testes.
6. Observe que o helper proposto é tipado, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicada a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário para o arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode adicionar comentários TSDoc mesmo sem uma instrução explícita. Tudo bem: o objetivo é observar a consistência depois da atualização.

## Adicionar um padrão para o repositório

Vamos registrar os padrões de código em `.github/copilot-instructions.md` para orientar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela descreve os padrões de código, mas ainda não exige documentação no código.
3. Adicione estas linhas Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve o arquivo.

> [!TIP]
> As instruções globais devem ficar em `.github/copilot-instructions.md`; instruções específicas de linguagens, tipos de arquivo ou tarefas podem ficar em arquivos `*.instructions.md`.

## Enviar o prompt novamente e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o mesmo prompt usado anteriormente:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentário semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo a código que servirá de base para o próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são gerenciados pelo controle do código-fonte como qualquer outro arquivo do repositório.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme que as alterações das instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações de suporte (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e integrar um pull request

Com a branch enviada, crie um pull request e associe-o à issue de documentação do backlog. O Copilot pode encontrar a issue, criar um PR para fechá-la e integrá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O modelo do projeto já configura as ferramentas do GitHub no espaço de trabalho. Na primeira utilização, o VS Code pode solicitar que você entre na conta do GitHub e autorize a conexão. Você aprenderá como ela funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição do Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que encontre a issue das instruções e crie um PR a partir da branch atual, descrevendo a atualização das instruções e a base do helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot localizará a issue e criará o PR.
5. Quando solicitado a permitir ações no GitHub, revise a ação e selecione **Allow** se concordar.
6. Depois que o PR for criado, peça ao Copilot que faça o merge e volte para a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir ações no GitHub e comandos de shell, revise cada solicitação antes de selecionar **Allow**.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código seguir a orientação atualizada.
- integrar as instruções e a base do helper à `main` para que o próximo exercício possa usá-las.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson], enquanto o Copilot adiciona uma funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas do GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
