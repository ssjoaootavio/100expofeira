# Tarefas — Agenda Expofeira

Quadro compartilhado entre Claude, Codex e Antigravity. Regras em `AGENTS.md`.

## Em andamento

_(nenhuma)_

## A fazer (por prioridade)

1. **Carregar `eventos.json` no HTML.** Hoje o site mostra 109 eventos do `DATA` embutido; o JSON tem 124 (faltam os 15 da fonte `jtr`, como Fazendinha, leilões, Tholl). Cuidados:
   - `mins()` espera `"8h"`; o JSON usa `"08:00"` e tem horário `null` ("Leilão Só Angus", 11/10) — exibir "A confirmar" e ordenar no fim.
   - Manter `DATA` como reserva se o `fetch` falhar (ex.: aberto via `file://`).
   - Ler `funcionamento` e `feriados` do JSON em vez de `HOURS`/`HOLIDAYS`.
   - A lista `CONF` usa `'Vetesul'`, mas o JSON usa `'VETESUL'` — comparar sem diferenciar maiúsculas.
2. **Categorias visíveis:** usar `tipos` (cor, ícone, nome) nos cards e adicionar filtro por tipo. Hoje as cores só alternam azul/dourado.
3. **Revisar dados do JSON:**
   - Acentos perdidos em 09/10: "Danzas Folkloricas Argentinas Jiron Gaucho" / "Musica" (o HTML está correto).
   - "Agro, Comunica…" (HTML) vs "Agro Comunica…" (JSON).
   - 09/10: "Julgamento de Admissão da Raça Jersey" às 13h e às 18h — possível duplicata.
   - Confirmar "Hereford de Braford" (08/10) e Montana na "Casa da Amizade" (06/10).
4. **Compartilhamento:** adicionar `<meta name="description">`, tags Open Graph e favicon (o link circula no WhatsApp).
5. **README desatualizado:** descreve visões semana/dia/lista e filtros da 1ª versão (commit `ac42957`) que não existem mais; cita `agenda-expofeira.html`, que foi removido.
6. Opcional: renomear `expofeira.html` para `index.html` (URL mais curta) — avisar o dono antes, muda o link publicado.

## Concluído

- Horários de funcionamento (Expositores, Fazendinha, Praça de Alimentação Gertum e Food Hall, Conferência Rural) exibidos por dia, distinguindo dias úteis e fim de semana/feriado. Dados em `HOURS` (HTML) e `funcionamento` (JSON). Fazendinha corrigida no JSON de 14h para 13h30 nos dias úteis.

## Registro

| Data | Agente | O que foi feito |
|---|---|---|
| 2026-10-05 | Claude | Análise do projeto; horários de funcionamento no HTML e no JSON; criados `AGENTS.md`, `CLAUDE.md`, `GEMINI.md` e este arquivo. |
