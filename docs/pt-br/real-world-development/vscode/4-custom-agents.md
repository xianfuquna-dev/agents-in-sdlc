---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, boas práticas, padrões de código e conhecimento específico do domínio para que o Copilot realize certos tipos de trabalho com mais eficiência. As equipes podem registrar sua experiência em agentes reutilizáveis: um agente de acessibilidade que exige conformidade com as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há uma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e coordenam skills (e até outros agentes) para realizar o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Para isso, poderá chamar skills como uma de fluxo de pull requests ou outra que execute e gerencie testes.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades ou preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, queremos implementar essa funcionalidade rapidamente. Para isso, usaremos um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado existente para acessibilidade.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado para acessibilidade já foi criado. Vamos revisar seu conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace, abra um terminal e mude para uma nova branch baseada em `main` para o trabalho de acessibilidade. Assim, o PR de filtragem do Exercício 3 ficará separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - boas práticas de acessibilidade.
   - exemplos de código em HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no menu suspenso de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente para limitar uma sessão de chat às instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa indicará o que importa.

1. Se ainda não estiver selecionado, escolha **Agent** no menu suspenso de agentes da exibição Chat.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abra o menu suspenso de agentes na parte inferior da exibição Chat. Ele mostra o agente ativo; por padrão, **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o prompt a seguir ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. À medida que as alterações forem feitas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos modificados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot fará alterações reais no repositório — editará componentes Astro, CSS e testes relacionados, se houver.

No próximo exercício, você revisará e orientará esse trabalho em andamento.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você aprendeu:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando um alternador para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
