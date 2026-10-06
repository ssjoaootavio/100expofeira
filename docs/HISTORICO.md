# Histórico do projeto

Registro do que foi feito, por quem e **por quê** — para que Claude, Codex e Antigravity entendam as decisões antes de mudar algo. Ordem cronológica. Detalhes técnicos em [`ARQUITETURA.md`](ARQUITETURA.md); tarefas em aberto em [`../TAREFAS.md`](../TAREFAS.md).

## Antes de 05/10/2026 — base do projeto

| Commit | O que foi feito |
|---|---|
| `ac42957` | Primeira versão: agenda com visões semana/dia/lista e filtros por tipo. |
| `5fb3018` | "Nova interface": página refeita com a identidade da Expofeira (azul-marinho e dourado, Montserrat, capa com folha recortada). As visões semana/lista e os filtros da 1ª versão **saíram**. Criado `eventos.json` (124 eventos), ainda não usado pela página. |
| `331fe6c` | README do projeto. |

## 05/10/2026 — primeiro dia da feira

### Análise inicial (Claude)
Problemas encontrados: a página mostrava só os 109 eventos embutidos no HTML (o JSON tinha 124); o README descrevia a 1ª versão; acentos perdidos no JSON; possíveis duplicatas. Virou a lista de prioridades do `TAREFAS.md`.

### Horários de funcionamento + coordenação entre agentes — `f245d7e` (Claude)
- Origem: arte oficial com horários de Expositores, Fazendinha, Praça de Alimentação Gertum e Food Hall e Conferência Rural.
- Bloco **Funcionamento** abaixo da data, que muda conforme o dia: dias úteis × sábados/domingos/feriado (12/10 é feriado).
- Fazendinha corrigida de 14h para **13h30** nos dias úteis (a arte oficial prevalece sobre o jornal).
- Criados `AGENTS.md` (regras únicas), `CLAUDE.md` e `GEMINI.md` (apontam para o AGENTS.md) e `TAREFAS.md` (quadro + registro).
- **Motivo:** o dono alterna entre Claude, Codex e Antigravity conforme os limites de uso; cada agente começa sem o contexto do outro.

### Integração do trabalho do Codex — `f272ea7` (Codex + Claude)
- O Codex tinha feito, **sem commit** numa worktree própria (`~/.codex/worktrees/abbc`) quando o limite dele acabou: página passa a ler `eventos.json` (carregamento, erro, validação, categoria, observação e fonte nos cards), painéis de Restaurantes e Experiências, Workshop de Carnes Angus e 3 shows.
- Claude juntou os dois trabalhos: base do Codex; `funcionamento` no formato estruturado (para o bloco por dia); painéis do topo ficaram só com restaurantes e experiências (evita horários duplicados); uma só fonte (`cards_usuario`) para as artes.
- **Decisão:** `eventos.json` passa a ser a fonte única dos dados. Nada de dados no HTML.
- **Lição:** regra 6 do protocolo no AGENTS.md — commit cedo e com frequência.
- Branches extras removidas; ficou só a `main`.

### Dias em grade e categorias sem emoji — `1396bcc` (Claude)
- **Problema relatado:** não era possível chegar aos dias 11 e 12. Os botões ficavam numa linha com rolagem lateral e barra escondida — no computador não rolava com o mouse.
- **Solução:** grade 4×2, os 8 dias sempre visíveis. No celular, dia da semana abaixo da data.
- Emojis removidos das etiquetas de categoria, a pedido do dono (o campo `icon` continua no JSON, mas não é exibido).

### Destaque do dia atual — `91b6693` (Claude)
- A página já abria no dia de hoje; como 05/10 é o primeiro dia, não dava para perceber.
- Botão do dia mostra **HOJE** (contorno dourado) e a data exibe "Hoje, …".
- Aba deixada aberta acompanha a virada do dia ao voltar a ficar visível, sem desfazer a escolha da pessoa.
- **Incidente:** durante o teste, a data foi simulada como 10/10 e a página ficou nesse estado no painel do navegador; o dono viu "HOJE" no 10/10 e achou que era bug. Não era. Daí a armadilha documentada: recarregar a página depois de simular datas.

