# Tarefas — Agenda Expofeira

Quadro compartilhado entre Claude, Codex e Antigravity. Regras em `AGENTS.md`; como o código funciona em `docs/ARQUITETURA.md`; decisões passadas em `docs/HISTORICO.md`.

## Em andamento

_(nenhuma)_

## A fazer (por prioridade)

1. **Cores por categoria nos cards:** a etiqueta já mostra o nome do `tipo` (sem emoji), mas o fundo dos cards ainda alterna azul/dourado (`nth-child(even)`). Usar `color`/`soft` de `tipos` e adicionar filtro por tipo.
2. **Revisar dados do JSON:**
   - Palco dos shows de Beto Borges, Ñanderekó Chamamé e Os Andeiros: posts individuais dizem "Palco da Rua Coberta", agenda cultural diz "Palco Tropa Entregue" (adotado). Confirmar se são o mesmo palco.
   - 10/10: confirmar o nome "CTG Cantinhos da Tradição" (comentário no Instagram sugere "DTG Caminhos da...").
   - 09/10: "Julgamento de Admissão da Raça Jersey" às 13h e às 18h — possível duplicata.
   - Confirmar "Hereford de Braford" (08/10) e Montana na "Casa da Amizade" (06/10).
   - Workshop de Carnes Angus (06/10, 19h) está sem local.
   - Leilão Só Angus (11/10) com horário a confirmar (18h ou 19h).
3. **Compartilhamento:** adicionar `<meta name="description">`, tags Open Graph e favicon (o link circula no WhatsApp).
4. **Stories:** testar o compartilhamento em celulares reais (Android Chrome e iPhone Safari) e dentro do navegador embutido do Instagram; ajustar a imagem se algo ficar sob as barras do app.
5. Opcional: renomear `expofeira.html` para `index.html` (URL mais curta) — avisar o dono antes, muda o link publicado.

## Concluído

- **HTML carrega `eventos.json`** (Codex): estados de carregamento/erro, validação dos dados, categoria, observação e fonte em cada card, eventos com horário a confirmar agrupados no fim.
- **Restaurantes e experiências** (Codex): painéis no topo, a partir de `restaurantes`, `experiencias` e `fonte_informacoes`.
- **Novos eventos** (Codex): Workshop de Carnes Angus (06/10) e shows no Palco da Rua Coberta — Beto Borges (07/10), Ñanderekó Chamamé (10/10), Os Andeiros (11/10). Total: 128 eventos.
- **Dias em grade, HOJE e virada do dia** (Claude): todos os dias visíveis; dia atual destacado.
- **Programação cultural oficial** (Claude): 8 eventos novos, duplicatas unificadas. Total: 136 eventos.
- **Compartilhar nos stories** (Claude): imagem 1080×1920 por evento, prévia com Compartilhar/Baixar.
- **Documentação** (Claude): `docs/ARQUITETURA.md`, `docs/HISTORICO.md`, README atualizado.
- **Funcionamento do dia** (Claude): bloco abaixo da data, lido de `funcionamento` + `feriados`, distinguindo dias úteis e fim de semana/feriado. Fazendinha corrigida para 13h30 nos dias úteis.

## Registro

| Data | Agente | O que foi feito |
|---|---|---|
| 2026-10-05 | Claude | Análise do projeto; horários de funcionamento; criados `AGENTS.md`, `CLAUDE.md`, `GEMINI.md` e este arquivo. |
| 2026-10-05 | Codex | Integração HTML ↔ JSON, restaurantes, experiências e novos eventos (deixado sem commit na worktree `~/.codex/worktrees/abbc` quando o limite acabou). |
| 2026-10-05 | Claude | Trouxe o trabalho do Codex para a `main`; `funcionamento` unificado no formato estruturado (lido pelo bloco do dia); painéis do topo ficaram só com restaurantes e experiências; fonte única `cards_usuario`; AGENTS.md atualizado (JSON é a fonte única). |
| 2026-10-05 | Claude | Dias em grade (todos visíveis, sem rolagem lateral — não era possível chegar a 11 e 12/10); etiquetas de categoria sem emoji. |
| 2026-10-05 | Claude | Dia atual destacado como HOJE (botão e data); a aba aberta acompanha a virada do dia ao voltar a ficar visível, salvo se a pessoa escolheu outro dia. |
| 2026-10-05 | Claude | Agenda cultural oficial: 8 eventos novos (06, 10, 11 e 12/10); Tholl e Quarteto renomeados ("Sicredi apresenta..."); acentos de Jirón Gaucho corrigidos; shows da Rua Coberta unificados sem duplicar. Total: 136 eventos. |
| 2026-10-05 | Claude | Compartilhar evento nos stories (imagem 9:16); documentação completa para os agentes (`docs/ARQUITETURA.md`, `docs/HISTORICO.md`), README atualizado ao estado real, AGENTS.md com leitura obrigatória. |
