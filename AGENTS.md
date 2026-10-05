# AGENTS.md — Agenda da 100ª Expofeira de Pelotas

Instruções compartilhadas para **todos os agentes de IA** que trabalham neste repositório: Claude Code, OpenAI Codex e Google Antigravity (ou qualquer outro). Este arquivo é a fonte única de regras; `CLAUDE.md` e `GEMINI.md` apenas apontam para cá.

O dono do projeto alterna entre agentes conforme os limites de uso de cada um. Por isso, **todo agente deve deixar o trabalho em um estado que o próximo consiga continuar sem contexto da conversa anterior.**

## Projeto

- Agenda estática da 100ª Expofeira de Pelotas (5 a 12/10/2026), publicada no GitHub Pages: https://ssjoaootavio.github.io/100expofeira/expofeira.html
- Repositório: https://github.com/ssjoaootavio/100expofeira — branch publicada: `main`.
- Sem framework, sem build, sem dependências. HTML + CSS + JavaScript puro.
- Idioma: tudo em **português do Brasil** (interface, código, comentários, commits).

## Arquivos

| Arquivo | Papel |
|---|---|
| `expofeira.html` | Página publicada. Hoje os eventos (`DATA`) e os horários de funcionamento (`HOURS`) estão **embutidos no HTML**. |
| `eventos.json` | Base de dados mais completa (124 eventos, categorias, fontes, funcionamento). **Ainda não é carregada pelo HTML.** |
| `README.md` | Documentação para pessoas, inclusive o formato do JSON. |
| `TAREFAS.md` | Quadro de tarefas e registro de passagem de bastão entre agentes. |

## Regras de dados

- Enquanto o HTML não carregar o JSON, **toda alteração de programação ou horário precisa ser feita nos dois lugares** (`DATA`/`HOURS` no HTML e `eventos.json`).
- O formato do JSON está descrito no `README.md`. Resumo: cada evento é `["HH:mm", "titulo", "tipo", "local", "fonte", "observacao"]`; horário/local desconhecido = `null`; `tipo` e `fonte` precisam existir em `tipos` e `fontes`.
- No HTML, os horários dos eventos usam o formato curto `"8h"`, `"13h30"`.
- Nunca invente evento, horário ou local. Se a fonte for ambígua, registre a dúvida em `observacao` e cite a `fonte`.
- Preserve acentuação correta dos nomes (ex.: "Folklóricas", "Jirón").
- 12/10/2026 é feriado (`HOLIDAYS` no HTML, `feriados` no JSON).

## Regras de código

- Manter o estilo atual: JavaScript compacto, sem bibliotecas, CSS com as variáveis de `:root` (azul-marinho `--navy-*`, dourado `--gold-*`, fonte Montserrat).
- Escapar todo texto vindo de dados com `esc()` antes de inserir em `innerHTML`.
- A página precisa funcionar bem em celular (375px de largura) — é o uso principal.
- Arquivos usam UTF-8 e quebras de linha CRLF (Windows, `core.autocrlf=true`).

## Como validar antes de entregar

```bash
node -e "JSON.parse(require('fs').readFileSync('eventos.json','utf8')); console.log('JSON ok')"
```

- Abra `expofeira.html` no navegador, troque entre os dias (inclusive 10, 11 e 12/10) e teste a busca.
- Para testar `fetch('./eventos.json')`, use um servidor local (`npx serve .` ou `python -m http.server`); via `file://` o fetch falha.

## Protocolo de passagem de bastão

1. **Ao começar:** leia este arquivo e o `TAREFAS.md`. Rode `git status` e `git log --oneline -5` para ver o que o agente anterior deixou.
2. **Ao pegar uma tarefa:** mova-a para "Em andamento" no `TAREFAS.md` com seu nome (Claude / Codex / Antigravity).
3. **Ao terminar ou ser interrompido:** atualize o `TAREFAS.md` — o que foi feito, o que falta, decisões tomadas — e adicione uma linha no "Registro".
4. **Commits:** pequenos, em português, descrevendo o que mudou. Não faça push nem merge na `main` sem o dono pedir: a `main` é publicada automaticamente.
5. Não apague nem reescreva trabalho de outro agente sem registrar o motivo no `TAREFAS.md`.
