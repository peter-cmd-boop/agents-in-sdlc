---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para desenvolver a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando uma pequena funcionalidade ao projeto. Mudanças maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo existente da organização e ajudar a desenvolver as coisas certas da maneira certa. Nesta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: começará com uma issue para gerar uma funcionalidade, verificará se o código está correto e se a funcionalidade se comporta como esperado e, por fim, fará o merge no projeto.

> [!NOTE]
> Você usará a mesma conversa e a mesma branch durante todo o fluxo de trabalho da funcionalidade. Em geral, cada tipo de arquivo teria uma branch ou um PR próprio, mas vamos simplificar para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar a funcionalidade usando o modo Autopilot.
- revisar o código.
- validar manualmente a funcionalidade em um navegador encaminhado.

Nas próximas etapas desse trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e as pessoas visitantes precisam filtrar jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes da implementação. Você usará o modo Plan para decidir esses pontos e, em seguida, autorizará uma implementação com escopo definido no Autopilot.

## Contexto

Adicionar agentes de programação com IA ao processo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes. A maioria das equipes segue um fluxo parecido com este:

1. Comece por uma issue registrada que descreve o que precisa ser feito.
2. Crie um plano do que será desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, faça o merge.

> [!NOTE]
> Os detalhes variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação desse padrão.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você trabalha em conjunto com o agente. Ele sugere alterações e espera sua orientação antes de prosseguir.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de avaliação por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou um pull request (PR) do GitHub.

## Planejar a filtragem

O planejamento permite definir a abordagem antes de pedir ao Copilot que implemente uma funcionalidade ou execute uma tarefa. É sempre recomendável reservar um tempo para planejar um trabalho complexo. Vamos mudar para o modo Plan e pedir que o Copilot crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, insira este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começa a elaborar o plano: primeiro explora o projeto e depois determina a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências. Não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a página para cima e para baixo para revisar o plano.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar no modo Autopilot.

O Autopilot permite que o Copilot itere até considerar a tarefa concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção de nome semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt exibe **Autopilot**.
3. Observe o Copilot seguir o plano acordado, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois que o código for gerado, revise-o antes de fazer o merge, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo está funcionando.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador de modo não exibe **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise a saída das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente os filtros de categoria e de editora, incluindo as combinações definidas no plano.
4. Confirme que as opções de redefinição e os resultados vazios correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que interrompa o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou a funcionalidade usando o modo Autopilot.
- revisou o código.
- validou manualmente a funcionalidade em um navegador encaminhado.

Em seguida, vamos explorar como garantir que o código siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
