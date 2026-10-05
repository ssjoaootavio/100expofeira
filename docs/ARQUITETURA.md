# Arquitetura — Agenda da 100ª Expofeira de Pelotas

Documento técnico para quem vai **programar** no projeto (pessoas ou agentes de IA: Claude, Codex, Antigravity). Regras de trabalho ficam em [`AGENTS.md`](../AGENTS.md); o histórico do que foi feito, em [`HISTORICO.md`](HISTORICO.md).

## Visão geral

```
GitHub Pages (branch main)
└── expofeira.html  ──fetch('./eventos.json')──►  eventos.json
    ├── CSS embutido (<style>)
    ├── HTML (capa + folha branca + <dialog> dos stories)
    └── JavaScript embutido (<script>), sem bibliotecas
```

- **Um arquivo de página** (`expofeira.html`) com CSS e JS embutidos. Sem build, sem npm, sem framework.
- **Um arquivo de dados** (`eventos.json`) — fonte única de eventos, horários, categorias e fontes. O HTML **não tem dados embutidos**.
- Publicação: push na `main` → GitHub Pages publica em https://ssjoaootavio.github.io/100expofeira/expofeira.html (leva 1–2 min).
- Única dependência externa: fonte **Montserrat** do Google Fonts (pesos 300–900).

## Estrutura da página (DOM)

| Elemento | id / classe | Conteúdo |
|---|---|---|
| Capa | `header.hero` | "100 Expofeira Pelotas", "Agenda / Programação Expofeira", "05 – 12 OUT." (estático) |
| Folha branca | `main.sheet > .wrap` | Todo o conteúdo dinâmico (largura máx. 640px) |
| Informações da visita | `#visitor-info` | Painéis `<details>` de Restaurantes e Experiências |
| Dias | `nav#days` | 8 botões `.day` em grade 4×2 (fixa no topo ao rolar — `position:sticky`) |
| Busca | `input#q` | Filtra por título e local |
| Data | `#pill` | "Hoje, segunda-feira, 05 de outubro" |
| Funcionamento | `section#hours` | Horários das áreas no dia selecionado |
| Eventos | `ul#list` | Cards `.card` (azul e dourado alternados) |
| Stories | `dialog#story` | Prévia da imagem 9:16 + botões Compartilhar / Baixar / Fechar |

## JavaScript

### Estado global

| Variável | Tipo | Significado |
|---|---|---|
| `agenda` | objeto | Conteúdo de `eventos.json` já validado |
| `days` | `string[]` | Datas `AAAA-MM-DD` ordenadas (chaves de `agenda.dias`) |
| `cur` | `string` | Data selecionada |
| `today` | `string` | Data de hoje no fuso `agenda.timezone` (`America/Sao_Paulo`) |
| `shown` | `object[]` | Eventos exibidos na lista (após busca e ordenação); o botão Stories usa o índice `data-i` |
| `storyFile`, `storyUrl`, `storyText` | — | Imagem gerada para os stories (File, object URL, texto do compartilhamento) |

### Funções

| Função | O que faz |
|---|---|
| `loadAgenda()` | Faz `fetch('./eventos.json')`, **valida** estrutura e cada evento, define `today`/`cur`, chama `renderVisitorInfo`, `renderNav`, `render`. Em erro mostra mensagem e loga no console. Chamada uma vez no fim do script. |
| `renderNav()` | Monta os 8 botões de dia. O dia atual recebe classe `.today`, `aria-current="date"` e o texto **HOJE** no lugar do dia da semana. |
| `render()` | Atualiza botão pressionado, `#pill`, chama `renderHours()`, filtra pela busca, ordena por hora (eventos `null` vão para o fim, sob o título "Horário a confirmar") e desenha os cards. |
| `renderHours()` | Lê `agenda.funcionamento.areas` e `agenda.feriados`; escolhe dias úteis ou fim de semana/feriado pela data `cur`. |
| `renderVisitorInfo()` | Desenha os painéis de `restaurantes` e `experiencias`. Experiências buscam hora/local no evento de `dias` com mesma data e título (`titulo_evento`). |
| `drawStory(e)` | Desenha a imagem 1080×1920 do evento `e` num `<canvas>` e devolve um `Blob` PNG. |
| `wrapText(ctx, texto, largura)` | Quebra texto em linhas para o canvas. |
| `roundRect(ctx, x, y, w, h, r)` | Caminho de retângulo arredondado (usa `ctx.roundRect`). |

### Utilitários