### Programação cultural oficial — `d4e5fa7`, `d530dd2` (Claude)
- Origem: carrossel "Programação Cultural" e posts de shows do @expofeirapelotas.
- **8 eventos novos:** O Chamamé e o Rasguido Doble (06/10); CTG Cantinhos da Tradição, Jirón Gaucho + Entre Amigos (2ª apresentação), CTG Raízes, Grupo Carqueja (10/10); Tri Baile (11/10); Ballet Sabrina de Freitas, Grupo de Invernada Escola Libório (12/10).
- **Sem duplicar:** Beto Borges, Ñanderekó Chamamé e Os Andeiros já existiam (vindos dos posts com "Palco da Rua Coberta"); ajustados para "Palco Tropa Entregue", como na agenda oficial, com a divergência em `observacao`. Tholl e Quarteto Coração de Potro ganharam o nome oficial ("Sicredi apresenta…").
- Acentos de "Jirón Gaucho" (09/10) corrigidos. Total: **136 eventos**.
- Pendências para o dono confirmar: se "Rua Coberta" e "Tropa Entregue" são o mesmo palco; nome do CTG/DTG de 10/10 às 15h.

### Compartilhar nos stories (9:16) — `d7afc49` (Claude)
- Botão **Stories** em cada card gera imagem 1080×1920 com a identidade da feira (data, horário, título, categoria, local, endereço da agenda).
- Prévia em `<dialog>`: **Compartilhar** (folha do sistema → Instagram → Stories, no celular) ou **Baixar imagem** (computador).
- **Decisões:** canvas puro, sem bibliotecas; prévia antes de compartilhar porque o iOS exige um toque recente para `navigator.share`; não há API web para postar direto nos stories.

### Documentação para os agentes (Claude)
- Criados `docs/ARQUITETURA.md` (como o código funciona) e este `docs/HISTORICO.md`; README atualizado para o estado real; AGENTS.md aponta a leitura obrigatória.

### Fonte única e limpeza das observações (Claude)
- **Pedido do dono:** remover as notas internas dos cards (ex.: "a fonte jtr anteriormente utilizada indicava 14h…") e exibir **somente "Instagram 100ª Expofeira de Pelotas"** como fonte.
- `fontes` passou a ter só `instagram` (padrão de todos os eventos). Observações reduzidas a 2, ambas úteis ao visitante: 2ª apresentação do Jirón Gaucho (10/10) e horário a confirmar do Leilão Só Angus (11/10).
- Divergências e eventos a confirmar ficam no `TAREFAS.md` (uso interno), não nos cards.
- **Atenção:** 5 leilões e o local da Fazendinha vieram originalmente do Jornal Tradição Regional e agora exibem Instagram como fonte — listados no `TAREFAS.md` para conferência.
- As linhas `Co-Authored-By: Claude` foram removidas de todos os commits (histórico reescrito). O push forçado só chegou ao GitHub em 06/10, junto com o layout de desktop. Commits futuros não devem ter atribuição a agentes de IA.

## 06/10/2026

### Layout de desktop em duas colunas (Claude)
- **Origem:** avaliação em áudio de uma designer. No computador, a página ficava como uma coluna estreita de 640px no centro e sobrava espaço nas laterais. Ela sugeriu restaurantes e experiências fixos à esquerda, a agenda à direita e os eventos em blocos ("enquadradinho"), não em lista.
- Em telas ≥1024px: lateral fixa com Funcionamento (no topo, porque muda com o dia), Restaurantes e Experiências já abertos; à direita, os 8 dias em uma linha, busca, data e cards em grade (3 colunas em 1366px).
- **Celular sem mudança**: mesma ordem e mesmo visual (as colunas usam `display:contents` abaixo de 1024px).
- Outros pontos da avaliação ficaram para depois: mapa e lista de expositores (não há material oficial; não inventar), destaque para a programação infantil (dados já existem — ver tarefa de cor e filtro por categoria) e ícones (a regra "sem emojis" continua valendo).

## Preferências do dono do projeto (João Santos)

- Tudo em **português do Brasil**; datas no padrão **DD/MM/AAAA**.
- Uso principal é no **celular** (público da feira, link circula no Instagram/WhatsApp).
- O dono faz o **push** para o GitHub (que publica o site). Agentes fazem commit na `main` só quando ele pede.
- Ao receber imagens/posts com programação: conferir **duplicatas** antes de adicionar; divergências vão para o `TAREFAS.md`.
- Fonte exibida: **somente Instagram 100ª Expofeira de Pelotas**. Cards sem notas internas.
- Commits **sem** `Co-Authored-By` ou atribuição a agentes de IA.
