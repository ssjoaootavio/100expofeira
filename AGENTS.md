# AGENTS.md — Agenda da 100ª Expofeira de Pelotas

Instruções compartilhadas para **todos os agentes de IA** que trabalham neste repositório: Claude Code, OpenAI Codex e Google Antigravity (ou qualquer outro). Este arquivo é a fonte única de regras; `CLAUDE.md` e `GEMINI.md` apenas apontam para cá.

O dono do projeto alterna entre agentes conforme os limites de uso de cada um. Por isso, **todo agente deve deixar o trabalho em um estado que o próximo consiga continuar sem contexto da conversa anterior.**

## Leitura obrigatória antes de programar

1. Este arquivo (regras).
2. [`TAREFAS.md`](TAREFAS.md) — o que está em andamento e o que falta.
3. [`docs/ARQUITETURA.md`](docs/ARQUITETURA.md) — como o código e os dados funcionam (funções, estado, validação, stories, armadilhas).
4. [`docs/HISTORICO.md`](docs/HISTORICO.md) — o que já foi feito e **por quê**. Não desfaça uma decisão registrada ali sem falar com o dono.

## Projeto

- Agenda estática da 100ª Expofeira de Pelotas (5 a 12/10/2026), publicada no GitHub Pages: https://ssjoaootavio.github.io/100expofeira/expofeira.html
- Repositório: https://github.com/ssjoaootavio/100expofeira — branch publicada: `main`.
- Sem framework, sem build, sem dependências. HTML + CSS + JavaScript puro.
- Idioma: tudo em **português do Brasil** (interface, código, comentários, commits).

## Arquivos

| Arquivo | Papel |
|---|---|
| `expofeira.html` | Página publicada. Carrega tudo de `eventos.json` via `fetch` — **não há dados embutidos no HTML**. |
| `eventos.json` | **Fonte única dos dados**: eventos por dia, categorias, fontes, feriados, funcionamento, restaurantes e experiências. |
| `README.md` | Documentação para pessoas, inclusive o formato do JSON. |
| `TAREFAS.md` | Quadro de tarefas e registro de passagem de bastão entre agentes. |
| `docs/ARQUITETURA.md` | Documentação técnica do código e dos dados. Atualize quando mudar funções, estado ou formato do JSON. |
| `docs/HISTORICO.md` | Histórico de entregas e decisões. Acrescente uma seção ao concluir algo relevante. |
| `CLAUDE.md`, `GEMINI.md` | Só apontam para este arquivo. |

## Regras de dados

- Toda alteração de programação ou horário é feita **só no `eventos.json`**. Não reintroduza dados no HTML.
- O formato do JSON está descrito no `README.md`. Resumo: cada evento é `["HH:mm", "titulo", "tipo", "local", "fonte", "observacao"]`; horário/local desconhecido = `null`; `tipo` e `fonte` precisam existir em `tipos` e `fontes`.
- O HTML valida o JSON ao carregar: um único registro inválido (horário fora de `HH:mm`, `tipo`/`fonte` inexistente) faz a página inteira mostrar erro. **Sempre valide antes de commitar.**
- `funcionamento.areas` usa `[area, seg_a_sex, sab_dom_feriado]`, com períodos `["abre", "fecha"]`; `null` = não informado.
- Nunca invente evento, horário ou local. Se a fonte for ambígua, registre a dúvida em `observacao` e cite a `fonte`.
- Preserve acentuação correta dos nomes (ex.: "Folklóricas", "Jirón").
- 12/10/2026 é feriado (`feriados` no JSON).
- Ao receber posts/imagens com programação: **procure duplicatas** (mesmo dia, título parecido, mesmo horário — o mesmo show pode ter nomes ou palcos diferentes em fontes diferentes) antes de adicionar.
- Datas no padrão brasileiro: `DD/MM` nos botões, por extenso em pt-BR nos textos. No JSON, `AAAA-MM-DD`.

## Regras de código

- Manter o estilo atual: JavaScript compacto, sem bibliotecas, CSS com as variáveis de `:root` (azul-marinho `--navy-*`, dourado `--gold-*`, fonte Montserrat).
- Escapar todo texto vindo de dados com `esc()` antes de inserir em `innerHTML`.
- A página precisa funcionar bem em celular (375px de largura) — é o uso principal.
- Os botões de dia ficam em grade (4 por linha), todos visíveis: não voltar para rolagem lateral, que impedia chegar aos dias 11 e 12.
- Não exibir emojis na interface (o campo `icon` de `tipos` no JSON não é usado na página).
- Imagem dos stories (`drawStory`): manter 1080×1920, conteúdo essencial fora dos ~250px do topo e da base (barras do Instagram), cores vindas das variáveis CSS. Não remover a prévia antes do compartilhamento (exigência do iOS).
- Arquivos usam UTF-8 e quebras de linha CRLF (Windows, `core.autocrlf=true`).

## Como validar antes de entregar

```bash
node -e "const j=require('./eventos.json');for(const d in j.dias)for(const e of j.dias[d]){if(!j.tipos[e[2]]||!j.fontes[e[4]||j.fonte_padrao]||(e[0]!==null&&!/^([01]\d|2[0-3]):[0-5]\d$/.test(e[0])))throw new Error(d+' '+e[1])}console.log('JSON ok')"
```

- Rode `python -m http.server 8765`, abra http://localhost:8765/expofeira.html, troque entre os dias (inclusive 10, 11 e 12/10), teste a busca e gere a imagem de stories de um evento.
- Teste em largura de celular (375px).
- Se simular datas no teste (sobrescrever `Date`), **recarregue a página ao terminar**.
- Abrir o HTML direto do disco (`file://`) não funciona: o `fetch` do JSON falha e a página mostra erro.

## Protocolo de passagem de bastão

1. **Ao começar:** leia este arquivo e o `TAREFAS.md`. Rode `git status` e `git log --oneline -5` para ver o que o agente anterior deixou.
2. **Ao pegar uma tarefa:** mova-a para "Em andamento" no `TAREFAS.md` com seu nome (Claude / Codex / Antigravity).
3. **Ao terminar ou ser interrompido:** atualize o `TAREFAS.md` — o que foi feito, o que falta, decisões tomadas — e adicione uma linha no "Registro".
4. **Commits:** pequenos, em português, descrevendo o que mudou. Não faça push nem merge na `main` sem o dono pedir: a `main` é publicada automaticamente.
5. Não apague nem reescreva trabalho de outro agente sem registrar o motivo no `TAREFAS.md`.
6. **Faça commit cedo e com frequência.** O limite de uso pode acabar no meio da tarefa; alteração sem commit numa worktree é invisível para o próximo agente e pode ser perdida. Se trabalhar em worktree/branch separada, registre o caminho e a branch no `TAREFAS.md`.
