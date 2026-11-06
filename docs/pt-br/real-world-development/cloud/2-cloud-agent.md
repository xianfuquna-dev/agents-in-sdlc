---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Pode ser uma questão de segurança ainda não resolvida, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um rascunho de pull request (PR) para uma pessoa desenvolvedora revisar. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em objetivos mais amplos.

Neste exercício, você explorará com o agente de nuvem do Copilot:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de delimitar bem as issues.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado insatisfatório: na prática, ela não existe. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre outros jogos relacionados — títulos da mesma categoria — para que as pessoas continuem explorando o catálogo. A equipe ainda não precisa de uma implementação refinada; quer algo para testar e aceitar a experiência do usuário. Esse trabalho é um impedimento, mas há issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem perder prioridade e que são ideais para delegar ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras tarefas. Depois, você pode revisar o trabalho para confirmar se tudo está como esperado.

## Sobre o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Há várias formas de atribuir trabalho, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que você atribui a tarefa, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot: não é possível pular a etapa de raciocínio. Mesmo tarefas aparentemente simples ficam complexas quando examinadas de perto; por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções já incluídos no repositório. Mais tarde, você poderá observar seus efeitos.

Abra os arquivos a seguir na interface web do GitHub ou em um Codespace que já esteja em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, principalmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que limita essas instruções aos arquivos `**/*.test.ts`.

Quando atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe se o pull request inclui comentários TSDoc, segue as convenções de TypeScript e tem cabeçalhos de comentário. Essas práticas vêm dos arquivos de instruções que você acabou de revisar. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot: ele trabalha de maneira semelhante a uma pessoa que desenvolve software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar o ambiente criando um [workflow de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes do início do trabalho. Assim, o agente terá as ferramentas e dependências necessárias. O workflow já foi configurado para este laboratório: fornece acesso ao Node.js, às dependências do projeto, aos navegadores do Playwright para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias às suas etapas. O Copilot recebe um token próprio para as operações dele.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório (por exemplo, para instalar dependências), use a permissão `contents: read`. Se não houver clonagem nessas etapas, o Copilot fará isso automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que o build e os testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com os demais workflows do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona sem esperar uma execução do Copilot.

## Adicionar documentação

Todo mundo reconhece a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica costuma ficar sem solução, reduzindo a produtividade, dificultando a manutenção da base de código e tornando mais difícil integrar novas pessoas à equipe. Felizmente, o Copilot é eficaz para criar documentação, e esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se o novo PR não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O agente está configurando o ambiente (executando o workflow descrito acima), analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema: explora o código e o contexto, faz uma pausa para pensar e definir um plano e, em seguida, cria o código.

Isso provavelmente levará alguns minutos. Uma das principais vantagens do agente de nuvem do Copilot é poder executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Vamos aproveitar esse recurso: enquanto o agente recebe outra tarefa, você começará a pensar em novas funcionalidades para o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é poder dividir o trabalho: enquanto você se concentra em um conjunto de tarefas, ele trabalha em outro. Talvez a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo, mas esse tempo pode ser usado em outras atividades. Vamos atribuir a tarefa ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo.
4. Selecione **Blank issue**.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. O agente de nuvem funciona de maneira semelhante a uma pessoa desenvolvedora: não é preciso acompanhá-lo ativamente, basta revisar o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona. Você pode resolver dívidas técnicas, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- delimitar bem as issues.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e pode acessar instruções personalizadas, que foram abordadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

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
