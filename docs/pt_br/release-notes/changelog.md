---
outline: 2
---

# Changelog

Esta página documenta as alterações em cada versão do Kimi Code CLI.

## 0.43.1 (2026-09-15)

### Funcionalidades

- Adicionar suporte nativo à área de transferência no Linux X11, para que copiar a partir da TUI não dependa mais do suporte OSC 52 do terminal.

### Aprimoramentos

- Reduzir travamentos do event loop e a sobrecarga do GC em sessões com muitos subagentes simultâneos.

### Correções de bugs

- Corrigir o comportamento em que pressionar Ctrl+C enquanto os subagentes estão em execução encerrava toda a CLI em vez de apenas interromper os subagentes.
- Corrigir a renderização progressivamente mais lenta a cada rodada de execuções grandes do AgentSwarm.
- Corrigir a memória que não era liberada quando os escopos dos subagentes eram descartados.
- Corrigir o modo tower que confundia agentes recém-criados com entradas do registro de agentes de sessões anteriores.
- Parar de retornar sessões excluídas na pesquisa global antes que o índice de pesquisa seja atualizado.
- Corrigir as cores dos links em tabelas Markdown com quebra de linha e a ordenação do preenchimento automático de arquivos com `@`.

## 0.43.0 (2026-09-14)

### Funcionalidades

