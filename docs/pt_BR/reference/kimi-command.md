# Comando `kimi`

O `kimi` é o comando principal da CLI do Kimi Code, usado para iniciar uma sessão interativa no terminal. Executá-lo sem argumentos abre uma nova sessão no diretório de trabalho atual; combinado com diferentes flags, você pode retomar uma sessão anterior, ignorar aprovações, iniciar no modo Plano (Plan) ou carregar Skills de um diretório personalizado.

```sh
kimi [options]
kimi <subcommand> [options]
```

## Opções Principais do Comando

Todas as flags são opcionais — execute `kimi` diretamente para entrar em uma sessão interativa:

| Opção | Atalho | Descrição |
| --- | --- | --- |
| `--version` | `-V` | Exibe o número da versão e sai |
| `--help` | `-h` | Exibe informações de ajuda e sai |
| `--session [id]` | `-S` | Retoma uma sessão. Com um ID, abre essa sessão diretamente; sem um ID, entra em um seletor interativo |
| `--continue` | `-c` | Continua a sessão mais recente no diretório de trabalho atual, sem precisar especificar um ID manualmente |
| `--model <model>` | `-m` | Especifica um alias de modelo para esta inicialização. Quando omitido, novas sessões usam o `default_model` do arquivo de configuração |
| `--prompt <prompt>` | `-p` | Executa um único prompt de forma não interativa e transmite a saída do Assistente para o stdout. Este modo não abre a TUI |
| `--output-format <format>` | | Define o formato de saída não interativo; suporta `text` e `stream-json`. Só pode ser usado com `--prompt`; o padrão é `text` |
| `--yolo` | `-y` | Inicia no modo Perguntar Quando Necessário (Ask When Needed): edições rotineiras e comandos rodam automaticamente; ações arriscadas, perguntas e planos ainda exigem confirmação |
| `--auto` | | Inicia no modo Nunca Perguntar (Never Ask): nunca o interrompe; tudo roda e é decidido automaticamente |
| `--plan` | | Inicia uma nova sessão no modo Plano (Plan) — a IA priorizará ferramentas somente leitura para exploração e planejamento |
| `--skills-dir <dir>` | | Carrega Skills do diretório especificado, substituindo os diretórios de usuário e de projeto descobertos automaticamente. Pode ser repetido |
| `--agent <name>` | | Inicia uma nova sessão com o agente especificado como o Agente principal. Não pode ser combinado com `--session`/`--continue` |
| `--agent-file <path>` | | Carrega um agente personalizado a partir de um arquivo Markdown para a nova sessão e o seleciona. Não pode ser repetido ou combinado com `--agent`, `--session` ou `--continue` |
| `--add-dir <dir>` | | Adiciona um diretório de workspace extra para esta sessão. Caminhos relativos são resolvidos em relação ao diretório de trabalho atual. Pode ser repetido |

`-r` / `--resume` é um alias oculto para `--session`; `--yes` e `--auto-approve` são aliases ocultos para `--yolo` e não aparecem na saída de ajuda.

::: warning
O `--yolo` ignora a aprovação humana para chamadas de ferramentas regulares, incluindo gravações de arquivos e execução de comandos de shell. Use-o apenas em diretórios de trabalho confiáveis. A aprovação de saída do modo Plano não é ignorada pelo `--yolo`; o `Bash` dentro do modo Plano é tratado sob as regras normais de permissão.
:::

### Regras de Conflito de Flags

As seguintes combinações são rejeitadas na inicialização:

- `--continue` e `--session` são mutuamente exclusivas — ambas significam "retomar uma sessão anterior"
- `--yolo` e `--auto` são mutuamente exclusivas — os dois modos de permissão não podem ser combinados
- `--prompt` não pode ser usado com `--yolo`, `--auto` ou `--plan` — o modo não interativo usa a permissão `auto` por padrão
- `--output-format` só pode ser usado em conjunto com `--prompt`

Ao retomar uma sessão, você pode substituir a permissão salva ou o modo plano adicionando `--auto`, `--yolo` ou `--plan`. Por exemplo, `kimi --continue --auto` retoma a última sessão e a alterna para o modo Nunca Perguntar.