| Nome | Uso |
|---|---|
| `esc(s)` | Escapa `& < > "` — **obrigatório** em todo texto de dados inserido via `innerHTML`. |
| `dateFor(day)` | `Date` ao meio-dia UTC da data (evita erro de fuso). |
| `dateLabel(day, opções)` | Data por extenso em pt-BR (ex.: "sexta-feira, 09 de outubro"). |
| `shortTime("19:15")` | `"19h15"`; `"08:00"` → `"8h"`. Formato usado nas artes oficiais. |
| `todayKey()` | Data de hoje `AAAA-MM-DD` no fuso da agenda. |
| `PIN`, `SHARE` | SVGs inline (pino de local, ícone de compartilhar). |
| `SITE` | Endereço exibido na imagem dos stories (o atual, se estiver no github.io). |
| `LEAF` | `Path2D` da folha do logotipo (mesmo desenho do SVG da capa). |

### Eventos de interface

| Evento | Efeito |
|---|---|
| Clique em `.day` | `cur = data`; `render()` |
| Digitar na busca | `render()` |
| `visibilitychange` (aba volta a ficar visível) | Se a data mudou desde o carregamento, atualiza `today`; se a pessoa estava no dia de hoje (ou antes da feira), muda `cur` para o novo dia. Se ela escolheu outro dia, mantém. |
| Clique em `.share` | Gera a imagem, abre `dialog#story` |
| Botões do diálogo | Compartilhar (`navigator.share`), Baixar (`<a download>`), Fechar / clique no fundo |

## Compartilhar nos stories (9:16)

Fluxo:

1. Cada card tem o botão **Stories** (`.share`, `data-i` = índice em `shown`).
2. O clique chama `drawStory(e)`: espera a Montserrat carregar (`document.fonts.load`), desenha no canvas 1080×1920 e gera PNG (~350 KB).
3. Abre a prévia (`dialog#story`). Se `navigator.canShare({files})` for verdadeiro (Android Chrome, iOS Safari), mostra **Compartilhar**; senão só **Baixar imagem** (computador).
4. **Compartilhar** abre a folha de compartilhamento do sistema → a pessoa escolhe **Instagram → Stories**. Cancelar (`AbortError`) é ignorado.

Por que existe a prévia em vez de compartilhar direto: o iOS só permite `navigator.share` logo após um toque; gerar a imagem é assíncrono e consome esse "toque". O botão da prévia dá um toque novo.

Limitações conhecidas:
- **Não existe API web para publicar direto nos stories** do Instagram; o caminho é a folha de compartilhamento (celular) ou baixar a imagem.
- Navegadores embutidos (abrir o link dentro do próprio Instagram/WhatsApp) podem não suportar compartilhar arquivos — nesse caso aparece só "Baixar imagem".

Layout da imagem (coordenadas em px de 1080×1920):
- Fundo: degradê `--navy-600 → --navy-700 → --navy-900` com textura de pontos.
- Topo (y≈250–430): "100 EXPOFEIRA PELOTAS" e "AGENDA" — abaixo da barra superior dos stories (~250px).
- Cartão branco (x 90–990, y a partir de ~500): pílula da data, horário grande (`shortTime` ou "A confirmar"), traço dourado, título (até 5 linhas; a fonte diminui de 76px até 44px para caber), etiqueta da categoria (fundo dourado), local com pino (até 2 linhas).
- Rodapé (y≥1560): folha + "05 – 12 OUT.", endereço da agenda em dourado, "PROGRAMAÇÃO SUJEITA A ALTERAÇÕES". Fica acima da barra de resposta dos stories (~250px finais).
- Cores lidas das variáveis CSS de `:root` em tempo de execução — mudar a paleta no CSS muda a imagem também.

## Dados — `eventos.json`