- web: Os títulos de sessão gerados por IA agora estão sempre ativados — um título é gerado após o primeiro turno e pode ser regenerado pelo campo de renomear, sem necessidade de uma flag experimental.
- Excluir sessões pelo seletor de sessões: pressione Ctrl+X em uma sessão e, em seguida, y para confirmar.
- Adicionar `-y, --yes` ao `kimi upgrade` (alias `kimi update`) para ignorar a solicitação de confirmação e instalar a atualização diretamente.
- Adicionar a opção de configuração `loop_control.compaction_max_attempts` para definir o número máximo total de tentativas para uma solicitação de compactação com falha (padrão 5). Consulte [`loop_control`](../configuration/config-files.md#loop_control) para obter detalhes.

### Aprimoramentos

- Ignorar a solicitação de confirmação para comandos rm -rf que tenham como destino apenas caminhos `/tmp` ou `/temp`.
- Permitir que mensagens de direcionamento interrompam esperas por tarefas em segundo plano.
- Os limites de tempo dos objetivos não contabilizam mais o tempo em que a sessão permanece fechada, e o limite de 24 horas foi removido.
- Adicionar a variável de ambiente `KIMI_CODE_PERMISSION_MODE_REMINDER`: defina-a como `0` para impedir a injeção dos lembretes automáticos sobre o modo de permissão no contexto do modelo.

### Correções de bugs

- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.42.0 (2026-09-09)

### Funcionalidades

- O Remote Control agora está sempre ativado; a flag experimental `KIMI_CODE_EXPERIMENTAL_REMOTE_CONTROL` foi removida. Consulte [Remote Control](https://moonshotai.github.io/kimi-code/guides/remote-control.html) para mais detalhes.
- web: Suporte à exclusão permanente de sessões pelo menu de contexto da linha da sessão, com uma solicitação de confirmação.
- Adicionadas ferramentas somente leitura ao agente lateral `/btw`.
- web: Visualização de imagens e vídeos em uma barra de mídia reorganizável no compositor, permitindo mencioná-los no texto quando necessário e mantendo as visualizações após colocá-los na fila e enviá-los.
- Aceita imagens HEIC, HEIF e BMP em anexos de prompts e no `ReadMediaFile` quando o modelo é servido pelo Kimi.

### Aperfeiçoamentos

- Recolher chamadas de ferramentas concluídas na transcrição, mostrando apenas um cabeçalho e uma linha de resultado marcada: a saída curta é exibida por completo, enquanto a saída oculta é contabilizada (`N more lines`, `+N more`) e revelada por `Ctrl-O`, conforme indicado no rodapé enquanto estiver disponível.
- Atualizar o nível padrão de esforço de raciocínio para o nível recomendado para usuários elegíveis.
- O conjunto de modelos de subagentes (`[secondary_model]`) agora está sempre ativado; a flag experimental de modelo secundário e a opção de desativação `KIMI_CODE_EXPERIMENTAL_SECONDARY_MODEL` foram removidas.
- Adicionar limites de caracteres configuráveis e leitura retomável de arquivos com linhas longas, sem truncamento repetido da saída; consulte [`read`](https://moonshotai.github.io/kimi-code/configuration/config-files.html#read) para mais detalhes.
- O modelo de leitura do índice de sessões do minidb e o worker de busca global agora estão sempre ativados; as flags experimentais foram substituídas pela seção de configuração `[database]` e pelas variáveis de ambiente `KIMI_CODE_PERSISTENCE_MINIDB_READMODEL` / `KIMI_CODE_SEARCH_WORKER`; consulte [`database`](https://moonshotai.github.io/kimi-code/configuration/config-files.html#database) para mais detalhes.

### Correções de bugs

- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.41.0 (2026-09-04)

### Funcionalidades

- web: Adicionar o modo de colaboração multiagente tower (experimental), ativado pelo comando `/tower` ou pelo menu de adição do compositor; `/tower` permite especificar uma branch base (por exemplo, `/tower add-new-feature`).
- web: Adicionar anotações de seleção — selecione um texto nas mensagens, nas visualizações de arquivos, no diff e nos painéis de alterações por turno, ou no terminal, para adicionar um comentário ou citá-lo no chat.
- CLI: Adicionar uma solicitação de avaliação da sessão que convida você a avaliá-la em momentos apropriados acima da caixa de entrada.

### Aperfeiçoamentos

- O modo de permissão automática não bloqueia mais comandos perigosos e comandos que não podem ser analisados estaticamente.
- Lembrar o modelo sobre seu orçamento de contexto antes da compactação automática e, após a compactação, direcioná-lo ao log de eventos da sessão para obter detalhes exatos.
- web: Renomear os três modos de permissão para Always Ask / Ask When Needed / Never Ask e atualizar suas descrições; ao alternar para o modo de permissão Ask When Needed ou Never Ask, agora é exibido um aviso de que os arquivos podem ser modificados ou excluídos diretamente nesse modo.

- web: A tecla Esc não fecha mais o painel de detalhes à direita.
- web: Reestilizar os comandos Bash no painel à direita para o estilo de terminal.
- Entregar as respostas de perguntas em segundo plano diretamente ao agente, em vez de por meio de um arquivo de saída salvo.
- As mensagens finais de subagentes com menos de 200 caracteres não são mais enviadas de volta para expansão.

### Correções de bugs

- Corrigir o modo de impressão (`kimi -p`) que perde os registros da sessão quando a execução é encerrada devido a um erro ou a um sinal de término.
- Corrigir o modo de impressão (`kimi -p`) que ignora a variável de ambiente `KIMI_DISABLE_TELEMETRY`.
- Modo tower (experimental): corrigir o modo tower que nunca era iniciado quando ativado por meio de `[experimental] tower = true` no config.toml em vez da variável de ambiente, e fazer `/tower` funcionar em diretórios que não são repositórios Git; os erros de ativação agora informam o bloqueador real.
- Corrigir o cancelamento das perguntas em segundo plano assim que o agente termina seu turno.
- Corrigir a retomada de um subagente por seu ID de agente depois que a sessão é reaberta em um novo processo; o subagente retomado segue o modo de permissão atual e é correspondido pelo seu próprio perfil nas regras de permissão.
- web: Corrigir as visualizações de alterações de arquivos por turno que exibiam linhas adicionadas/removidas que nunca existiram e contagens de linhas imprecisas quando o mesmo arquivo era editado várias vezes em um único turno; os cartões de alterações agora exibem apenas estatísticas exatas de linhas.
- web: Corrigir o nível padrão de esforço de raciocínio nas configurações, que não podia ser definido para o nível mais alto (Max).
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.40.1 (2026-09-02)

### Correções de bugs

- Corrigir a condição para exibição da solicitação de migração do kimi-cli.

## 0.40.0 (2026-09-02)

### Funcionalidades

- web: Adicionar um painel de Plugins às Configurações para navegar pelo marketplace de plugins e instalar, ativar, desativar e remover plugins.
- web: Permitir a ativação de múltiplas skills a partir de uma única mensagem.
- Adicionar o comando `kimi session list` para listar sessões pela linha de comando.
- Modo tower (experimental, `KIMI_CODE_EXPERIMENTAL_TOWER=1`): o agente não entra mais no modo tower por conta própria — ative-o com `/tower on` ou `/tower <base-branch>`.
- A configuração de modelo do subagente (`[secondary_model]`) passa de experimental para estável.
- Bloquear comandos de shell perigosos, como shutdown, reboot ou rm -rf, no modo Auto, e sempre perguntar antes de executá-los nos modos Manual e YOLO; desative a proteção com `[permission] dangerous_command_guard = false` ou `KIMI_CODE_DANGEROUS_COMMAND_GUARD=false`.

### Aperfeiçoamentos

- Preservar comentários, ordem das chaves e formatação no config.toml quando os valores de configuração forem atualizados.
- Remover a restrição do workspace sobre o parâmetro cwd da ferramenta Bash.
- Definir como padrão a opção "Trust this folder" na solicitação de confiança do workspace, em vez de "Don't trust".
- O subcomando `kimi acp` não respeita mais `KIMI_CODE_LEGACY_FLAG`; ele sempre é executado no mecanismo de agente padrão.
- web: Adicionar uma opção para alternar a quebra de linha ao painel de diff e simplificar seu cabeçalho.

### Correções de bugs

- Respeitar entradas de configuração `[experimental]` explícitas em vez do interruptor principal `KIMI_CODE_EXPERIMENTAL_FLAG`, de modo que uma flag definida como `false` no config.toml permaneça desativada; as variáveis `KIMI_CODE_EXPERIMENTAL_<NAME>` específicas de cada recurso ainda substituem ambas.
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.41.0 (2026-09-04)

### Funcionalidades

- web: Adicionar modo de colaboração entre múltiplos agentes do tower (experimental), habilitado por meio do comando `/tower` ou do menu de mais opções do compositor; `/tower` permite especificar uma branch base (por exemplo, `/tower add-new-feature`).
- web: Adicionar anotação de seleção — selecione um texto nas mensagens, nas pré-visualizações de arquivos, nos painéis de diff e de alterações por turno ou no terminal para adicionar um comentário ou citá-lo no chat.
- CLI: Adicionar um aviso de avaliação da sessão que convida o usuário a avaliá-la nos momentos apropriados acima da caixa de entrada.

### Aprimoramentos

- O modo de permissão automática não bloqueia mais comandos perigosos e comandos que não podem ser analisados estaticamente.
- Lembrar o modelo de seu limite de contexto antes da compactação automática e, após a compactação, direcioná-lo para o log de eventos da sessão para obter detalhes exatos.
- web: Renomear os três modos de permissão para Always Ask / Ask When Needed / Never Ask e atualizar suas descrições; ao mudar para o modo de permissão Ask When Needed ou Never Ask, agora é exibido um aviso de que os arquivos podem ser modificados ou excluídos diretamente nesse modo.
- web: Esc não fecha mais o painel de detalhes à direita.
- web: Reformular os comandos Bash no painel à direita no estilo de terminal.
- Entregar as respostas de perguntas em segundo plano diretamente ao agente, em vez de por meio de um arquivo de saída salvo.
- As mensagens finais do subagente com menos de 200 caracteres não são mais reenviadas para expansão.

### Correções de bugs

- Corrigir o modo de impressão (`kimi -p`) que perdia os registros da sessão quando a execução era encerrada devido a um erro ou a um sinal de término.
- Corrigir o modo de impressão (`kimi -p`) que ignorava a variável de ambiente `KIMI_DISABLE_TELEMETRY`.
- Modo tower (experimental): corrigir o modo tower que nunca era iniciado quando habilitado por meio de `[experimental] tower = true` no config.toml em vez da variável de ambiente, e fazer `/tower` funcionar em diretórios que não são repositórios Git; os erros de habilitação agora identificam o bloqueio real.
- Corrigir perguntas em segundo plano que eram canceladas assim que o agente terminava seu turno.
- Corrigir a retomada de um subagente por seu ID de agente depois que a sessão era reaberta em um novo processo; o subagente retomado segue o modo de permissão atual e é associado ao seu próprio perfil nas regras de permissão.
- web: Corrigir as pré-visualizações de alterações de arquivos por turno que exibiam linhas adicionadas/removidas que nunca existiram e contagens de linhas imprecisas quando o mesmo arquivo era editado várias vezes em um único turno; os cartões de alteração agora mostram apenas estatísticas exatas de linhas.
- web: Corrigir a configuração do nível de esforço de raciocínio padrão que não podia ser definida para o nível mais alto (Max).
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.40.1 (2026-09-02)

### Correções de bugs

- Corrigir a condição para exibir o aviso de migração do kimi-cli.

## 0.40.0 (2026-09-02)

### Funcionalidades

- web: Adicionar um painel de Plugins às Configurações para navegar pelo marketplace de plugins e instalar, habilitar, desabilitar e remover plugins.
- web: Permitir a ativação de várias skills a partir de uma única mensagem.
- Adicionar o comando `kimi session list` para listar sessões pela linha de comando.
- Modo tower (experimental, `KIMI_CODE_EXPERIMENTAL_TOWER=1`): o agente não entra mais no modo tower por conta própria — ative-o com `/tower on` ou `/tower <base-branch>`.
- A configuração do modelo de subagente (`[secondary_model]`) deixa de ser experimental e passa a ser estável.
- Bloquear comandos de shell perigosos, como shutdown, reboot ou rm -rf, no modo Auto, e sempre perguntar antes de executá-los nos modos Manual e YOLO; desabilite a proteção com `[permission] dangerous_command_guard = false` ou `KIMI_CODE_DANGEROUS_COMMAND_GUARD=false`.

### Aprimoramentos

- Preservar comentários, ordem das chaves e formatação no config.toml quando os valores de configuração forem atualizados.
- Remover a restrição do workspace sobre o parâmetro cwd da ferramenta Bash.
- Definir como padrão a opção "Trust this folder" na solicitação de confiança do workspace, em vez de "Don't trust".
- O subcomando `kimi acp` não respeita mais `KIMI_CODE_LEGACY_FLAG`; ele sempre é executado pelo mecanismo de agente padrão.
- web: Adicionar uma opção de quebra de linha de código ao painel de diff e simplificar seu cabeçalho.

### Correções de bugs

- Respeitar entradas explícitas de configuração `[experimental]` em vez da chave mestra `KIMI_CODE_EXPERIMENTAL_FLAG`, de modo que uma flag definida como `false` no config.toml permaneça desativada; as variáveis `KIMI_CODE_EXPERIMENTAL_<NAME>` específicas de cada recurso ainda substituem ambas.
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.39.1 (2026-08-28)

### Correções de bugs

- web: Corrigir a alteração do modo de permissão em uma sessão que modificava o modo de todas as sessões; o modo de permissão agora é específico para cada sessão.
- web: Corrigir usuários conectados sem um modelo utilizável que eram solicitados incorretamente a fazer login (e ficavam presos nessa tela na web); a etapa de envio agora oferece a opção de selecionar ou configurar um modelo.
- web: Corrigir o primeiro caractere do IME (ou teclado) que era silenciosamente perdido após clicar no espaço reservado do compositor.
- web: Corrigir anexos em uma sessão recém-criada que continuavam sendo exibidos como se estivessem sendo enviados após o upload ter sido concluído.
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.39.0 (2026-08-27)

### Funcionalidades

- Adicionar o Remote Control como um recurso experimental para acessar remotamente uma sessão web local. Habilite-o com `KIMI_CODE_EXPERIMENTAL_REMOTE_CONTROL=1` e, em seguida, execute `kimi rc`, `kimi web --remote-control` ou `/remote-control` para iniciá-lo.
- Adicionar o modo tower experimental para orquestração de múltiplos agentes; defina `KIMI_CODE_EXPERIMENTAL_TOWER=1` e, em seguida, execute `/tower on` e `/tower <objective>` para iniciá-lo.
- Adicionar um parâmetro opcional `fork` às ferramentas subagent e swarm, que inicia o subagente com um instantâneo do histórico de conversas do agente que o chamou; defina `KIMI_CODE_EXPERIMENTAL_SUBAGENT_FORK=1` ou `subagent_fork = true` em `[experimental]` no config.toml para habilitá-lo.
- web: Permitir mover um comando Bash ou subagente em primeiro plano em execução para segundo plano por meio do botão "Move to background" no cartão em execução.
- web: Adicionar uma aba flat/por workspace à lista de sessões para dispositivos móveis.
- Adicionar o plugin Tencent CloudBase ao marketplace selecionado.
- Adicionar uma opção de configuração dedicada `[swarm] timeout_ms` (ou a variável de ambiente `KIMI_CODE_SWARM_TIMEOUT_MS`) para os tempos limite dos subagentes do AgentSwarm, que não seguem mais `[subagent] timeout_ms`.

### Aprimoramentos

- web: Reformular a barra lateral direita como um painel com várias abas.
- web: Melhorar a interação do compositor, incluindo a apresentação de anexos de arquivos, pastas e mídia.
- web: Melhorar o estilo da interface de usuário para dispositivos móveis.

### Correções de bugs

- Corrigir as ferramentas de arquivos e os diretórios de trabalho do shell que falhavam ao resolver caminhos do Git Bash, como /c/Users ou /tmp no Windows.
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.38.0 (2026-08-20)

### Funcionalidades

- Suportar dois métodos de login OAuth — kimi.ai e kimi.com.
- Adicionar a ferramenta WaitFor: o agente agora pode aguardar a conclusão de uma tarefa em segundo plano durante o turno atual, em vez de encerrar o turno e ser reinvocado.
- Adicionar 13 fontes de dados ao plugin oficial Kimi Datasource — dados do governo chinês (NDA/NBS) e padrões (GB/HB/DB/TT), oito conjuntos de dados de organizações internacionais (WHO, FAO, UNSD, ECB, Eurostat, UNICEF, OECD, FRED), Xinhua Finance e Caixin. Atualize o plugin pela aba Official em /plugins.
- web: Adicionar uma ação Pin ao menu de mais opções do cabeçalho do chat.

### Aprimoramentos

- Edit e Write agora exigem a leitura de um arquivo existente antes de modificá-lo.
<!-- - Os subagentes não iniciam mais seus próprios subagentes por padrão; perfis de agentes personalizados ainda podem permitir isso explicitamente. -->
- Recolher a saída longa de comandos shell `!` em vez de inundar o histórico. Pressione ctrl+o para expandir ou recolher a saída junto com a saída da ferramenta.

### Correções de bugs

- Corrigir entradas do config.toml que eram perdidas quando o arquivo apresentava um erro de sintaxe ou era editado fora do aplicativo.
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.37.2 (2026-08-19)

### Aprimoramentos

- web: As Configurações agora possuem uma aba Lab com uma nova opção de alternância da barra lateral com várias abas; quando habilitada, a barra lateral exibe as abas Open / Done / Workspaces.
- Realizar diversos aprimoramentos e melhorias internas. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.37.1 (2026-08-18)

### Correções de bugs

- Corrigir imagens e vídeos colados que não conseguiam chegar ao modelo.

## 0.37.0 (2026-08-18)

### Funcionalidades

- Ativar várias skills em um único prompt. Digite `/` após um espaço em branco para inserir um token de skill.
- A CLI nativa do Windows (binário único) agora oferece suporte a atualizações automáticas.
- web: A barra lateral ganha as abas Open / Done / Workspaces, e as sessões podem ser marcadas como concluídas.
- web: Adicionar uma página de gerenciamento de sessões.

### Aprimoramentos

- Colocar na fila os comandos de skills iniciados com `/` enquanto o agente estiver ocupado, em vez de rejeitá-los.
- web: Arquivos, pastas e skills mencionados com @ nas mensagens do chat agora são exibidos como ícones em formato de pílula.
- web: O título da aba do navegador agora exibe o nome do diretório do workspace atual.
- web: A caixa de diálogo de pesquisa agora também encontra workspaces, e ao selecionar um resultado de workspace ou sessão, a barra lateral é expandida e o item é colocado em evidência por meio da rolagem.
- web: Renomear o painel Subagent para "Background Agent".
- Avisar quando um objetivo `/goal` digitado ultrapassar o limite de 4.000 caracteres e manter a entrada caso ela seja rejeitada.

### Correções de bugs

- Corrigir sessões de chamadas de ferramentas do Gemini que falhavam em solicitações subsequentes.
- web: Corrigir o Ctrl+K no compositor que abria a pesquisa de sessões no macOS — a pesquisa de sessões agora responde somente ao Cmd+K.
- web: Corrigir o painel Background Agent que exibia contagens e status de tarefas incorretos.
- web: Corrigir a falha ao colar uma pasta copiada no compositor, que causava um erro de conexão durante o upload — as pastas agora são ignoradas.
- Corrigir vários problemas conhecidos e realizar diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver entradas mais técnicas.

## 0.36.1 (2026-08-14)

### Recursos

- web: Gerar títulos de sessão com IA (experimental). Desativado por padrão — defina `KIMI_CODE_EXPERIMENTAL_AUTO_SESSION_TITLE=1` (ou a flag principal `KIMI_CODE_EXPERIMENTAL_FLAG=1`) para ativá-lo.

### Melhorias

- web: Aprimorar os controles de Plano, Objetivo e Enxame no compositor, que agora ficam no menu + ao lado da caixa de entrada.

### Correções de bugs

- Corrigir vários problemas conhecidos e fazer diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver mais detalhes técnicos.

## 0.36.0 (2026-08-13)

### Recursos

- Atualizar a configuração experimental do modelo de subagente para um pool de modelos: a seção `[secondary_model]` agora pode conter um conjunto de modelos candidatos com descrições, e o agente principal escolhe entre eles a cada inicialização com base na tarefa.

Defina `KIMI_CODE_EXPERIMENTAL_SECONDARY_MODEL=1` (ou a flag principal `KIMI_CODE_EXPERIMENTAL_FLAG=1`) antes de iniciar o Kimi para ativá-lo.

Configurações recomendadas:

Mínima: execute `/secondary-model` na TUI ou escreva uma única linha `default_model` no `config.toml`, para fazer com que todos os subagentes usem o mesmo modelo por padrão; adicione `force = true` para fixar essa escolha, impedindo que o agente principal a substitua.

Declare um pool nomeado com uma descrição de uma linha para cada alias — as descrições são o que o agente principal vê ao fazer a escolha:

```toml
[secondary_model]
default_model = "kimi-code/kimi-for-coding-highspeed"
[secondary_model.models]
"kimi-code/kimi-for-coding-highspeed" = "Fast and cheap — good for daily refactoring, code explanation, and small edits."
"kimi-code/k3" = "Strong at complex reasoning and deep debugging — pick it for hard problems."
```

Consulte a [documentação do pool de modelos de subagentes](https://moonshotai.github.io/kimi-code/en/configuration/config-files.html#subagent-model-pool) para mais detalhes.
- Adicionar um modo TUI experimental em tela cheia. Defina a variável de ambiente `KIMI_CODE_TUI_FULL_SCREEN=1` para ativá-lo.
- Suportar a renderização de fórmulas matemáticas em LaTeX (`$…$` / `$$…$$`) nas mensagens da TUI como fórmulas Unicode.

### Correções de bugs

- Mostrar os alvos de inicialização do MCP do projeto no aviso de confiança do workspace, definir como padrão a recusa de confiança e resolver os binários `fd` e `stty` para caminhos absolutos, para que workspaces não confiáveis não possam inserir executáveis com nomes simples antes da confirmação.
- Corrigir sessões que falhavam com um erro 400 do provedor em todas as solicitações seguintes após um turno ser interrompido enquanto o modelo ainda estava pensando, em provedores compatíveis estritamente com OpenAI (por exemplo, DeepSeek).
- Corrigir o Ctrl+C sendo ignorado durante novas tentativas automáticas de solicitações de API que falharam.
- Corrigir vários problemas conhecidos e fazer diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver mais detalhes técnicos.

## 0.35.0 (2026-08-12)

### Recursos

- Adicionar o plugin Modern Web Guidance ao marketplace de plugins incluído. Execute `/plugins` e selecione Modern Web Guidance para instalá-lo.
- Exibir o progresso de trabalho em tempo real dos subagentes em segundo plano no painel `/tasks`.

### Correções de bugs

- Corrigir subagentes coder que iniciavam outros subagentes por padrão.
- Corrigir as contagens de tokens informadas após a compactação que ficavam muito abaixo do tamanho real do contexto; agora elas correspondem aos números exibidos enquanto a sessão está em execução.
- Corrigir dois riscos de inserção de binários no Windows.
- Corrigir vários problemas conhecidos e fazer diversos aprimoramentos. Consulte o [changelog no GitHub](https://github.com/MoonshotAI/kimi-code/blob/main/apps/kimi-code/CHANGELOG.md) para ver mais detalhes técnicos.

## 0.34.0 (2026-08-06)

### Recursos

- web: Adicionar uma visualização plana à lista de sessões da barra lateral.
- O plugin Kimi Computer Use agora oferece suporte ao Windows x64 — instale-o pelo `/plugins`.
- Exibir um lembrete de expiração do cache ao retomar ou enviar após um longo período de inatividade. Defina [`cache_expiry_hint`](https://moonshotai.github.io/kimi-code/en/configuration/config-files.html#tui-toml) como `false` para desativá-lo.

### Aprimoramentos

- web: As tarefas de subagentes exibem seu modelo e nível de raciocínio.
- web: Exibir um cartão de falha com opção de retomar com um clique quando uma solicitação ao modelo falhar.
- web: Exibir o progresso das tentativas (tentativa N de M) no status de execução durante novas tentativas automáticas.
- Exibir links da extensão do navegador e as etapas de ativação após instalar o Kimi WebBridge.

### Correções de bugs

- Corrigir arquivos de texto UTF-16 LE/BE (com ou sem BOM) que falhavam ao carregar.
- web: Corrigir anexos que eram descartados quando enviados com um comando de skill.
- web: Corrigir o seletor de modelos que ultrapassava os limites da tela quando muitos modelos estavam disponíveis.
- web: Corrigir um caminho de arquivo com espaços que abria a pasta Documentos em vez do arquivo no Windows.
- web: Corrigir o nível de raciocínio que era redefinido para o padrão do modelo quando uma nova sessão era iniciada com um comando de skill.
- web: Corrigir sessões canceladas manualmente que exibiam um marcador de erro na barra lateral; agora ele aparece somente quando o último turno falha.
- web: Corrigir a composição de IME ao renomear uma sessão — Enter e Esc não atuam mais durante a composição.
- web: Corrigir o arraste para selecionar texto durante a renomeação que movia o item inteiro da lista.
- web: Corrigir os indicadores de tarefas em segundo plano e todos que pulavam para o topo quando a caixa de diálogo de aprovação do plano era expandida.
- web: Corrigir a direção do chevron no botão "mostrar menos" do cartão de resumo de arquivos alterados.
- Corrigir o kimi -p que era encerrado antes que as tarefas em segundo plano e os subagentes terminassem.
- /feedback agora funciona para usuários conectados em qualquer modelo; usuários desconectados veem a página de cadastro e os links do GitHub Issues.
- Corrigir a remoção de um servidor MCP que quebrava sessões abertas: suas ferramentas continuam visíveis, mas as chamadas falham com um aviso de remoção.
- Corrigir o resultado do último turno que era perdido após reinicializações do servidor — turnos com falha agora continuam marcados nas listas de sessões e nas sessões retomadas.
- Corrigir sessões retomadas que exibiam a conclusão de tarefas em segundo plano como texto bruto do protocolo em vez de um cartão de status.

## 0.33.0 (2026-08-05)

### Recursos

- Adicionar o Kimi Computer Use e o Kimi WebBridge como entradas oficiais integradas do marketplace na CLI v2. A instalação pelo `/plugins` configura o runtime gerenciado e o plugin mais recentes, informa etapas manuais incompletas e permite tentar novamente uma configuração interrompida.
- web: Adicionar e gerenciar provedores personalizados nas configurações.
- web: Fixar sessões no topo da barra lateral.
- web: Definir um emoji para o título da sessão.
- web: Exibir a conta conectada e o uso do plano.
- Adicionar `/bug` como um alias para o comando slash `/feedback`. Digite `/bug` para enviar feedback.

### Aprimoramentos

- Perguntar se o usuário confia na pasta atual durante a inicialização.
- `/fork` não muda mais para a sessão criada a partir do fork: a sessão atual permanece ativa e suas tarefas em segundo plano continuam em execução. Encontre o fork em `/sessions`.
- web: Reformular a interface e a experiência do usuário (UI/UX) e corrigir problemas conhecidos.
- Iniciar a TUI interativa sem criar uma sessão.
- Renomear a aba do marketplace de plugins parceiros para Curated e esclarecer que ela contém plugins de terceiros dos parceiros da Kimi.

### Correções de bugs

- Corrigir todas as chamadas de ferramentas que falhavam com spawn EBADF no macOS quando uma pasta de skill continha uma árvore de arquivos muito grande.
- Corrigir a reautorização OAuth do MCP que sempre falhava com "Invalid redirect URI"; o registro de cliente antigo agora é descartado e recriado com a URI de callback atual.
- Garantir que a primeira solicitação aguarde a conclusão da inicialização do MCP enquanto a interface continua sendo aberta imediatamente.
- Os resultados das ferramentas MCP agora disponibilizam ao modelo o campo `structuredContent` definido pela especificação e os metadados do servidor `_meta`, em vez de descartá-los silenciosamente, para que servidores que retornam seu contrato legível por máquina nesses campos funcionem da mesma forma que em outros hosts MCP.
- Corrigir a disponibilidade dos recursos integrados e o status de instalação em `/plugins`, preservar as skills legadas do WebBridge como backups durante as atualizações e impedir que as atualizações do Computer Use dupliquem ou desconectem servidores MCP.

### Refatorações

- Executar as interfaces da CLI (TUI interativa, `kimi -p`, `kimi acp`, `kimi export`, `kimi provider`) no mecanismo agent-core-v2 por padrão. Defina `KIMI_CODE_LEGACY_FLAG=1` para voltar ao mecanismo legado.

## 0.32.0 (2026-08-04)

### Recursos

- Adicionar quatro eventos de hook: `TurnStarted`, `UserPromptQueued`, `TaskStarted` e `SessionHeartbeat`. Configure-os em `[[hooks]]` no `config.toml` — consulte [Hooks](https://moonshotai.github.io/kimi-code/en/customization/hooks.html) para mais detalhes.

### Aprimoramentos

- Renomear duas chaves de `[loop_control]`: `max_retries_per_step` → `max_attempts_per_step` e `max_steps_per_run` → `max_steps_per_turn`; as chaves antigas deixam de funcionar e exibem um aviso sobre a renomeação na inicialização — consulte [loop_control](https://moonshotai.github.io/kimi-code/en/configuration/config-files.html#loop-control).
- Adicionar uma seção de configuração `[token_counting]`: quando um provedor não informar o uso de tokens, alternar a exibição do tamanho do contexto para estimativas locais — consulte [token_counting](https://moonshotai.github.io/kimi-code/en/configuration/config-files.html#token-counting).

### Correções de bugs

- Corrigir respostas a prompts de perguntas interativas que eram rejeitadas quando o provedor do modelo retornava IDs de chamadas de ferramentas contendo dois-pontos (alguns gateways compatíveis com OpenAI).
- Corrigir a compactação automática do contexto que ficava presa tentando novamente uma solicitação grande demais até falhar.
- Usar como alternativa o snapshot integrado do catálogo models.dev quando o catálogo público estiver inacessível, para que a importação de um provedor conhecido continue funcionando offline ou em redes bloqueadas.
- Corrigir o limite da janela de contexto que era exibido como 0 quando nenhum modelo estava configurado; agora ele usa o modelo padrão como alternativa.
- web: Corrigir os controles monocromáticos do modo escuro e alinhar o raio dos cantos do compositor de chat ao sistema de design.
- Corrigir a confirmação de `/login` para usuários que já estavam conectados, que era difícil de ler; agora ela usa a cor de sucesso.

## 0.31.1 (2026-07-31)

### Aprimoramentos

- Reduzir redesenhos frequentes de tela inteira na TUI.
- Preservar a saída parcial do assistente quando um turno é interrompido com Esc e lembrar o modelo de que o turno anterior foi interrompido deliberadamente.
- web: Ordenar os modos de permissão do mais seguro ao mais permissivo nas diferentes áreas de configurações e corrigir as cores de risco invertidas de yolo/auto no painel de status e nas configurações para dispositivos móveis.
- web: Habilitar o destaque baseado no Monaco para blocos de código e corrigir números de linha que se sobrepunham ou ficavam desalinhados em blocos de código renderizados pelo mecanismo alternativo.

### Correções de bugs

- Corrigir erros ocasionais de "modelo não configurado" ao iniciar o kimi web, causados pela atualização de provedores em segundo plano que limpava temporariamente o catálogo de modelos enquanto a primeira sessão estava sendo criada.
- web: Corrigir novas sessões que exibiam o nível de raciocínio (por exemplo, Max) enquanto a primeira mensagem era executada com o raciocínio desativado.
- web: Fazer a menção de arquivos com @ funcionar em um rascunho de uma nova sessão, antes que o primeiro prompt crie a sessão.
- web: Corrigir blocos de código do chat que eram renderizados com a fonte proporcional da interface em tamanho incorreto após a atualização do renderizador Markdown e alinhar o fallback de carregamento ao bloco com destaque.

## 0.31.0 (2026-07-30)

### Recursos

- Suportar agentes personalizados definidos em Markdown no agent-core.
- Adicionar o comando slash /secondary_model para configurar o modelo secundário usado pelos subagentes (experimental; habilite-o primeiro em /experiments).
- Plugins podem contribuir com agentes personalizados, que são descobertos automaticamente e ficam disponíveis para delegação aos subagentes.
- Plugins podem contribuir com instruções de prompt do sistema por meio de `systemPrompt` ou `systemPromptPath` em `kimi.plugin.json`.

### Correções de bugs

- Remover a espera bloqueante `block`/`timeout` da ferramenta TaskOutput para que verificar uma tarefa em segundo plano não possa mais travar a conversa; ela agora sempre retorna um snapshot imediato, e a conclusão continua sendo informada por meio de uma notificação automática.
- Corrigir sessões que não apareciam no seletor de sessões quando seus metadados em cache eram anteriores à flag de arquivamento.
- Corrigir cabeçalhos de solicitação que não eram enviados corretamente em algumas solicitações.

## 0.30.0 (2026-07-29)

### Recursos

- Adicionar uma linha de status personalizável no rodapé, configurada por meio de `[status_line]` em `tui.toml`.

### Aprimoramentos

- Exibir uma observação sobre a cota após instalar plugins oficiais que utilizam a cota do plano (como o Kimi Datasource).
- Exibir um aviso quando um plugin oficial usado na sessão tiver uma atualização disponível — execute /plugins para atualizá-lo.
- Remover o limite de tamanho de 50 MB para uploads de arquivos no servidor integrado.

### Correções de bugs

- Falhar imediatamente quando a cota ou o saldo da conta estiver esgotado, em vez de tentar novamente silenciosamente por ~3 minutos.
- Encerrar o turno após chamadas de ferramentas inválidas repetidas, em vez de tentar novamente indefinidamente.
- web: Corrigir números de linha corrompidos em blocos de código.

## 0.29.2 (2026-07-27)

### Correções de bugs

- Corrigir a pausa do cumprimento de objetivos quando um turno de objetivo atinge o limite de etapas por turno (`loop_control.max_steps_per_turn`).
- Corrigir mensagens enviadas durante o cumprimento de objetivos que eram rejeitadas.
- Corrigir o /undo para restaurar de forma consistente o histórico da conversa, as listas de tarefas, o modo de planejamento e as notificações de tarefas.
- web: Corrigir a cópia de texto selecionado do chat por HTTP simples que sobrescrevia a área de transferência com um marcador de evento.

## 0.29.1 (2026-07-24)

### Recursos

- Adicionar tempos limite globais padrão para servidores MCP no `config.toml` e nas variáveis de ambiente.
- Adicionar variáveis de ambiente para configurar os serviços de pesquisa e busca na web sem login OAuth.
- Adicionar associações experimentais de modelos secundários para subagentes recém-iniciados, incluindo preferências de modelo por agente e substituições de modelo exclusivas para subagentes.

### Correções de bugs

- Corrigir a perda do conteúdo de raciocínio em endpoints compatíveis com OpenAI que retornam o raciocínio usando um nome de campo diferente (por exemplo, versões mais recentes do vLLM).

## 0.29.0 (2026-07-22)

### Funcionalidades

- web: Suporte à definição de agentes em arquivos Markdown, declarando prompt do sistema, nome, descrição e permissões de ferramentas. [Detalhes](https://moonshotai.github.io/kimi-code/en/customization/agents.html#agent-file-format)
- web: Substituição permanente do prompt do sistema do agente principal usando SYSTEM.md. [Detalhes](https://moonshotai.github.io/kimi-code/en/customization/agents.html#overriding-the-main-agent-s-system-prompt-with-system-md)
- web: Ativação ou desativação global de ferramentas em todas as sessões por meio do config.toml. [Detalhes](https://moonshotai.github.io/kimi-code/en/configuration/config-files.html#tools)
- Vídeos anexados a um prompt agora chegam ao modelo junto com o prompt, sem uma rodada adicional de ferramentas.
- Suporte à seleção de um nível de thinking effort a partir de clientes ACP.
- Adição de substituições por variáveis de ambiente para os limites do agent loop e das background tasks.

### Melhorias

- Importação de muito mais provedores do catálogo models.dev.
- Melhoria do desempenho do TUI e da velocidade de retomada de sessões de longa duração.
- Reconexão automática de uma conexão perdida com um servidor MCP quando uma de suas ferramentas é chamada, com uma nova tentativa da chamada.
- Remoção da cor vermelha do syntax highlighting nas pré-visualizações de código e nos blocos de código Markdown.
- Adição de um lembrete para fontes de instalação de terceiros utilizarem o instalador oficial no prompt de atualização.

### Correções de bugs

- Correção de sessões que ficam travadas com o erro de provedor "message must not be empty" após uma resposta filtrada por conteúdo.
- Correção de solicitações de modelo canceladas que estavam sendo tratadas como erros de provedor que permitem novas tentativas.
- Correção da exibição de níveis de thinking para modelos que não oferecem suporte a eles.
- Correção de substituições de configuração por variáveis de ambiente que estavam sendo persistidas no config.toml enquanto a variável de ambiente estivesse definida.
- Envio da chave de cache do prompt da sessão para os provedores OpenAI e OpenAI Responses.
- Correção da falha do ReadMediaFile em vídeos quando o provedor não possui um canal de upload de arquivos.
- Correção de prompts de continuação do goal mode que vazavam para o transcript ao retomar uma sessão.
- web: Exibição de imagens transparentes sobre um canvas quadriculado.
- Remoção de referências ao comando inexistente `kimi resume` das descrições das ferramentas de tarefas agendadas.

## 0.28.1 (2026-07-20)

### Funcionalidades

- Permitir que sessões ACP sejam iniciadas com credenciais de modelo não-OAuth configuradas, em vez de exigir login pelo terminal.

### Melhorias

- Executar servidores web somente em primeiro plano de ponta a ponta: o comando /web agora sempre inicia um novo servidor, e os subcomandos `kimi web kill` / `kimi web ps` foram removidos — servidores em primeiro plano são encerrados com Ctrl+C. `kimi server kill` continua disponível como alternativa obsoleta que só encerra servidores iniciados por uma versão anterior à 0.28.0.

### Correções de bugs

- Corrigir subagentes em execução que não observavam alterações no modo de permissão feitas após seu início.

## 0.28.0 (2026-07-20)

### Funcionalidades

- **Breaking:**
  - A árvore de comandos `kimi server` está obsoleta; use `kimi web` em seu lugar.
  - `kimi web` agora é executado em primeiro plano no terminal atual e abre o navegador; encerre-o com Ctrl+C.

### Melhorias

- O thinking effort persiste apenas nos níveis abaixo do nível máximo do modelo (max).
- web: Adição de uma observação no seletor de modelos informando que a troca de modelo ou de thinking effort invalida o prompt cache existente.

### Correções de bugs

- Correção das descrições dos modos de permissão YOLO e Auto: YOLO aprova automaticamente as ações das ferramentas, mas o agente ainda pode fazer perguntas, enquanto Auto é totalmente autônomo e nunca faz perguntas.
- Correção do backend web que ignorava links simbólicos ao carregar arquivos AGENTS.md e ler arquivos.

## 0.27.0 (2026-07-17)

### Funcionalidades

- Adição do comando /copy para copiar a última mensagem do assistente para a área de transferência.
- O uso de uma chave de API para modelos de programação Kimi agora também busca automaticamente a lista de modelos mais recente.

### Melhorias

- Erros de conexão OAuth agora incluem a causa de rede subjacente (DNS, conexão recusada, TLS ou timeout), em vez de apenas "fetch failed".

### Correções de bugs

- Correção de rejeições repetidas de solicitações após uma resposta de modelo interrompida.
- Correção das proteções de rede da ferramenta integrada de busca de URLs: domínios manipulados e cadeias de redirecionamento não podem mais acessar serviços de loopback ou da rede interna.
- web: Correção de fórmulas LaTeX que eram renderizadas como texto sobreposto e ilegível quando a interface web era acessada pela rede.
- web: Correção de mensagens enfileiradas que reenviavam silenciosamente arquivos enviados anteriormente quando uma sessão era reaberta.
- web: Lembrança do nível de thinking por modelo, corrigindo um seletor de thinking vazio e sem resposta quando o modelo não oferece suporte ao nível armazenado.
- web: Correção de grupos de workspaces duplicados no Windows quando a mesma pasta é aberta com diferentes formas de escrever o caminho; agora suas sessões são listadas em um único grupo mesclado.
- Correção de arquivos AGENTS.md instalados como links simbólicos que eram ignorados pelo backend web.
- Correção de Esc e Ctrl+C que cancelavam a compactação em vez de fechar um painel /btw aberto.
- Correção do conteúdo de thinking composto apenas por espaços em branco, que era renderizado como uma linha vazia no transcript.
- Correção de `/export-debug-zip` e `kimi export`, que sobrescreviam o ZIP anterior em execuções repetidas para a mesma sessão; o nome de arquivo padrão agora inclui um timestamp.

## 0.26.0 (2026-07-16)

### Melhorias

- Expansão do conjunto de ferramentas do subagente coder para incluir tarefas em segundo plano, listas de tarefas, modo plan, invocação de skills e agentes aninhados, espelhando as capacidades do agente principal.
- Aviso nos seletores `/model` e `/effort` de que a troca invalida o prompt cache existente, com uma sugestão para usar `/new` e evitar custos adicionais de tokens.
- web: Atualização do catálogo de modelos de todos os provedores ao abrir o seletor de modelos, para que os modelos recém-disponibilizados sejam sempre exibidos.
- Otimização da formatação das unidades na exibição de uso do contexto.

### Correções de bugs

- Correção de uma sessão retomada que era marcada como recém-atualizada e movida para o topo da lista de sessões sem nenhuma nova atividade.
- Correção do indicador de tamanho do contexto que informava um uso de contexto inferior ao real do modelo.
- Correção de modelos do provedor Kimi encaminhados pelo protocolo Anthropic que exibiam incorretamente opções de esforço de raciocínio.
- Respeito à configuração explícita de thinking como "off" em provedores compatíveis com OpenAI (chat completions).
- Exibição de uma informação quando os usuários interrompem tarefas e preservação dos outros motivos de interrupção no contexto do modelo.
- Correção de uma condição de corrida em que retomar um subagente em segundo plano logo após ele ser interrompido manualmente poderia falhar com um erro de "already running".
- Reprodução do conteúdo vazio de thinking literalmente, em vez de substituí-lo por um espaço como placeholder em endpoints compatíveis com Anthropic e Kimi com thinking preservado.
- Manutenção das migrações legadas idempotentes em múltiplos diretórios Kimi e exibição de sessões danificadas ou não mapeadas em vez de ignorá-las silenciosamente.
- web: Correção da alça de redimensionamento da barra lateral que ficava coberta pelo fundo do compositor de chat.

## 0.25.0 (2026-07-16)

### Funcionalidades

- web: Anexar qualquer tipo de arquivo no chat — arquivos podem ser arrastados para qualquer lugar da janela, e arquivos, imagens e vídeos enviados são exibidos como chips no balão da mensagem.

### Melhorias

- web: Exibir diagnósticos completos para falhas nas solicitações ao modelo.
- Aplicar os perfis de esforço oficiais da Anthropic e um fallback de saída de 128k para modelos desconhecidos.

### Correções de bugs

- Corrigir a verificação do bearer token do servidor web que podia ser contornada por caminhos de API codificados em porcentagem, permitindo acesso não autenticado a todas as rotas da API.
- Corrigir a API do sistema de arquivos da sessão que seguia links simbólicos apontando para fora do workspace, permitindo acesso a arquivos do host além do diretório da sessão.
- web: Manter os indicadores de atividade da sessão sincronizados com o trabalho do agente e impedir conteúdo transmitido duplicado após condições de corrida na ativação da sessão ou novas tentativas do LLM.
- Corrigir modelos com nomes personalizados em provedores compatíveis com Anthropic que iniciavam novas sessões com o thinking desativado e não exibiam o controle de thinking em clientes ACP.
- Respeitar `adaptive_thinking = false` em modelos compatíveis com Anthropic, omitindo o parâmetro de esforço das solicitações.
- web: Corrigir a Content-Security-Policy em servidores vinculados a endereços que não são de loopback, que bloqueava o script de inicialização do tema da interface web e as fontes incluídas no pacote.
- Corrigir sessões que não conseguiam ser criadas quando o diretório do workspace era fornecido por meio de um link simbólico.
- Corrigir a CLI que era encerrada inesperadamente quando a leitura de uma imagem da área de transferência falhava; agora ela usa texto colado como alternativa.
- web: Corrigir subagentes em segundo plano concluídos que perdiam sua saída final após o recarregamento de uma sessão.
- web: Corrigir o Enter que não confirmava diálogos modais de confirmação em builds de desenvolvimento.
- web: Corrigir um subagente em segundo plano que aparecia como duas linhas idênticas no painel de agentes durante o streaming.
- Corrigir o log de diagnóstico que não registrava o erro real quando a CLI era encerrada inesperadamente.

## 0.24.2 (2026-07-15)

### Funcionalidades

- Adicionar a skill integrada `/check-kimi-code-docs`, que responde automaticamente a perguntas sobre o produto Kimi Code usando fontes da documentação oficial.

### Melhorias

- Alinhar o comportamento do `kimi -p` entre os diferentes engines: `print_background_mode` e `print_max_turns` agora são aplicados, e execuções de `/goal` permanecem ativas até que o objetivo seja concluído.
- O `kimi -p` agora permanece ativo por padrão enquanto houver tarefas em segundo plano pendentes, sem limite efetivo de espera ou de turnos, e envia cada conclusão de volta ao agente. Defina `print_background_mode = "exit"` ou `"drain"` para restaurar o comportamento antigo de sair após um único turno.
- As tarefas em segundo plano e os subagentes do `kimi -p` não atingem mais o tempo limite por padrão (o modo interativo permanece inalterado); restaure os limites com `[background] bash_task_timeout_s` ou `[subagent] timeout_ms`.
- O tempo limite de subagentes agora é de 2 horas por padrão em todos os modos; substitua esse valor com `[subagent] timeout_ms` ou `KIMI_SUBAGENT_TIMEOUT_MS`.
- O limite de novas tentativas do LLM por etapa foi aumentado de 3 para 10 tentativas, para que falhas temporárias do provedor (429 / sobrecarga) sejam novamente tentadas antes que um turno falhe; ajuste com `loop_control.max_retries_per_step`.
- Os workspaces agora permanecem sincronizados: novas sessões são registradas automaticamente, workspaces ausentes são restaurados na inicialização e os removidos permanecem removidos.
- O `kimi web` agora registra solicitações que falharam e operações importantes, facilitando o diagnóstico de problemas do daemon.
- web: Os cartões do AgentSwarm agora permanecem expandidos enquanto os subagentes ainda estão em execução.
- web: Os cartões minimizados de revisão do plano e de perguntas agora usam um chevron apontando para cima para expandir.

### Correções de bugs

- web: Corrigir o layout em dispositivos móveis no iOS, incluindo o compositor, as áreas seguras e as notificações.
- Corrigir novas sessões que não eram abertas em versões mais antigas da CLI.
- Corrigir notificações de conclusão que eram disparadas antecipadamente quando um subagente terminava enquanto o turno principal ainda estava em execução.
- Corrigir a interface web que exibia a versão incorreta da CLI.
- Corrigir IDs de chamadas de ferramentas do Gemini que colidiam entre turnos e faziam com que execuções do swarm fossem mescladas em um único cartão.
- web: Exibir detalhes do erro do servidor quando ações como interromper ou arquivar uma sessão falham.
- web: Corrigir respostas longas que ficavam travadas após a aba ser colocada em segundo plano.
- web: Corrigir os botões de cópia de blocos de código em conexões HTTP simples.
- web: Manter as sessões carregadas visíveis quando a lista de sessões falha ao ser recarregada.
- web: Restaurar a lista de membros do AgentSwarm após recarregar a página.
- web: Corrigir títulos de sessão que não eram gerados quando a primeira mensagem era um comando slash.
- web: Exibir o horário real de envio de cada mensagem após recarregar uma sessão.
- Corrigir vários problemas do modo de objetivos relacionados a orçamentos e limites de turnos, pausa e retomada, recuperação após falhas, mensagens de status finais e registros de objetivos persistidos inválidos.
- Corrigir objetivos substituídos que podiam afetar o orçamento do novo objetivo e rejeitar objetivos de subagentes de forma consistente.
- Corrigir as orientações exibidas quando um objetivo não pode ser pausado ou retomado.

### Refatorações

- Renomear o recurso de carregamento dinâmico de ferramentas de `select_tools` para `dynamically_loaded_tools`; o comportamento permanece inalterado.

## 0.24.1 (2026-07-14)

### Correções de bugs

- Corrigir sessões do Kimi que ficavam travadas quando o histórico com thinking preservado continha uma etapa de raciocínio vazia.
- Corrigir ferramentas integradas que ficavam indisponíveis quando o provedor do modelo ficava pronto após o início da sessão.
- Corrigir o roteamento do thinking effort: provedores que não são Kimi agora preservam os valores configurados, enquanto modelos Kimi validam as seleções em tempo de execução e usam um fallback seguro durante a resolução do modelo.
- web: Alinhar o tratamento dos níveis de thinking com a CLI: enviar o nível selecionado exatamente como definido, em vez de reduzi-lo silenciosamente; usar o padrão do próprio modelo quando nada for selecionado ou quando o modelo mudar; e persistir as escolhas explícitas como padrão para novas sessões.
- Preservar os resumos de conclusão de objetivos e exibir erros do LLM sem tipo, sem o prefixo de código de erro interno nos eventos de interrupção de etapas.

### Melhorias

- web: Exibir apenas o nome do nível (por exemplo, Max) no indicador do modelo, em vez de "thinking: max".

## 0.24.0 (2026-07-14)

### Funcionalidades

- web: Adicionar a exportação de sessões: execute `/export` ou selecione Export session no menu de opções de uma sessão para baixar a sessão e os logs de diagnóstico como um arquivo ZIP (limitado a 64 MiB).
- Mover comandos Bash executados em primeiro plano que atingirem o tempo limite para segundo plano, em vez de encerrá-los, para que comandos de longa duração continuem sendo executados e informem o resultado quando forem concluídos. Defina `bash_auto_background_on_timeout = false` em `[background]` no config.toml para restaurar o comportamento de encerrar o comando ao atingir o tempo limite.

### Melhorias

- web: Refinar os controles do modo de objetivos com interações animadas na faixa, progresso com base no orçamento e confirmação de cancelamento seguindo o sistema de design.
- Ao fechar uma sessão, as tarefas em segundo plano agora recebem uma solicitação para serem interrompidas e um período de tolerância antes de serem encerradas à força.
- Reescrever os lembretes de chamadas repetidas de ferramentas para direcionar o agente a uma ação diferente, em vez de proibir a chamada.
- Otimizar os prompts da ferramenta TaskOutput para desencorajar esperas bloqueantes por tarefas em segundo plano.
- Enviar o User-Agent `kimi-code-cli` nas solicitações ao registro de provedores (`api.json`) e ao catálogo de modelos, para que os registros possam identificar a versão do cliente.
- Registrar um aviso quando uma skill falhar ao ser analisada, em vez de descartá-la silenciosamente, e corrigir os resultados da verificação de skills que não eram reportados.

### Correções de bugs

- Impedir que leituras de imagens grandes demais prejudiquem as sessões; sessões que já falharam com erros de solicitação grande demais agora são recuperadas automaticamente.
- Corrigir o fork de sessões que perdia tudo, exceto o histórico da conversa: sessões derivadas agora carregam anexos de mídia, arquivos de plano, saída de tarefas em segundo plano e tarefas cron, e um fork com falha não deixa mais uma cópia corrompida para trás.
- web: Corrigir vários problemas de renderização de sessões ao reabri-las, reconectá-las ou ressincronizá-las, incluindo o indicador de uso do contexto que caía para 0, balões de mensagens do usuário duplicados e texto duplicado em turnos com várias etapas.
- web: Corrigir imagens enviadas que não eram exibidas ao conectar-se ao servidor por um endereço diferente de localhost.
- web: Continuar objetivos bloqueados depois que o usuário os retoma pelos controles de objetivos.
- web: Corrigir a lista de membros do AgentSwarm que desaparecia após atualizar a página enquanto os subagentes ainda estavam em execução.
- web: Corrigir o cartão de objetivo que desaparecia após atualizar a página enquanto um objetivo da sessão estava ativo.
- web: Corrigir o menu do seletor de workspace que ficava estreito demais para seu conteúdo.
- web: Recuperar limites de taxa temporários de subagentes sem exibi-los como erros da sessão.
- Corrigir a detecção automática do Bash no Windows que falhava quando o Git vinha de uma toolchain nativa do MSYS2 (ucrt64/clang64/clangarm64).
- Corrigir o login OAuth que ficava travado após a autorização no navegador quando a configuração do provedor mudava durante o login.
- Exibir a mensagem real de rejeição do provedor, em vez de um aviso enganoso para fazer login novamente, quando um modelo gerenciado por OAuth continua retornando 401 após uma atualização do token.
- Corrigir provedores sem `base_url` configurada que eram rejeitados: provedores dos protocolos anthropic/openai e outros agora voltam a usar seus endpoints oficiais padrão.
- Corrigir ferramentas MCP que ficavam indisponíveis no primeiro turno após a inicialização da sessão.
- Corrigir mídias e imagens coladas que eram descartadas dos argumentos de comandos de `/skill` e plugins, e ao enviar instruções com `Ctrl-S`.
- Corrigir blocos de raciocínio vazios que eram descartados entre provedores, o que quebrava chamadas de ferramentas em várias etapas.
- No modo de permissão automático, saídas do plano agora são marcadas como aprovadas automaticamente, em vez de revisadas pelo usuário, para que o agente não confunda a aprovação com um sinal do usuário para começar a executar.
- Corrigir tarefas em segundo plano que eram perdidas ou marcadas incorretamente como perdidas ao retomar sessões.
- Corrigir o encerramento do servidor que, em alguns casos, deixava para trás um arquivo de instância obsoleto.

### Refatorações

- O `kimi web` agora é executado por padrão no mecanismo de agente reformulado.

## 0.23.6 (2026-07-12)

### Polish

- web: Let wide Markdown tables grow beyond the reading column up to 1040px, scrolling horizontally inside the table when wider.
- web: Keep the server access token for up to 7 days across tab close and browser restarts, instead of asking for it again with every new tab.
- web: Add workspaces by typing an absolute path directly in the workspace picker's search box, with live validation and completion suggestions.
- web: Auto-enable the default thinking effort when switching to a model that supports effort levels in the web UI.
- Recognize the `support_efforts` and `default_effort` fields when importing a custom registry, so thinking effort levels are available for those models.
- Update the WebBridge install page link opened from the `/plugins` panel.
- Add a `subagent.timeout_ms` config option (or the `KIMI_SUBAGENT_TIMEOUT_MS` env var) to control how long a single subagent may run before timing out; the default is raised from 30 minutes to 2 hours.
- Add a print-mode background policy: set `[background].print_background_mode = "steer"` to keep `kimi -p` alive across background-task completions, so the main agent can be steered into follow-up turns.

### Bug Fixes

- web: Fix sessions getting stuck in a sending state after a reconnect; turns that finish while the connection is down now stop the spinner and let the next message send normally.
- web: Fix the first visit after starting or updating the web UI bouncing to the login page when the initial auth check fails; the connecting screen now stays up, shows the connection error, and retries.
- Keep `kimi -p` runs alive after a turn ends while a goal is still active or a cron task is pending, so goal continuations and cron fires run their turns instead of being cut off when the main turn finishes.
- Treat a dismissed question prompt as the user choosing not to answer, instead of implicitly selecting the recommended option.
- web: Fix ReadMediaFile results rendering as plain tool cards instead of images after resuming or reloading a session.
- web: Fix the chat view jumping downward while scrolling through conversation history.
- web: Fix the model dropdown showing checkmarks on same-named models from other providers; the current model is now matched by its unique model id.
- web: Fix sidebar lag with many sessions by removing repeated session list scans during rendering.

### Refactors

- Rename the dynamic tool loading model capability from `select_tools` to `dynamically_loaded_tools`.

## 0.23.5 (2026-07-10)

### Polish

- Retry provider 429, overload, and other transient errors more reliably, honoring the server Retry-After delay, and surface retries in `-p --output-format stream-json`.

### Bug Fixes

- Stop unsupported image formats (AVIF, BMP, TIFF, ICO, …) from breaking sessions at every entry point — including remote image URLs and images mislabeled by a tool — and recover an already-stuck session by dropping the offending image and retrying, so one such image can no longer make every later request fail.
- web: Fix the "Turn finished" desktop notification and completion sound firing twice per turn.
- web: Hide the internal image-compression note so it no longer renders as user message text.

## 0.23.4 (2026-07-10)

### Features

- web: Add notifications when a tool needs approval, and improve notification reliability.

### Polish

- web: Polish the chat UI with Inter typography, localized labels, and tighter composer and menu styling.
- web: Polish the session sidebar layout, colors, icons, and typography.
- Display the Extra Usage (fuel pack) balance in the `/usage` and `/status` commands.
- Add a Kimi WebBridge entry to the Official tab of the `/plugins` panel that opens the WebBridge install page in your browser.

### Bug Fixes

- Keep image-heavy sessions within provider request-size limits: oversized images (model-read and pasted, including WebP) are downscaled and compressed, HEIC/HEIF reads are refused with a platform-matched conversion command instead of poisoning the session, and an HTTP 413 request-too-large now recovers automatically — the request and `/compact` retry with older media replaced by text markers. The limits are configurable via `[image]` in `config.toml` (or `KIMI_IMAGE_*` env vars), and each core keeps its own settings so reloading one client's config no longer changes another client's compression.
- Fix resuming sessions whose original working directory no longer exists.
- Fix prompt-mode goals so they run until completion and report invalid goal commands before sending prompts.
- web: Fix an occasional "another turn is active" error when sending the first message of a new conversation, and show a starting state while it is being sent.

## 0.23.3 (2026-07-08)

### Bug Fixes

- Fix a misleading "OAuth login expired" message shown when a model is not available for the current account.

## 0.23.2 (2026-07-08)

### Features

- Add the Vercel plugin to the bundled plugin marketplace. Run `/plugins` and select Vercel Plugin to install it.

### Bug Fixes

- Fix `kimi -p` runs exiting with code 0 when a turn fails.
- Prevent autonomous goals from being paused by model-reported status updates.
- Count the turn that starts an autonomous goal toward its turn budget.
- Raise the image downscale cap from 2000px to 3000px, and fix swapped width/height for EXIF-rotated (portrait) photos in compression captions and media read notes so region readback coordinates map correctly.
- web: Fix the connection error toast lingering after the WebSocket reconnects when returning from the background.
- Fix console windows flashing on Windows each time a hook runs.

### Polish

- web: Redesign the scheduled reminder UI.
- web: Show session skills in the slash menu as `/skill:<name>` so they are distinguishable from built-in commands; typing the bare skill name still works.
- web: The composer model switcher switches the active session's model as before and additionally bumps the global default model, so new sessions inherit the choice.
- web: Press Enter to confirm in archive and other confirmation dialogs.
- Tighten goal-mode guidance for blocked and complete status updates.
- Progressive tool disclosure (`select_tools`, experimental): compaction now discards the loaded tool schemas instead of re-injecting them, and the model re-selects the tools it still needs afterward. A from-memory call to a no-longer-loaded tool is rejected with guidance to select it first. No effect unless the `tool-select` experimental flag and a `select_tools`-capable model are active.

### Refactors

- web: Compile icons at build time so the bundled web UI only carries the icons it renders.

## 0.23.1 (2026-07-07)

### Bug Fixes

- Fix `kimi -p` abandoning background subagents that start late or run long, so their results reach the main agent.
- web: Recover chat streaming after a stale background-tab WebSocket instead of requiring a page refresh.
- Fix some third-party models (e.g. Opus 4.8) falling back to the family default max output tokens; an unrecognized minor now reuses the nearest earlier known version's limit.
- Honor explicit Anthropic `max_output_size` settings instead of clamping them to built-in ceilings.
- Stop showing tool-produced `<system>` metadata in tool outputs; failed tools now show their own error text.
- Fix goal completion and blocked updates to produce one final user-facing outcome summary from the tool result.
- Fix goal startup and queue handling so failed starts restore permission mode and queued goals wait behind new user messages.
- Fix goal token budgets to count model completion tokens and stop without extra continuation steps when the budget is exhausted.
- Fix goal tools being unavailable to the main agent, and return clear messages for invalid goal-control calls.
- Respect the `--skills-dir` flag in interactive mode.
- web: Fix several slash commands and skills not working on the new-session screen: `/goal <objective>` and slash skill activations (for example `/pre-changelog`) silently did nothing, and `/btw [<question>]` opened an empty side chat.

### Polish

- Preserve prior turns' thinking by default on the Anthropic provider (Claude and Kimi's Anthropic-compatible mode), matching the Kimi default. Disable with `[thinking] keep = "off"` or `KIMI_MODEL_THINKING_KEEP=off`.
- Clarify the permission mode descriptions shown by `/permission`, `/auto`, and `/yolo`, and reorder `/auto` and `/yolo` in the command list.
- Show long-running goal wall-clock budget reminders in hours.
- Tighten goal-mode guidance so agents continue reasonable work across turns instead of ending goals prematurely.

### Refactors

- Record a per-request trace in the session wire log, so model requests can be reconstructed for debugging.

## 0.23.0 (2026-07-06)

### Features

- web: Add an Archived sessions page in Settings to browse and restore archived sessions. Open Settings → Archived to find it.
- Add experimental on-demand tool loading (`select_tools`) under the `tool-select` flag: a supporting model loads MCP tools only when needed instead of sending all of them in every request, preserving the provider prompt cache. Off by default and only active on models that declare the `select_tools` capability.

### Bug Fixes

- Fix sessions that exist on disk but were missing from the session list or returned 404 on direct access, by rebuilding the session index at server startup.
- Fix the Bash and Edit tool cards collapsing, jumping, or flickering in height when results stream in or finish with short output, and visually separate the Bash command from its output.
- Fix the input box shifting upward after the slash command menu closes.
- Fix the edit approval preview shown by Ctrl+E to include surrounding context lines, matching the summary panel.
- Fix `@` file completion missing deeply nested files in large projects after adding extra workspace directories.
- web: Fix several web layout and animation glitches: the collapsed sidebar now hides correctly, the chat history no longer replays its entrance animation when opening a session, and tool components no longer jump the conversation when expanded or collapsed.
- web: Fix scheduled-reminder (cron) fires being hidden; they now show as notice cards in the chat.
- web: Fix the end of a reply staying missing after reopening a session.
- web: Fix queued media messages not loading back into the composer and keep attachments when undoing a message.
- web: Keep the composer toolbar from clipping its controls on narrow windows and phones, with the context ring staying visible at every width.
- web: Fix the font size setting so chat text, composer text, and sidebar text follow the selected size.
- web: Fix an almost-invisible composer input caret and a washed-out strikethrough on completed todos.
- web: Show the correct session search shortcut on Windows.
- Fix tool calling with Google Gemini models, including Gemini 3 thinking-signature round-trips across turns.

### Polish

- web: Replace the swarm footer with a single inline tool card that shows live subagent progress and the aggregated result, and keep the swarm progress bar stable after refresh.
- Show compaction summaries in the TUI after compaction. Press Ctrl+O to show or hide the summary.
- web: Render AskUserQuestion answers as a readable option list with the chosen option(s) highlighted, instead of raw JSON.
- web: Show available skills in the composer before a session is created.
- web: Add an Archived sessions entry to the mobile settings sheet and clarify the archive confirmation to mention restoring from Settings.
- web: Show the Kimi icon and clearer titles in desktop notifications.
- web: Align the markdown diff code block with the design system: code text keeps the normal ink colour while the sign and a soft row background carry the change, matching the `~/diff` panel.
- web: Prevent chat text from hyphenating at line breaks and render code without font ligatures.
- web: Drop the stray left indent in the tool-call card body so expanded content aligns with the header.
- Feed AskUserQuestion answers back to the model as question text and option labels instead of positional ids, so the model no longer has to map them back. Question texts must now be unique per call and option labels unique per question; existing clients keep answering with option ids, so no client change is required.
- Keep prior reasoning across turns for Kimi models by default when Thinking is on. Set `[thinking] keep = "off"` to disable.

## 0.22.3 (2026-07-04)

### Bug Fixes

- Wait for background subagents to finish and respond to their results before exiting in `kimi -p`, instead of ending the turn early.
- web: Fix uploaded videos failing to play in the web chat.
- Revert the recent TUI transcript rendering changes to the original upstream behavior and fix related rendering issues.

### Polish

- Add `--dangerous-bypass-auth` and `--keep-alive` flags to `kimi server run`, so the server can run without a token on trusted networks and stay alive past the idle timeout.
- web: Add click-to-enlarge for images uploaded in the web chat. Click an image in a message to open it.

## 0.22.2 (2026-07-03)

### Bug Fixes

- Fix sessions silently dropping later user messages after a turn was interrupted between a tool call and its result.
- Fix requests being rejected by strict providers when the model emits duplicate tool call ids.
- Fix `kimi upgrade` failing on Windows with a spawn error when installing the new version.
- Fix duplicated transcript content appearing in scrollback during streaming.
- Fix compressed-image prompts leaking an internal `<system>` compression note into the visible message and the session title.
- Keep automatic background updates from flashing a console window on Windows.

### Polish

- Have context-compaction notes capture a forward plan for the remaining work — upcoming steps, settled decisions, and foreseeable obstacles — instead of only the immediate next step, so the agent continues more coherently after auto-compaction.
- Enrich PATH from the user's login shell at startup, so shell commands find user-installed tools (e.g. Homebrew's `gh`) even when kimi-code was launched without the full profile PATH.
- Promote the language-matching rule to a dedicated section in the system prompt, so replies and reasoning consistently follow the user's language through long English tool output, while repository artifacts keep project conventions.
- Add a TUI preference to keep rapid multi-line pastes from submitting line by line when bracketed paste is unavailable. Set `disable_paste_burst = true` in `tui.toml` to turn it off.
- Keep subagent cards at a stable height and show a live status spinner with a compact two-row activity window.
- In `kimi -p` runs, wait for background subagents to finish before exiting when `background.keep_alive_on_exit` is enabled. Set `keep_alive_on_exit = true` to let concurrent background subagents complete.

### Refactors

- Record model response ids in session wire logs to make individual model requests easier to trace.

## 0.22.1 (2026-07-02)

### Bug Fixes

- Fix TUI rendering bugs that caused the screen to go blank and the input box to disappear.
- Fix the TUI crashing when the terminal is resized to a very narrow width while the input contains CJK or emoji text.
- Fix the web UI becoming sluggish after opening many sessions.
- Clear the screen fully when starting a new session via /new, /clear, or a session switch.
- Fix web tooltips that could get stuck on screen when their trigger element is removed while open.
- Fix the sidebar session row shifting its title and status badges when hovered.
- Fix the session search dialog showing a horizontal scrollbar for long session titles or snippets.

### Polish

- Improve compaction handoff summaries for more reliable resumed sessions. They now keep the latest intent, key tool results, decisions, open questions, and context to re-check.
- Save shell commands to input history and recall them in bash mode. Press Up on an empty `!` prompt to browse previous shell commands.
- When large images are compressed, tell the model the original and delivered image details. Keep the original image available, and support cropped or full-resolution reads for fine details.
- Refresh the web UI icon set and unify the message copy and undo button hover states and tooltips.
- Let the web sidebar collapse an expanded workspace session list back to its first page.
- Trim redundant and incorrect tooltips in the web UI.
- Show an up arrow on the web composer send button.

### Refactors

- Remove the experimental micro compaction feature and its toggle from the experiments panel.
- Remove duplicate newline-shortcut handling from the prompt editor.

## 0.22.0 (2026-07-02)

### Features

- Automatically compress oversized images before they reach the model, downsampling and re-encoding them to cut vision-token cost and avoid provider image-size errors.
- Add model alias overrides, letting you set model metadata under `[models."<alias>".overrides]` to override provider catalog refresh results.

### Bug Fixes

- Fix plan, swarm, and goal modes being shared across sessions in the web UI; each session now keeps its own toggles.
- Fix the transcript jumping to the top when scrolling up through history during streaming output.
- Release pasted images and streaming timers once they are no longer shown, so memory stops growing in long sessions.
- Fix the terminal being left in raw mode with a hidden cursor and disabled flow control after a crash or abrupt exit.
- Fix an active workspace showing only its five most recent sessions on load, so it now keeps loading older sessions from the last 12 hours.
- Fix the Thinking-by-default setting not taking effect, so new sessions correctly start with thinking enabled.
- Fix spurious errors from the web question, approval, and task actions when the action was already complete, and add loading feedback so each click is acknowledged immediately.
- Show draft pull requests with a distinct draft status instead of displaying them as open.
- Hide the conversation outline when there is not enough room to expand its labels, so it no longer clips against the window edge.
- Hide the unsupported Off option in the /model thinking switcher for always-on models that already expose multiple effort levels.

### Polish

- Refresh the web UI with a new design system, including updated colors, typography, spacing, light and dark palettes, restyled tooltips, and subtle enter/exit and expand/collapse animations.
- Group consecutive tool calls into a collapsible stack with per-tool renderers, including diff line-count chips for edits and inline previews for image, video, and audio results.
- Improve session search with a Cmd/Ctrl+K palette that filters by title, workspace, and last prompt with highlighted matches. Press Cmd+K or Ctrl+K to open it.
- Show queued prompts inline below the running turn in the web chat, and split Stop into its own button so Send no longer interrupts.
- Show the conversation outline as one entry per user query that expands into a labeled list on hover.
- Replace the Explore and Native theme options with a single chat layout and a Blue or Black accent-color setting.
- Add workspace sorting by manual order or last-edited time, plus collapse-all and expand-all controls, to the sidebar.
- Show time, duration, connection, and stack details in web error and warning toasts.
- Use one consistent modal dialog for confirmations in the web UI (archive session, delete workspace, delete provider, undo message, and mode toggles).
- Reduce the default TUI transcript window to keep long sessions responsive.
- Reduce the web composer's default height for a more compact empty state, and fix ArrowUp recalling the previous message while editing a multi-line draft; ArrowUp now recalls only from the very start of the text and is disabled in the expanded editor.
- Remove the fade-out animation when undoing a message in the web chat.

## 0.21.1 (2026-07-01)

### Bug Fixes

- Keep the waiting spinner visible while encrypted reasoning streams, fixing a blank spinner-less gap before the first response text appears.

## 0.21.0 (2026-07-01)

### Features

- Plugins can now provide slash commands via a `commands` field in their manifest, registered as `<plugin>:<command>` and invoked with `$ARGUMENTS` expansion.
- Add Mermaid diagram rendering to the web chat. Fenced `mermaid` blocks in assistant responses now render as diagrams. KaTeX math and Mermaid diagram parsing also run in Web Workers to keep the UI responsive during live streaming.

### Bug Fixes

- Stop a malformed message history from permanently bricking a session on strict providers (Anthropic). The request is repaired before sending — orphaned tool calls are closed and empty/whitespace-only text blocks dropped — and if the provider still rejects its structure, it is resent once with a wire-compliant rebuild.
- Force-exit headless runs (`kimi -p`) so a stray ref'd handle left over from the run can't keep a completed run alive until an external timeout, and bound prompt cleanup so a wedged shutdown step can't hang shutdown.
- Fix @ file mentions not opening when typed inside a slash command argument.
- Fix adding a workspace by path in the web UI failing silently when the daemon rejects the path; it now shows an error instead of a broken workspace.
- Fix duplicate workspaces showing in the web sidebar when the same folder is registered more than once.
- Fix the web workspace rename not persisting after a page refresh.

### Polish

- Add a double-Esc shortcut to open the undo selector. Press Esc twice while idle to undo.
- Show file path completions when typing `/` in shell mode (`!`).
- Always show the usage-data opt-out toggle in the web settings with a clearer label and description.

### Refactors

- Rework conversation compaction:
  - Keep only recent user prompts plus a single user-role summary; drop assistant and tool messages.
  - Repair tool_use/tool_result adjacency before sending, fixing a strict-provider HTTP 400 when a tool call and its result became non-adjacent.
  - Merge consecutive user turns for strict providers (Gemini/Vertex), fixing an HTTP 400 ("roles must alternate") after compaction or when a turn is steered in right after a tool result.
  - Micro-compaction now defaults off.
- Refactor the thinking effort system
- Add a server-side key-value store API for persisting web UI preferences to the user's data directory.

## 0.20.3 (2026-06-30)

### Bug Fixes

- Fix provider error messages rendering as blank lines in the TUI when the server returns an HTML error page.
- Fix the web composer being hidden behind the mobile Safari toolbar and the page auto-zooming when the composer is focused.

### Polish

- Refresh provider model lists automatically in the background instead of only at startup, so newly available models appear without restarting.
- Glob now uses ripgrep, so it respects .gitignore by default, supports brace patterns, returns only files, and keeps partial results with a warning when some directories are unreadable.

### Refactors

- Align malformed tool call argument handling with schema validation fallback.

## 0.20.2 (2026-06-29)

### Features

- Support the Anthropic-compatible protocol for Kimi Code, including video input.
- Add a completion sound and question notifications to the web UI, with separate Settings toggles for completion notifications, question notifications, and sound. Question notifications default off so question text only reaches your desktop after you opt in.
- Add `KIMI_CODE_CUSTOM_HEADERS` for custom outbound LLM request headers, and send the `User-Agent` header to non-Kimi providers. Set `KIMI_CODE_CUSTOM_HEADERS` to newline-separated `Name: Value` lines.
- Add an optional `exclude_empty` parameter to the session list API to omit sessions that have no messages.

### Bug Fixes

- Recover from provider 413 context overflows by compacting before retrying.
- Cap compaction output at 128k tokens by default to avoid provider `max_tokens` errors.
- Fix compaction ignoring the configured max output size.
- Fix unnecessary full-screen redraws when typing in the input box or toggling the slash panel.
- Keep unsent composer attachments scoped to their session in the web UI, so switching sessions no longer leaks them into another session's next message.
- Fix the web composer occasionally keeping typed text after sending the first message of a new session.
- Fix debug timing output lingering after undoing a turn.
- Fix working tips getting squeezed against the agent swarm progress bar.

### Polish

- Rework the web ask-user-question card into a step-by-step wizard so multi-question navigation and the final Submit action are easier to see.
- In the bundled web UI, a new session is now created only when the first message is sent, so `+ New` without a workspace opens the composer instead of making an empty session.
- Restore each session's scroll position when switching back to it in the web UI.
- Keep the open side panel when switching between sessions in the web UI.
- Scope the web composer's up/down input history to the current session instead of sharing it across all sessions.
- In the bundled web UI, `/new` and `/clear` are now aliases that open the session onboarding composer and focus the input. iOS auto-zoom is prevented by keeping text inputs at 16px instead of disabling viewport scaling.
- Hide unused "New Session" entries from the web session list by default.
- Remove the `/sessions` slash command from the web UI; the sidebar already covers session browsing.
- Show the first five sessions per workspace in the web sidebar instead of ten.
- Replace the web composer attach button's plus icon with an image icon.

### Refactors

- Route Kimi Code models on the Anthropic-compatible protocol through the beta Messages API.
- Upgrade web markdown renderer dependencies (katex, markstream-vue, shiki) for bug fixes and performance improvements.
- Add provider type and protocol attributes to turn and API error telemetry.

## 0.20.1 (2026-06-26)

### Features

- Plugins now support declaring lifecycle hooks in `kimi.plugin.json` to run scripts at specific stages. See [Hooks in Plugins](../customization/plugins.md#hooks-in-plugins).
- `/feedback` now supports attaching diagnostic logs and codebase context.
- Add the `kimi update` command, equivalent to `kimi upgrade`, for upgrading to the latest version.
- `kimi web` adds the `--allowed-host <host>` option to add a specified Host to the DNS-rebinding allowlist; 403 errors now explain how to allow it via `--allowed-host` or `KIMI_CODE_ALLOWED_HOSTS`, e.g. `kimi web --allowed-host example.com`.

### Bug Fixes

- Fix kimi server failing to start on Windows after the first run.
- Fix the Web UI opened by the `/web` command not signing in automatically; the terminal now prints the access token.
- Cap chat-completions providers' `max_tokens` to the remaining context window, avoiding context overflow and invalid parameter errors.

### Polish

- Optimize the default system prompt and built-in tool descriptions to stop the agent from blocking background tasks, unify tool guidance across profiles, and surface previously missing tool-result details (fetched-page mode, Grep match totals).
- Cache rendered message lines to keep the terminal responsive in long conversations.
- Retain only recent turns in the transcript and collapse older steps within each turn to keep long sessions responsive.
- Make the web chat input grow with its content and add an expandable editor for longer messages.
- Show the done / in progress / pending breakdown of hidden todos in the collapsed todo panel.

## 0.20.0 (2026-06-26)

### Features

- Add shell mode to the TUI. Type `!` in the input box to enable it. For long-running commands, press Ctrl+B to move them to the background. For example, you can run `!gh auth login` to sign in to the GitHub CLI without opening a new terminal.
- Add a `--host` CLI option so `kimi web --host` can expose the server to the internet, with hardened token authentication, rate limiting, and other security measures.
- Render LaTeX display math (`$$…$$`) in the web UI.

### Bug Fixes

- Fix a startup crash on Linux caused by an unhandled native clipboard error.
- Fix `kimi web` and `/web` failing to start the background server daemon on Windows with `spawn EFTYPE` when the CLI is installed via npm/pnpm or run from source. The official single-binary install script was not affected.
- Fix the terminal window repeatedly losing focus on Linux Wayland, which broke IME input.
- Stop auto-dismissing questions in the web UI after 60 seconds so they wait for the user's answer.
- Fix explore subagents silently losing git context when git commands time out or the directory is not a repository.
- Fix Ctrl-C during compaction so it clears a pending editor draft first instead of cancelling immediately.
- Fix MCP server working directories when sessions are hosted by the web server.
- Fix duplicate session snapshot reloads in the bundled web UI during resync.
- Fix truncated skill descriptions missing an ellipsis in the model's skill listing.

### Polish

- Redesign `/plugins` as a single tabbed panel: **Installed** (manage installed plugins — toggle, remove, MCP, details, reload), **Official** (Kimi-maintained marketplace plugins), **Third-party** (marketplace plugins from other publishers), and **Custom** (install straight from a GitHub URL, zip URL, or local path). Use `Tab` / `Shift-Tab` to switch tabs.
- Show a line-by-line diff when the agent edits or writes a file in the web chat.
- Show the plan body and approach choices in the plan review card when exiting plan mode in the web UI.
- Show the full accumulated progress of a subagent in its detail panel, with concise tool-call summaries instead of raw JSON.
- `/reload` now refreshes the assistant's view of plugin skills, so plugin changes take effect in the current session instead of requiring a new one.
- Replace silent AGENTS.md truncation with a visible warning in the TUI status bar and web UI.
- Add a confirmation prompt before installing third-party plugins.
- Show update badges on the `/plugins` Installed tab, where Enter now installs the available update and I opens plugin details.
- Add a copy button to user messages in the web chat.
- Preserve full tool output logs when previews are truncated and link background task completion notifications to saved output.
- Sync session title changes across all connected clients in server mode.
- Add Ctrl+U and Ctrl+D as page up and page down shortcuts in the task output viewer.
- Add a hint to the per-turn step limit error pointing users to the `loop_control.max_steps_per_turn` config option.
- Reduce streaming redraw cost for long assistant messages with code blocks.
- Page the web session list per workspace so the first screen no longer fetches every session up front.
- Keep the web session sidebar from re-rendering on every streaming token to improve rendering performance.
- Create missing parent directories automatically when writing a file.
- Improve the image paste hint.

## 0.19.2 (2026-06-24)

### Features

- Keep drag-and-drop workspace reordering in the web sidebar, with sort order persisted locally; sessions now also float to the top of their group as soon as a new message arrives.
- Add an Alt+S shortcut in the model picker to switch the model for the current session only, without saving it as the default.
- Add a Ctrl+T shortcut to expand and collapse a truncated todo list.
- Add `-c` as a shorthand for `--continue`.

### Bug Fixes

- Fix yolo mode in the web app auto-approving plan reviews and sensitive file access.
- Fix resume not realigning a tool call that was interrupted mid-history.
- Fix the composer's ↑/↓ input-history recall doing nothing right after the first message of a new session.
- Fix stale rows occasionally leaving duplicate input boxes after tall content shrinks.
- Fix inline images being rendered as broken escape sequences in the transcript.
- Fix code blocks nested inside list items rendering blank in the web chat after a turn finishes generating.
- Fix the Tab key unexpectedly opening the file completion list.
- Fix clipboard copy actions in the web UI when served over plain HTTP.
- Fix the web question prompt missing the free-text Other option.
- Fix web chat stop actions so stale prompt ids fall back to cancelling the active session.

### Polish

- Read large text files in bounded memory and read tail lines without scanning whole files.
- Show the command in running Bash tool cards and allow expanding it with Ctrl+O before the result arrives.
- Allow the web sidebar and detail panel to be resized up to the available viewport width, keeping their resize handles reachable on narrow windows.
- Show subcommand suggestions after Tab-completing a slash command name.
- Show a transient footer hint when an image is detected in the clipboard, displaying the platform-appropriate paste shortcut.
- Persist the collapsed state of workspace groups in the web sidebar across page reloads.
- Add a development-mode indicator to the web sidebar for local development.
- Optimize the loading tips display.

### Refactors

- Reorganize the web app's components into area subdirectories (chat/settings/dialogs/mobile) and refresh the component path comments.
- Extract several composer pieces into reusable composables.
- Extract pure turn-rendering helpers out of the chat pane into their own module.
- Extract the beta conversation outline (table of contents) into its own component.
- Extract the workspace group rendering out of the sidebar into its own component.

## 0.19.1 (2026-06-23)

### Bug Fixes

- Fix ACP editors such as Zed failing to start a new thread.
- Fix the web sidebar's unread dots getting out of sync across browser tabs.
- Clear all per-session state when a session is archived or removed, so archived sessions no longer leave orphaned data behind.

### Refactors

- Consolidate web client localStorage access and split the root state store and app shell into focused composables.

## 0.19.0 (2026-06-22)

### Features

- Added the ability to add extra workspace directories:
  - Use the `/add-dir <path>` command to add extra working directories to the current session, or remember them for the project.
  - Use `kimi --add-dir <path>` to add them on startup.
  - Project-level local config is now managed in `.kimi-code/local.toml`; we recommend adding it to your `.gitignore`.
- Allow long-running foreground commands and subagents to be moved into background tasks with `Ctrl+B`, and inspect them via the `/tasks` panel.

### Bug Fixes

- Surface provider safety-policy blocks instead of silently treating them as completed turns, and prevent the context token count from dropping to zero after a filtered response.
- Fix provider requests failing when restored conversation history contains empty text content blocks.
- Detect the real image format from file contents when reading media, so a mismatched filename extension no longer produces a data URL the model API rejects.
- Fix commands flashing an empty console window on Windows.
- Stop showing unread dots on cancelled or failed sessions in the web sidebar.

### Polish

- Speed up session snapshot loading with a direct disk reader and a request timeout safeguard, keeping the previous path as a legacy fallback.
- Show longer branch names in the web chat header and expose the full name on hover.
- Keep the web page title fixed instead of changing with the session or workspace name.
- Polish file mention UX.

### Refactors

- Unify image format detection when sniffing fails.
- Consolidate web client localStorage access and decouple appearance/notification state into dedicated modules.

## 0.18.0 (2026-06-18)

### Features

- Add session filtering to the web sidebar, filtering by title and the last user prompt.
- Add scroll-up lazy loading for older messages in the web chat session view.
- Add an environment variable to cap AgentSwarm concurrency during the initial ramp, so large swarms do not trip provider rate limits as easily.

### Bug Fixes

- Fix the web app only loading the 20 most recent sessions.
- Fix web slash skill selection sending immediately and allow slash search to match skill names by substring.
- Fix the highlighted web slash command not staying visible while navigating a long slash menu.
- Fix incorrect display after archiving the last session.
- Fix the web login slash command description to match the browser authorization flow.

### Polish

- Redesign the web OAuth login dialog so the order of steps is unambiguous.
- Show the current version in web settings.
- Allow long web slash command names and descriptions to wrap without overflowing the slash menu.
- Add `/reload` suggestion in plugin-change hints.

## 0.17.1 (2026-06-17)

### Bug Fixes

- Fix the `kimi web` command failing to start in the background.
- Stop the background local server from locking the directory it was started in.
- Prevent the web login dialog from closing when clicking the backdrop.

### Polish

- Group the default model dropdown in web settings by provider.

## 0.17.0 (2026-06-17)

### Features

- Add Kimi Code Web mode, which you can start with `kimi web` or `/web` in the CLI, and continue sessions in a browser chat interface.

### Bug Fixes

- Show the underlying connection error when OAuth token refresh fails after internal retries, instead of prompting for login. Token refresh failures are no longer re-retried at the agent loop level.
- Restore the turn counter from persisted loop events on resume so post-resume turns no longer reuse turn ids that already appear in history.

### Polish

- Skip debug TPS when the output stream is too short to measure reliably.

## 0.16.0 (2026-06-16)

### Features

- Add a built-in `kimi vis` command that launches the session visualizer in your browser, pointed at your local sessions. Supports `--port`/`--host`, `--no-open`, and `kimi vis <sessionId>` deep-links.

### Bug Fixes

- Stop Anthropic-compatible providers from reading ambient Anthropic shell credentials and custom headers.
- Fix repeated compaction handling when context remains over the blocking threshold.
- Prevent session shutdown from resuming the agent when stopping background tasks.
- Project session replay ranges over rendered replay records instead of raw persisted records.
- Close wrapped output streams when buffered readers are destroyed.

### Polish

- Reduce the maximum height of the `/btw` side panel from half to one-third of the terminal.
- Polish queue pane styling.
- Add configurable banner display frequencies with local display state.

### Refactors

- Remove redundant LLM request logging context plumbing.

## 0.15.0 (2026-06-15)

### Features

- Add an all-sessions picker view with name search, paginated browsing, and clipboard-ready resume commands for sessions in other working directories.
- Add support for legacy SSE MCP servers alongside stdio and streamable HTTP transports.

### Bug Fixes

- Recover resumed sessions when an interrupted tool call result was not recorded.
- Stop writing resume version markers into persisted agent metadata.
- Do not carry obsolete legacy loop, background, plan, yolo, or unknown experimental flags into migrated config files.
- Repair mismatched JSON Schema types emitted by Xcode 26.5 MCP server for Moonshot compatibility.

### Polish

- Keep TUI components within narrow terminal widths by wrapping, compacting, or truncating lines that could exceed the render width.
- Prompt the CLI to show one brief same-language status sentence before non-trivial tool calls.
- Extend the same-language rule to the model's reasoning, so thinking follows the user's language while keeping code and technical terms in their original form.
- Read media files using header-detected types before falling back to media extensions.
- Prioritize clearing draft editor text before Ctrl-C cancels an active stream.
- Collapse hidden directories in the workspace prompt and explain how to inspect them.
- Include the skill's directory on the loaded-skill context block so the agent can locate a skill's bundled resources (scripts, templates) after it is invoked.
- Show the all-sessions toggle hint when the current working directory has no sessions.
- Clarify that compaction summaries must be emitted in the final answer.
- Clarify AGENTS.md prompt guidance and mark truncated instruction files.

### Refactors

- Resolve model capabilities through a static lookup instead of instantiating a temporary provider.
- Decouple agent skill access from session-specific registry implementations.
- Optimize the npm packaging system.

## 0.14.3 (2026-06-14)

### Polish

- Refresh provider model metadata before opening the model picker.

## 0.14.2 (2026-06-12)

### Bug Fixes

- Fix endless desktop notifications in iTerm2 by only sending terminal progress sequences to terminals that support them.
- Show completed and cancelled compaction records correctly when resuming a session.
- Drop invalid config.toml sections with a warning instead of failing to start.

### Polish

- Stream foreground Bash stdout and stderr while commands are still running.
- Allow `--auto`, `--yolo`, and `--plan` to be combined with `--session` or `--continue` by applying the requested mode to the resumed session.
- Qualify sub-skill names with their parent prefix and expose sub-skills as dotted slash commands in the TUI.
- Sync custom registry provider additions, removals, and rotated registry keys during startup refresh.

## 0.14.1 (2026-06-12)

### Bug Fixes

- Cancel active turns during session shutdown so foreground shell commands do not outlive prompt-mode exits.
- Stop background tasks by default when sessions close.
- Prevent overlapping interactive agent requests from using the wrong active agent.
- Fix premature stream close errors when shell processes time out or are killed.
- Degrade unsupported audio/video to placeholder text and reattach tool result media instead of silently dropping them.
- Send OpenAI Responses system prompts as request instructions.
- Propagate configured execution environment overrides across spawned processes.
- Fix ACP file reads and edits for Windows workspaces opened through IDE clients.
- Require AgentSwarm tool calls to run alone in a model response.

### Polish

- Add runtime support for dynamic MCP server updates, reference skills, replay timestamps, and Node file uploads.
- Add a YOLO choice when starting swarm tasks from Manual mode.
- Polish builtin skills.
- Find slash commands by their aliases in autocomplete — typing `/clear` now suggests `new (clear)`.
- Wrap long command and skill descriptions in the autocomplete menu onto a second line instead of cutting them off.
- Display a tips banner below the welcome panel on startup.

## 0.14.0 (2026-06-10)

### Features

- Add an `Interrupt` hook event that fires when the user interrupts a turn (e.g. pressing Esc), letting hooks observe the turn stopping instead of getting stuck on a working state.

### Bug Fixes

- Preserve image outputs from tools when using OpenAI-compatible chat completions.

## 0.13.1 (2026-06-10)

### Bug Fixes

- Prevent forking sessions during active turns and consolidate wire protocol definitions into a shared internal package.
- Fix Kimi Datasource to use the matching OAuth credentials and service endpoint for the active Kimi Code environment.
- Fix goal marker text overflowing terminal width.

### Polish

- Add Claude Fable 5 support to the Anthropic provider.
- Add an interactive undo selector and clearer undo-limit messages.
- YOLO mode no longer asks before writing or editing files outside the working directory.
- Clarify active skill prompts so loaded skills are no longer represented as system reminders.
- Tighten file tool guidance to route incremental edits through Edit.

## 0.13.0 (2026-06-10)

### Features

- Add custom color themes. Define your own palette as a JSON file in `~/.kimi-code/themes/`, or generate one with the built-in `/custom-theme` skill command.
- Add `/import-from-cc-codex` to import selected Claude Code and Codex instructions, Skills, and MCP settings.
- Show available plugin updates in the marketplace.

### Bug Fixes

- Fix Windows builds and development launches that could fail when package binaries resolve to command shims.
- Fix device login to keep the URL and code visible when the browser cannot be opened.

### Polish

- Clarify grouped subagent progress with active status breakdowns and elapsed time.
- Truncate queued message display to a single line with ellipsis when it exceeds terminal width.

## 0.12.1 (2026-06-09)

### Bug Fixes

- Allow obsolete experimental config entries to remain without blocking startup.
- Pass through xhigh reasoning effort for OpenAI-compatible chat completions requests.

## 0.12.0 (2026-06-09)

### Features

- Add the `/swarm` command for running agent swarms with live progress and rate-limit-aware retries.
- Make goals, background questions, and sub-skill discovery available without experimental opt-ins.
- Honor the standard `HTTP_PROXY` / `HTTPS_PROXY` / `ALL_PROXY` / `NO_PROXY` environment variables, including SOCKS proxies, for all outbound traffic.
- Support Homebrew installations.
- Enable micro compaction by default. Disable via `/experiments`.

### Bug Fixes

- Fix ACP slash skill routing, bootstrap context reads, file and permission edge cases, subagent event handling, and stale-file edit messaging.
- Fix goal resume behavior by restoring goal state from agent records.
- Fix thinking text and tool output display for subagents.
- Fix session workdir mismatch on Windows caused by inconsistent path separators.
- Fix the `/mcp` status panel border being broken by multi-line MCP server errors, which are now folded onto a single row.
- Detect Git Bash installed through Scoop and other Git shims on Windows.
- Show the underlying error when migration fails.
- Allow the startup session picker to exit with repeated Ctrl-C or Ctrl-D.

### Polish

- Remove the per-turn auto-compaction limit so long conversations can keep compacting instead of failing early.
- Improve goal mode outcome handling with follow-up messages, safer error pauses, and clearer TUI transcript display.
- Show full plan cards directly and remove the Plan card keyboard shortcut.
- Wrap long single-line shell commands in approval prompts so the full command remains visible.
- Rework file reference completion in the TUI.
- Load Kimi-specific user Skills and global agent instructions from `KIMI_CODE_HOME` when it is set.

## 0.11.0 (2026-06-05)

### Features

- Add experimental sub-skill discovery gated by the `KIMI_CODE_EXPERIMENTAL_SUB_SKILL` environment variable. Ships the `sub-skill` builtin bundle (`sub-skill.review`, `sub-skill.consolidate`) for inventorying and consolidating skills into hierarchical groups.
- Add the following environment variables:

  - `KIMI_MODEL_TEMPERATURE`, `KIMI_MODEL_TOP_P` — sampling parameters applied globally to any `kimi` provider (not tied to `KIMI_MODEL_NAME`).
  - `KIMI_MODEL_THINKING_KEEP` — Moonshot preserved-thinking passthrough (`thinking.keep`), injected only while Thinking is on.
  - `KIMI_CODE_NO_AUTO_UPDATE` (legacy alias `KIMI_CLI_NO_AUTO_UPDATE`) — fully disables the update preflight (no check, background install, or prompt).
- Show built-in skills as direct slash commands and group them ahead of external skill commands.

### Bug Fixes

- Fix slash command autocomplete so goal text can be submitted when the cursor is before existing text.
- Fix queued goals so failed promotion attempts do not lose or duplicate queued work.
- Fix upcoming-goal queue handling while editing or pasting queued goals.
- Ask before starting goals in YOLO mode so users can switch to Auto for unattended work.
- Show concise provider filtering errors when responses are blocked before visible output.
- Show "unknown command" instead of "too many arguments" when an invalid subcommand is entered.
- Clamp OpenAI Chat Completions `xhigh` and `max` thinking effort to `high` unless the model supports `xhigh` on `v1/chat/completions`.
- Preserve thinking effort when compacting long conversations.
- Refresh provider model metadata when capabilities change without model ID changes.

### Polish

- Show the upcoming-goal confirmation with the same accent treatment as goal lifecycle messages.
- Start upcoming goals immediately when there is no active goal to wait for.
  Support multiline edits when managing upcoming goals.
- Use a fixed 30-minute timeout for subagents and show concise resume instructions when they time out.
- Highlight goal queue subcommands while typing slash commands.

## 0.10.1 (2026-06-05)

### Bug Fixes

- Fix a crash when starting a goal in the TUI.

## 0.10.0 (2026-06-04)

### Features

- Users now can prepare several goals for the agent to work on sequentially. The agent will pick up the next goal from the queue once the current goal is completed. Use `/goal next <objective>` to queue a goal and `/goal next manage` to review and change the queue interactively.
- Add the built-in `update-config` skill — you can now have Kimi edit its own config files.
- Add persistent experimental feature toggles and a TUI panel that applies confirmed changes by reloading the current session.
- Add `/reload` to reload the current session and apply updated config files, plus `/reload-tui` to reload only TUI preferences.
- Add a doctor command for validating Kimi Code configuration files.

### Bug Fixes

- Normalize malformed Responses stream rate limit errors as provider rate limit failures.
- Keep managed OAuth credentials scoped to their configured authentication and API endpoints.
- Stop carrying active and queued goals into forked sessions.
- Fail early when Git Bash is missing on Windows before starting CLI sessions.
- Refresh the update target before showing foreground update prompts so the displayed version matches the install.
- Point session error diagnostics to the `/export-debug-zip` command.
- Set terminal tab titles without renaming the running process.

### Polish

- Start automatic background updates as soon as startup's fresh update check finds a newer version.
- Set the CLI process title to kimi-code during startup.
- Lowercase the stale file content message in edit tool errors.

### Refactors

- Ensure Nix-packaged CLI builds can find ripgrep and fd.

### Other

- Document the Git Bash prerequisite for Windows installs.

## 0.9.0 (2026-06-03)

### Features

- Add the `kimi acp` subcommand: kimi-code now speaks [Agent Client Protocol 0.23](https://agentclientprotocol.com/) over stdio so IDEs (Zed, JetBrains AI Chat, custom clients) can drive sessions directly — coverage matrix, Zed configuration and breaking pre-release notes are in [kimi acp Subcommand Page](https://moonshotai.github.io/kimi-code/en/reference/kimi-acp.html).
- Add `/btw` for side-channel conversations without steering the active main turn, and allow `/btw` to open the side-channel panel before entering a question.

### Bug Fixes

- Fix external editor (Ctrl+G) on Windows by removing `/bin/sh` dependency and using platform-aware shell quoting for temp file paths.
- Use the OpenAI completion token field required by newer Chat Completions models.
- Use configured model output limits for completion token caps.
- Fix goal budget tool schemas for OpenAI-compatible providers.
- Resume saved subagents lazily when they are accessed.

### Polish

- Unify the interaction and visuals across TUI dialogs and selectors.
- Log enabled experimental flags at startup.

### Refactors

- Allow SDK runtime creation to use a separate RPC client while preserving local CLI startup.

## 0.8.0 (2026-06-02)

### Features

- Add experimental goal mode for longer tasks that need more than one turn. Turn it on with `KIMI_CODE_EXPERIMENTAL_GOAL_COMMAND=1` before you start Kimi.

  Use `/goal <objective>` in the TUI when you want Kimi to keep working on one task across turns. For example:

  ```text
  /goal Fix the failing checkout test
  ```

  Kimi shows the goal in the TUI and keeps progress visible while it works. Use `/goal status`, `/goal pause`, `/goal resume`, `/goal cancel`, and `/goal replace <objective>` to manage the goal. This feature is still experimental. Try it and tell us what would make it more useful.
- Add `kimi provider` CLI subcommand with `add`, `remove`, `list`, and `catalog list` / `catalog add` actions, so providers from a custom registry (api.json) or the public models.dev catalog can be imported and managed without launching the TUI.
- Add background structured questions so agents can continue while waiting for user answers.
- Add background automatic upgrades, which can be disabled in tui.toml.
- Add `/undo` slash command to withdraw the last prompt from conversation history, and keep replay records in sync when a prompt is undone.
- Add a `kimi upgrade` command for manually checking and upgrade Kimi Code CLI.
- Add approval lifecycle hook events for observing pending and completed permission prompts.
- Allow subagents to use custom tools registered on their parent agent.
- Allow glob searches to target explicit absolute paths outside the workspace.

### Bug Fixes

- Fix cross-provider replay failures from incompatible tool call IDs and unsigned Claude thinking history.
- Fix custom registry provider handling during re-import. Prevent loss of multi-provider entries and remove stale providers along with their model aliases and default model references.
- Fix tool output preview rendering: trim trailing empty lines, append ellipsis to multi-line Bash command headers, and truncate long single-line output by visual wrapped lines instead of raw newline count.
- Fix slash-activated skills not being recognized by the model due to missing system reminder wrapper.
- Fix a crash in the `/sessions` picker on very narrow terminals by clamping every rendered line to the terminal width.
- Normalize glob patterns before brace expansion to prevent incorrect path matching.
- Prevent modified keyboard release sequences from appearing after exiting the CLI.
- Fix Git Bash path detection on Windows by also searching `usr\bin\bash.exe` locations, which is where bash lives in many Git for Windows installations where `bin\bash.exe` does not exist.

### Polish

- Show MCP server summary in the welcome panel and add configuration hints in the /mcp command output.
- Point users to `/provider` instead of the removed `/connect` command in the welcome screen and the no-models-configured hint.
- Append the current todo list as markdown to compaction summaries before writing them to history.
- Show the full model name in the footer status bar instead of truncating the provider prefix.
- Remind the model to refresh TodoList during long-running tasks and strengthen TodoList progress-tracking guidance.
- Replace chalk named color with theme-aware hex in session-directory warning.

### Refactors

- Consolidate background task management under the agent background runtime.

## 0.7.0 (2026-06-02)

### Features

- Add `/provider` command for managing AI providers, support custom registry imports, and introduce a tabbed model selector. It replaces the deprecated `/connect` command — use `/provider` instead.
- Render scheduled reminders distinctly in the TUI, expose cron fired events to SDK clients, and report cron fire times with local timezone offsets.
- Add `KIMI_MODEL_ADAPTIVE_THINKING` (and a matching `adaptive_thinking` model-alias field) to force adaptive thinking (`thinking: { type: 'adaptive' }`) on or off, overriding the Anthropic model-name version inference. This lets custom-named compatible endpoints that back an adaptive-capable model opt in even when the model name does not encode a parseable Claude version.

### Bug Fixes

- Report truncated compaction summaries clearly and apply valid completion token budgets across supported providers.
- Fix glob pattern backslash escaping and include match count in truncation messages.

### Polish

- Clarify Kimi Platform API key login labels and prompt details.
- Polish a small TUI visual interaction.

## 0.6.0 (2026-05-29)

### Features

- Add a `KIMI_MODEL_*` environment-variable channel that lets you run Kimi Code against a specific model (provider type, base URL, API key, context size, capabilities, and thinking settings) without editing `config.toml`.
- Install plugins directly from GitHub repository URLs, and surface each install's origin and trust level (kimi-official, curated, third-party) in the plugin manager.

### Bug Fixes

- Show the real terminal status of background agents in the transcript so lost, failed, and killed ones no longer appear as completed, and include the resume agent id and recovery instructions in the failure notification so the model can resume reliably.
- Recover from provider model token limit errors during long conversations.
- Automatically retry when a model response stream is dropped mid-flight (a `terminated` error) instead of failing the turn.
- Handle context overflow errors consistently across provider responses.
- Back off failed compaction retries by a fixed slice of the model context window.
- Fix the native self-updater reporting a successful update when the install command actually failed.
- Project persisted hook and blocked prompt messages into model context.
- Keep blocked prompt hook conversations available to subsequent model turns.
- Fix footer leaking onto the terminal when resuming a non-existent session.
- Fix automatic ripgrep installation when temporary files are on another filesystem.

### Polish

- Remove the default per-turn step limit of 1000. Users can still set `max_steps_per_turn` in config to enforce a custom limit.
- Support querying sessions by sessionId or workDir in listSessions, and show a helpful cd command when resuming a session from a different working directory.
- Expand the footer's rotating tips to surface more commands and shortcuts, featuring newer and important ones more prominently.
- Improve the usage information display in the TUI.
- Restrict plugin trust badges to Kimi-hosted plugin CDN URL patterns.
- Clarify subagent and background task stop messages as user-initiated.
- Align the datasource plugin with the generic two-tool workflow.

### Refactors

- Introduce `ModelProvider` interface and `SingleModelProvider` to decouple `Agent` from `ProviderManager`.
- Split `RuntimeConfig` into `Kaos` and `ToolServices` and update all references accordingly.
- Slim the LLM diagnostic logs with fewer, more compact fields.
- Relocate shared tool service typing to the tool support layer.

## 0.5.0 (2026-05-28)

### Features

- Add scheduled tasks:

  You can now ask the agent to remind you at a specific time, run a task on a recurring cron schedule (for example, check a deploy every 5 minutes or run a daily report every weekday at 9am), or come back on its own in a few minutes to continue what it was doing.

  Schedules use the standard 5-field cron syntax.

- Add `/auto` slash command and `--auto` CLI flag for auto permission mode.
- Show file content and diff in Write and Edit approval prompts, and open them in a dedicated full-screen viewer on ctrl+e instead of expanding inline.

### Bug Fixes

- Fix compaction to handle edge cases where no messages are compactable and improve retry logic.
- Fix official datasource tools to preserve complete responses and write returned result files.
- Fix migration mapping the legacy `default_yolo` key to the dead `yolo` field instead of `default_permission_mode`.

### Polish

- Add a clickable changelog link to the update prompt.
- Show the full Bash command when expanding a Bash tool card with `ctrl+o`. The header still truncates long commands at 60 chars, but the expanded view now reveals the complete multi-line command above the output.
- Shorten the session title written to the terminal window/tab from 80 to 32 characters so long first messages and pasted content no longer stretch the tab bar past readable width.
- Cap the inline todo panel at five rows and show a `+N more` indicator so long task lists no longer fill the screen.
- Clarify plugin manager keyboard shortcuts and show plugin state changes inline.
- Report discovered plugin skills in plugin manager summaries.
- Offload large base64 media payloads from `wire.jsonl` into external blob files to reduce wire size and memory pressure during session replay. Includes an in-memory read-through cache on `BlobStore` so repeated rehydration avoids redundant disk reads.
- Wrap long question, body, and option text in the AskUserQuestion dialog instead of truncating with an ellipsis. The question prompt, body description, option label, option description, and submit-tab review entries now flow onto multiple lines with a hanging indent.

### Refactors

- Refactor TUI code structure.

## 0.4.0 (2026-05-27)

### Features

- Add user-global plugin installation, interactive plugin management, plugin-provided skills, and plugin-owned MCP servers.
- Expand folded paste markers on second paste.
- Rework tool permissions: reads outside cwd no longer prompt, session approvals match the exact call, and path-based rules are case-insensitive.
- Add `/export-debug-zip` slash command to export the current session as a debug ZIP archive directly from the TUI.
- Add `/export-md` slash command to export the current session as a Markdown file.

### Bug Fixes

- Prevent the TUI from crashing when pull request lookup fails during startup.
- Fix thinking spinner leaking past turn end when an empty thinking delta creates an orphaned thinking component.
- Show the original session resume command after forking a session.
- Restrict plugin zip installs to manifests at the archive root or a single wrapper directory.
- Route session-tagged log entries exclusively to the session sink instead of duplicating them to the global sink. Consistently omit stable main-agent context keys from all session log lines that carry `agentId=main`.

### Refactors

- Refactor TUI resume replay logic.
- Use one retry classification for transient LLM failures across regular turns and compaction.

### Other

- Enhance `kimi export` to include more diagnostic information in the manifest.

## 0.3.0 (2026-05-26)

### Features

- `/logout` now opens a picker so you can choose which provider to log out of, instead of always logging out the one tied to the current model. The current provider is highlighted by default, so pressing Enter matches the previous behavior. The command is also available as `/disconnect`.
- The `openai` provider now works out of the box for OpenAI-compatible reasoner models: it auto-detects thinking fields in responses (`reasoning_content` / `reasoning_details` / `reasoning`) and auto-injects `reasoning_effort` when history contains prior thinking. DeepSeek, Qwen, One API and other gateway-fronted services no longer need a hand-set `reasoning_key`, which remains available as an explicit override for non-standard gateways.

### Bug Fixes

- Prevent running the `/model` and `/sessions` slash commands while streaming or compacting context.
- Preserve catalog-declared interleaved reasoning fields for OpenAI-compatible models configured through `/connect`.
- Fix API key input dialog showing a masked dot in empty state.
- Fix user skills in `~/.agents/` not being loaded.
- Restore real-time token display for running subagents in the TUI.
- Hide the todo panel on resume when all todos are already completed.
- Always emit a paired tool result when a tool returns a malformed or missing result, preventing the next request from failing with a missing tool_call_id error.
- Fix Plan mode session resets so new sessions no longer fail after plan review rejection and continue receiving events after setup errors.
- Exit promptly when the controlling terminal goes away. The TUI now handles `SIGHUP` / `SIGTERM` and stdout/stderr `EIO` / `EPIPE` / `ENOTCONN` errors, preventing leftover `kimi` processes that pin a CPU core after the parent shell or multiplexer dies unexpectedly.
- Avoid overly small local completion caps that can truncate reasoning before summaries are produced.

### Refactors

- Make `AgentRecords` hold the `Agent` instance directly and inline the restore dispatch logic.

### Other

- Improve the Write tool UX.

## 0.2.0 (2026-05-26)

### Features

- Add a `/connect` command that configures a provider and model from a model catalog.
- The `/connect` provider and model pickers now support type-to-search filtering, and long lists are paginated. The `/model` picker is also paginated when many models are configured.
- Add `Ctrl-J` as an additional shortcut for inserting new lines in the TUI prompt.
- Add wire record migration handling during session replay.
- Migrate user skills from `~/.kimi/skills/` to `~/.kimi-code/skills/` during the first-launch migration; existing target skills are kept.
- Emit session resume hint as a structured meta message in stream-json output format.

### Bug Fixes

- Report the macOS product version in OAuth device information instead of the Darwin kernel version.
- Correct the `X-Msh-Platform` header value to `kimi_code_cli`.
- Clarify the prompt-mode error when no model is configured by pointing users to the login flow.
- Hide the empty current session from the sessions picker while keeping other empty sessions visible.
- Stop mentioning OAuth credentials in the migration UI — they are never migrated, so the previous "needs /login" notice misread as a failure. OAuth-only installs no longer trigger the migration screen.
- Surface API-provided error messages during feedback, usage, login, and model setup failures.
- Persist model selections from the terminal UI to the default configuration, and honor the configured default thinking state for new sessions.
- Retry compaction responses that do not contain a summary before updating conversation history.
- Avoid CPU spikes from large streamed tool arguments and coalesce high-frequency streaming UI updates.
- Resume sessions with a newer wire protocol version instead of failing. A warning is now shown in the TUI and records are replayed without migration.
- Warn tmux users when extended key settings may prevent modified Enter shortcuts from working.
- Let Kimi requests use the remaining context window for completion tokens by default while keeping explicit environment limits as hard caps.

### Refactors

- Flatten tool call data by inlining tool names and arguments at the top level, and limit legacy record migration so it only rewrites matching tool call payloads.
- Move wire metadata handling into the record layer and keep persistence backends limited to storage operations.

### Other

- When no models are configured, `/model` and the welcome panel now point users to `/login` (for Kimi) and `/connect` (for other providers).
