---
title: "Exercício 2 - Adicione funcionalidades com o modo Agent do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até a atualização mais simples de um aplicativo costuma exigir alterações em vários arquivos e outras tarefas, como executar testes. Normalmente, o desenvolvedor localiza os arquivos necessários, faz as alterações, executa os testes, depura, descobre qual arquivo faltou e faz outra alteração. E assim por diante.

É aí que entra o modo Agent do Copilot.

O modo Agent foi criado para atuar com mais autonomia no IDE. Ele funciona de maneira semelhante a um desenvolvedor: explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo Agent e o agente na nuvem oferecem experiências diferentes. O modo Agent executa tarefas no IDE e permite ciclos rápidos de feedback e interação. O agente na nuvem trabalha de forma assíncrona, como outra pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o modo Agent do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo Agent implementa novos recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo Agent antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. Você já adicionou um auxiliar de editoras no exercício anterior e agora concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo Agent.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site para conhecer os recursos atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema para desenvolvedores. As pessoas podem listar jogos e exibir detalhes. O site é um único aplicativo Astro, que renderiza páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM no frontmatter; não há uma API de backend separada nem framework de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com pequenos elementos `<script>` do Astro, limitados ao escopo necessário, usando as APIs DOM padrão.

### Inicie o site

Há uma tarefa `npm run dev` para iniciar o único servidor de desenvolvimento Astro. Siga estas etapas no GitHub Codespace:

1. Volte ao Codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um aviso com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura de `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link `localhost` no terminal redireciona para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que hospeda o servidor Web.

### Explore o site

Com o site em execução, explore suas funcionalidades:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá seus detalhes, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam em cada exercício; as etapas correspondentes explicarão quando necessário.

A implementação inicial do site funciona, mas queremos adicionar novos recursos. Vamos começar examinando o backlog. A criação do projeto a partir do modelo gerou issues automaticamente; peça ao GitHub Copilot para exibi-las.

1. Selecione **Agent** no menu de agentes da visualização Chat. O agente planeja e implementa alterações em vários arquivos, executa comandos no terminal e invoca ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos o Claude Sonnet 4.5 ao criar estas instruções para que as sugestões fossem relativamente consistentes. Como os LLMs são probabilísticos, suas sugestões podem ser diferentes. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar outro comando MCP e ainda concluir a tarefa.

3. Peça ao Copilot para mostrar o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens conforme a utilidade que atribuiu a cada um.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios para design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções de tipos específicos, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso facilita a reutilização, a divisão de conjuntos grandes em partes menores e o uso de exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações independentes:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento dos filtros.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser aprovados antes da mesclagem. O modo Agent do Copilot pode realizar essas tarefas.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Seleção do arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no menu de agentes.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade conforme a issue relacionada no backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos da funcionalidade, da camada de dados, da interface e dos testes. Em seguida, ele alterará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas nos testes durante o processo. Isso é normal: o Copilot alterna entre geração de código e testes até concluir a tarefa sem detectar erros.

   ![Sessão completa do modo Agent do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, exploramos como usar o modo Agent do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo Agent implementa recursos na interface e na camada de dados.
- o Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar alterações e testes gerados pelo modo Agent antes de mesclá-los à base de código.

Agora vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implemente paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Com base no que aprendeu neste exercício, peça ao Copilot para implementar a paginação. Considere:

- seguir as práticas recomendadas existentes e usar os arquivos de instruções disponíveis.
- decidir como a paginação deve funcionar, incluindo se a pessoa usuária poderá escolher o tamanho da página.
- fornecer contexto e orientações suficientes no prompt.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo Agent no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