Formato completo descrito no [`README.md`](../README.md#estrutura-do-json). Resumo técnico:

```jsonc
{
  "feriados": ["2026-10-12"],                 // usam horário de fim de semana
  "funcionamento": {
    "fonte": "instagram",
    "campos": ["area", "seg_a_sex", "sab_dom_feriado"],
    "areas": [["Expositores", ["13:00","21:00"], ["10:00","21:00"]],
              ["Conferência Rural", ["08:00", null], null]]   // fecha null = não informado; período null = sem horário
  },
  "restaurantes": [{"nome": "...", "horarios": ["texto livre", "..."]}],
  "experiencias": [{"nome": "...", "data": "AAAA-MM-DD", "titulo_evento": "título exato em dias"}],
  "fonte_informacoes": "instagram",
  "timezone": "America/Sao_Paulo",
  "campos": ["hora", "titulo", "tipo", "local", "fonte", "observacao"],
  "fonte_padrao": "instagram",
  "fontes": {"instagram": {"descricao": "Instagram 100ª Expofeira de Pelotas", "url": "https://www.instagram.com/expofeirapelotas/"}},
  "tipos": {"chave": {"name": "...", "icon": "(não usado)", "color": "#...", "soft": "#..."}},
  "dias": {"AAAA-MM-DD": [["HH:mm" | null, "título", "tipo", "local" | null, "fonte?", "observação?"]]}   // fonte omitida = instagram
}
```

### Validação feita pelo HTML (em `loadAgenda`)

A página inteira mostra erro se **qualquer** item falhar:
- `dias`, `campos`, `tipos`, `fontes` existem e `campos` contém os 6 nomes;
- chave de dia no formato `AAAA-MM-DD` e data válida;
- cada evento é array; `titulo` string não vazia; `hora` é `null` ou `HH:mm` (00–23); `local` é `null` ou string;
- `tipo` existe em `tipos`; `fonte` (ou `fonte_padrao`) existe em `fontes`.

Validação equivalente em linha de comando (rodar antes de todo commit que mexa no JSON):

```bash
node -e "const j=require('./eventos.json');for(const d in j.dias)for(const e of j.dias[d]){if(!j.tipos[e[2]]||!j.fontes[e[4]||j.fonte_padrao]||(e[0]!==null&&!/^([01]\d|2[0-3]):[0-5]\d$/.test(e[0])))throw new Error(d+' '+e[1])}console.log('JSON ok')"
```

### Fontes cadastradas

Por decisão do dono, **a única fonte é `instagram`** ("Instagram 100ª Expofeira de Pelotas", padrão de todos os eventos). Não adicione outras fontes sem ele pedir.

### Observações

O 6º campo (`observacao`) **aparece no card para o público**. Use só para informação útil ao visitante (ex.: "Horário a confirmar", "Segunda apresentação do grupo"). Nada de notas internas ("conforme a transcrição", "encontrado na imprensa", divergência entre fontes) — isso vai para o `TAREFAS.md`. Para ter observação, o evento precisa do 5º campo preenchido (`"instagram"`).

### Receitas

- **Adicionar evento:** inserir o array no dia certo de `dias`, em ordem de horário (a página ordena, mas manter ordenado facilita revisão). Antes, procurar duplicata pelo título/horário no mesmo dia — o mesmo show pode vir de fontes diferentes com nomes ligeiramente diferentes.
- **Divergência entre divulgações:** escolher a mais oficial e anotar a outra no `TAREFAS.md`; horário desconhecido = `null` (com observação "Horário a confirmar").
- **Nova categoria:** adicionar em `tipos` (`name`, `color`, `soft`; `icon` não é exibido).
- **Mudar horário de funcionamento:** editar `funcionamento.areas`.
- **Novo feriado:** adicionar a data em `feriados`.

## Estilo e identidade visual

- Variáveis em `:root`: `--navy-900 #0D2144`, `--navy-700 #1F3567`, `--navy-600 #233A6E`, `--gold-400 #D6B370`, `--gold-300 #E4CB98`, `--muted #6B7690`; espaçamentos `--s1..--s6` (base 8px); raios `--radius-card 20px`, `--radius-sheet 40px`, `--radius-pill`.
- Fonte: Montserrat (300 a 900).
- Sem emojis na interface.
- Mobile primeiro: testar em 375px. Regras específicas em `@media (max-width:520px)`.
- Datas sempre no padrão brasileiro: botões `DD/MM`, textos por extenso em pt-BR.

## Armadilhas conhecidas

- **`file://` não funciona** — o `fetch` falha. Testar com `python -m http.server 8765` e abrir http://localhost:8765/expofeira.html.
- **Um erro no JSON derruba a página toda** (por desenho da validação). Sempre validar.
- **Alternância azul/dourado** usa `.card:nth-child(even)`; o `<li class="pending-heading">` ("Horário a confirmar") conta na contagem e pode quebrar a alternância depois dele.
- **Testes que simulam data** (sobrescrever `Date`) deixam a página num estado falso — recarregar ao terminar, para não confundir quem olhar a tela.
- **CRLF**: o repositório usa `core.autocrlf=true`; scripts que editam texto devem preservar `\r\n`.
- **Ordem de carregamento**: `drawStory` depende de `agenda` e `cur`; só é chamado depois do carregamento (os botões só existem após `render`).
