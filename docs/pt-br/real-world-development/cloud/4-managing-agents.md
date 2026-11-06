---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu ao agente de nuvem do Copilot três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas do agente com pull requests (PRs) abertos, explorar as operações realizadas e orientar sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois que o agente recebeu a tarefa de criar um modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o trabalho já envolve atualizar o estilo do site e adicionar alternadores, parece lógico incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente tanto o modo claro quanto o modo de alto contraste.

## Revisar as tarefas do agente de nuvem

Vamos conferir o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações já executadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel do pull request à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à adição do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no painel central](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o agente comece a trabalhar. Você verá mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente de nuvem na página de agentes, com a caixa de diálogo de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento e inclui a solicitação no fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot continuará trabalhando na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos; aproveite o tempo para refletir sobre o que aprendeu até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o painel central para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Nele, você pode monitorar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou fornecer orientações.

Você explorou estes conceitos:

- acompanhar as tarefas do agente de nuvem na página de agentes e no painel central.
- redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados por ele][next-lesson].

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
