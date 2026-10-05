# Tarefas — Agenda Expofeira

Quadro compartilhado entre Claude, Codex e Antigravity. Regras em `AGENTS.md`.

## Em andamento

_(nenhuma)_

## A fazer (por prioridade)

1. **Cores por categoria nos cards:** a etiqueta já mostra ícone e nome do `tipo`, mas o fundo dos cards ainda alterna azul/dourado (`nth-child(even)`). Usar `color`/`soft` de `tipos` e adicionar filtro por tipo.
2. **Revisar dados do JSON:**
   - Acentos perdidos em 09/10: "Danzas Folkloricas Argentinas Jiron Gaucho" / "Musica" → "Folklóricas", "Jirón", "Música".
   - 09/10: "Julgamento de Admissão da Raça Jersey" às 13h e às 18h — possível duplicata.
   - Confirmar "Hereford de Braford" (08/10) e Montana na "Casa da Amizade" (06/10).
   - Workshop de Carnes Angus (06/10, 19h) está sem local.
   - Leilão Só Angus (11/10) com horário a confirmar (18h ou 19h).
3. **Compartilhamento:** adicionar `<meta name="description">`, tags Open Graph e favicon (o link circula no WhatsApp).
4. **README:** a seção "Funcionalidades" ainda cita visões semana/dia/lista e filtros da 1ª versão (commit `ac42957`) que não existem mais; a tabela de arquivos cita `agenda-expofeira.html`, que foi removido.
5. Opcional: renomear `expofeira.html` para `index.html` (URL mais curta) — avisar o dono antes, muda o link publicado.

## Concluído

- **HTML carrega `eventos.json`** (Codex): estados de carregamento/erro, validação dos dados, categoria, observação e fonte em cada card, eventos com horário a confirmar agrupados no fim.
- **Restaurantes e experiências** (Codex): painéis no topo, a partir de `restaurantes`, `experiencias` e `fonte_informacoes`.
- **Novos eventos** (Codex): Workshop de Carnes Angus (06/10) e shows no Palco da Rua Coberta — Beto Borges (07/10), Ñanderekó Chamamé (10/10), Os Andeiros (11/10). Total: 128 eventos.
- **Funcionamento do dia** (Claude): bloco abaixo da data, lido de `funcionamento` + `feriados`, distinguindo dias úteis e fim de semana/feriado. Fazendinha corrigida para 13h30 nos dias úteis.

## Registro

| Data | Agente | O que foi feito |
|---|---|---|
| 2026-10-05 | Claude | Análise do projeto; horários de funcionamento; criados `AGENTS.md`, `CLAUDE.md`, `GEMINI.md` e este arquivo. |
| 2026-10-05 | Codex | Integração HTML ↔ JSON, restaurantes, experiências e novos eventos (deixado sem commit na worktree `~/.codex/worktrees/abbc` quando o limite acabou). |
| 2026-10-05 | Claude | Trouxe o trabalho do Codex para a `main`; `funcionamento` unificado no formato estruturado (lido pelo bloco do dia); painéis do topo ficaram só com restaurantes e experiências; fonte única `cards_usuario`; AGENTS.md atualizado (JSON é a fonte única). |
| 2026-10-05 | Claude | Dias em grade (todos visíveis, sem rolagem lateral — não era possível chegar a 11 e 12/10); etiquetas de categoria sem emoji. |