## Uso Comum

Iniciar uma nova sessão diretamente:

```sh
kimi
```

Continuar de onde você parou (encontra automaticamente a sessão mais recente no diretório atual):

```sh
kimi --continue
```

Escolher da lista de histórico de sessões ou especificar um ID conhecido diretamente:

```sh
kimi --session
kimi --session 01HZ...XYZ
```

Ignorar prompts de aprovação — adequado para tarefas em lote que são comprovadamente seguras:

```sh
kimi --yolo
```

Deixar o Agente lidar com tudo de forma autônoma, sem fazer perguntas ao usuário:

```sh
kimi --auto
```

Ler o código e produzir um plano de implementação antes de fazer qualquer alteração em arquivos:

```sh
kimi --plan
```

### Diretórios de Skills Personalizados

Existem duas maneiras de especificar diretórios de Skills, com semânticas diferentes:

- **`--skills-dir <dir>`** (flag de CLI): **Substitui** os diretórios de usuário e de projeto descobertos automaticamente apenas para esta inicialização. Pode ser repetido para empilhar vários diretórios:

  ```sh
  kimi --skills-dir /path/to/team-skills --skills-dir ./local-skills
  ```

- **`extra_skill_dirs`** (`config.toml`): **Adiciona** diretórios além daqueles descobertos automaticamente, aplicando-se de forma permanente. Adequado para configurar Skills compartilhadas pela equipe. Consulte [Agent Skills](../customization/skills.md).

### Agentes Personalizados

`--agent` e `--agent-file` selecionam qual agente conduz uma nova sessão, tanto no modo de impressão (`kimi -p`) quanto na TUI interativa:

```sh
kimi --agent reviewer
kimi -p --agent reviewer "Review the changes on this branch"
```

O `--agent-file` registra um único arquivo de agente com a mais alta prioridade apenas para esta inicialização e o seleciona; a flag não pode ser repetida, e `--agent` e `--agent-file` são mutuamente exclusivos. Ambas as flags só se aplicam ao iniciar uma nova sessão — nenhuma delas pode ser combinada com `--session`/`--continue`, porque o agente é vinculado na criação da sessão e a retomada restaura o agente vinculado automaticamente. A seleção é fixa no primeiro vínculo da sessão e não pode ser alterada posteriormente; na TUI, as flags vinculam apenas a sessão de inicialização, e uma sessão criada posteriormente no mesmo processo (por exemplo, via `/new`) inicia com o agente padrão. Consulte [Agentes e Subagentes](../customization/agents.md#custom-agents) para conhecer o formato do arquivo de agente e os diretórios de descoberta.

## Execução Não Interativa

Ao executar um único prompt em um script ou ambiente de CI, use `-p`:

```sh
kimi -p "Summarize the current repository status"
```

A saída usa um estilo de transcrição: o conteúdo de raciocínio (thinking) e o texto do Assistente recebem o prefixo `• `, e as linhas quebradas são recuadas por dois espaços. O texto do Assistente vai para o stdout; o raciocínio, o progresso de ferramentas e os avisos de "retomando sessão" vão para o stderr. No modo `-p`, nenhuma aprovação humana é solicitada — chamadas de ferramentas regulares são tratadas sob a política de permissão `auto`, enquanto as regras de negação estática continuam em vigor.

Alternar temporariamente o modelo:

```sh
kimi -m kimi-code/kimi-for-coding -p "Explain the latest diff"
```

Quando precisar analisar a saída programaticamente, use o formato `stream-json` — cada linha no stdout é um objeto JSON:

```sh
kimi -p "List changed files" --output-format stream-json
```

No modo `stream-json`, respostas regulares produzem uma mensagem do Assistente; quando o modelo chama uma ferramenta, uma mensagem do Assistente com `tool_calls` é emitida primeiro, seguida pela mensagem da Ferramenta correspondente e, em seguida, pelas mensagens subsequentes do Assistente. O conteúdo de raciocínio não é gravado no JSONL; o progresso de ferramentas e os avisos de "retomando sessão" ainda são gravados no stderr.
