---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse melhorias de acessibilidade, como modos de alto contraste e claro. Agora, vamos revisar as alterações de código e, se necessário, pedir que o Copilot aprimore o resultado.

### Cenário

Como já destacamos, os fundamentos do design de software e do DevOps não mudam com a IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Vamos examinar as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e as melhorias de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e efêmero.
- Uma pessoa precisa aprovar os workflows do GitHub Actions antes de executá-los.
- [O acesso a recursos externos é limitado por padrão][agent-firewall], inclusive a servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: adicionar documentação ao código. Você fará a revisão na interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**, porque o acesso do Copilot a recursos externos, inclusive chamadas a servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR intitulado algo como **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos. Enquanto isso, faça uma pausa ou reflita sobre o que aprendeu.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui novos comentários TSDoc e outra documentação. As alterações exatas podem variar.

    Ao revisar, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles seguem os arquivos de instruções personalizadas revisados no início do Exercício 2.

6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows pendentes, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do PR. Se tudo correr bem, as verificações do projeto da aplicação Astro passarão. Isso pode levar alguns minutos.

## Pedir alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não precisa ser uma via de mão única. Você pode mencioná-lo em comentários do PR ou em comentários em linha do código, como faria com qualquer outra pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para resolvê-los. Como os resultados são não determinísticos, não há um prompt único obrigatório. Por exemplo, você pode pedir ao Copilot que:

- adicione no início de cada arquivo de código um cabeçalho breve que descreva sua finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias anteriores ou sugira outra melhoria de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Ele iniciará uma nova sessão para fazer as alterações.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit deverá aparecer no PR.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Quando estiver tudo pronto, você poderá converter o PR de rascunho em pronto para revisão e integrá-lo à branch principal.

![Converter um PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, volte ao PR criado pelo Copilot para resolver a issue sobre exibir jogos relacionados na página de detalhes do jogo.

1. Acesse novamente seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Procure um indicador de que alguns workflows estão aguardando aprovação.
7. Se houver workflows pendentes, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Aguarde os workflows aparecerem na seção de verificações do PR. Se tudo correr bem, as verificações da aplicação Astro passarão; isso pode levar alguns minutos.
9. **Opcional:** você pode mudar para essa branch no Codespace e testar manualmente os jogos relacionados. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Assim como antes, você pode pedir atualizações ao agente de nuvem. Por exemplo, peça que altere o número de jogos exibidos ou adicione cabeçalhos de comentário e comentários TSDoc — lembre-se de que essa tarefa foi atribuída **antes** de atualizar as instruções personalizadas. Para fazer essas solicitações, adicione um comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado. Esse PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado no painel de controle do Exercício 4.

1. Acesse novamente seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - os controles da interface para alternar entre os modos.
   - o uso do armazenamento local para persistir as preferências do usuário.
   - as alterações de CSS ou estilos para os modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - o código JavaScript/TypeScript responsável pela alternância de modos.

6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que alguns workflows estão aguardando aprovação.
8. Se houver workflows pendentes, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Aguarde os workflows aparecerem na seção de verificações do PR. Se tudo correr bem, as verificações da aplicação Astro passarão; isso pode levar alguns minutos.
10. **Opcional:** mude para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que funcionam como esperado e que as preferências persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo as práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como fez nos outros PRs.

## Exercício opcional — continuar delegando

O agente de nuvem funciona melhor quando você pode atribuir a ele itens reais do backlog e se dedicar a outras tarefas. Para praticar, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- Criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- Implementar paginação na página de lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizados do repositório** para observar como afetam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, incluindo os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel de controle** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e aprimorou pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional com `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outra área.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para configurar o agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
