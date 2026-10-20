---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser realizada de uma forma específica — ou há informações de contexto que o Copilot deve conhecer — esse contexto precisa estar acessível. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instrução com escopo por caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos percursos do VS Code e da CLI, aqui você não executará um prompt *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, por isso é mais difícil demonstrar o efeito das instruções lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve ser documentado com comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo de código e os requisitos do projeto. Você pode orientar as sugestões especificando convenções, bibliotecas e os comentários esperados. É possível criar instruções para todo o repositório ou para tipos de arquivo e tarefas específicos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em todas as solicitações do repositório. Ele deve conter informações relevantes para a maioria das interações, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Eles podem orientar linguagens como TypeScript ou Astro ou tarefas como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também aceita instruções em outros padrões, como arquivos AGENTS.md e CLAUDE.md, conforme descrito em [formatos de instrução compatíveis][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está além do escopo deste workshop. Os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` concentrado nas orientações gerais do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões de código globais.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou de tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos do que fazer e do que evitar.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única forma correta de usar IA. Com a experimentação, você descobrirá o que funciona no projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para diversos tipos de tarefa, incluindo [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição correspondente explicará o recurso quando ele for relevante.
>
> Para encontrar modelos ou exemplos iniciais, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Reserve um momento para ler os arquivos de instruções incluídos no repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para workflows do GitHub Actions. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob, relativo à raiz do repositório, que determina os arquivos aos quais as instruções se aplicam. Neste caso, correspondem arquivos de teste TypeScript, por exemplo, aqueles que correspondem a `**/*.test.ts`.
5. Observe as instruções para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o fim. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores em arquivos menores e reutilizáveis e indicar exemplos para o Copilot. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário por arquivo.

## Adicionar um novo padrão para o repositório

Esta é a única edição que você fará neste exercício: adicionar ao projeto uma regra que exija documentação em comentários TSDoc e um cabeçalho de comentário por arquivo. O agente de nuvem usará essa orientação nas issues que você atribuir a ele mais adiante.

Antes de editar, prepare uma branch (o agente de nuvem lerá as instruções da branch de destino da issue, mas você também fará commit das alterações):

1. No terminal do codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como destacado anteriormente, `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela descreve os padrões de código do projeto, mas ainda não exige documentação no código; por isso, o helper gerado não tinha comentários de documentação.
3. Adicione estas linhas Markdown abaixo dos padrões existentes para instruir o Copilot a criar cabeçalhos de comentário e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como visto na lição anterior, os arquivos de instruções podem ficar no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientação global ou em arquivos `*.instructions.md` específicos para linguagens, tipos de arquivo ou tarefas. O arquivo de nível do repositório é o lugar adequado para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Fazer commit, enviar e integrar a atualização

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Quando você atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`; portanto, suas alterações nas instruções precisam estar em `main` para que o agente as use.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem receberá as instruções quando trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente na `main` durante este workshop, pule a etapa de criar uma branch e faça o commit direto nela. A etapa da branch demonstra um fluxo de trabalho mais próximo do que você usaria em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests gerados pelo agente de nuvem nas próximas lições.

Em seguida, você vai [atribuir sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
