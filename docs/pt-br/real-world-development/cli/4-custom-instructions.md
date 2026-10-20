---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você desenvolveu a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções com escopo por caminho chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não sejam óbvias, em vez de apenas repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar a finalidade, os parâmetros e os valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários precisam continuar corretos quando o código relacionado mudar.
- As orientações de formatação e lint existentes devem ser mantidas.

Com os arquivos de instruções, você fornecerá ao Copilot o contexto necessário para trabalhar de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo de código e os requisitos do projeto. Esse recurso permite orientar as sugestões e os trechos de código, por exemplo, especificando convenções, bibliotecas e os comentários esperados. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um arquivo enviado ao Copilot em todas as solicitações do repositório. Ele deve conter informações aplicáveis à maioria das solicitações de chat ou CLI.
- `.github/instructions/*.instructions.md`, arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui vários arquivos de instruções. Vamos examiná-los antes de fazer uma alteração para observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, e não no terminal, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e para arquivos do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot.

## Atualizar as instruções para refletir as diretrizes da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma orientação. Vamos modificar o arquivo principal `copilot-instructions.md` para que o TypeScript recém-gerado tenha comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione este item ao final da lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot carregue a orientação atualizada sem perder o contexto da funcionalidade.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para encerrá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique a orientação atualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme que eles explicam o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instruções e aplicou um novo padrão à sua funcionalidade. Nesta lição, você:

- explorou como as instruções do repositório e os arquivos com escopo por caminho chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você vai [personalizar e executar a skill reutilizável `quality-checks`][next-lesson] para garantir que o lint e os testes sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
