# Agenda da 100ª Expofeira de Pelotas

Agenda interativa para consultar a programação da 100ª Expofeira de Pelotas, realizada de **5 a 12 de outubro de 2026**, no Parque de Exposições Ildefonso Simões Lopes, na Associação Rural de Pelotas.

**Endereço:** Avenida Fernando Osório, 1.754 — Pelotas/RS.

O projeto organiza os eventos por data, horário e categoria. A proposta visual combina a identidade da Expofeira com uma navegação inspirada em aplicativos de calendário.

> Projeto independente, sem vínculo oficial com a organização da Expofeira.

## Acesso

- [Agenda publicada](https://ssjoaootavio.github.io/100expofeira/expofeira.html)
- [Repositório](https://github.com/ssjoaootavio/100expofeira)

O endereço da agenda depende do nome do HTML publicado. Se o arquivo principal for renomeado para `index.html`, o acesso passa a ser:

https://ssjoaootavio.github.io/100expofeira/

## Funcionalidades

A versão inicial da agenda inclui:

- Visualizações por semana, dia e lista.
- Pesquisa por nome, assunto ou local.
- Filtros por tipo de evento.
- Identificação das categorias por cor, ícone e texto.
- Detalhes dos eventos com horário, local e fonte.
- Layout adaptado para computadores e celulares.
- Indicação de horários ainda não confirmados.
- Horários de funcionamento do dia selecionado (expositores, Fazendinha, praça de alimentação e Conferência Rural), distinguindo dias úteis de fins de semana e feriado.

## Status do projeto

O projeto está em evolução:

- A agenda carrega os eventos diretamente de `eventos.json`.
- O arquivo `eventos.json` centraliza a programação, as categorias e as fontes.
- A integração inclui estados de carregamento, erro e busca sem resultados, além de fontes e observações.

Alterações no JSON são exibidas quando a página é carregada novamente.

## Tecnologias

- HTML5
- CSS3
- JavaScript
- JSON
- GitHub Pages

A versão inicial não exige framework, banco de dados ou etapa de compilação.

## Arquivos

| Arquivo | Finalidade |
|---|---|
| `expofeira.html` | Versão inicial da agenda |
| `agenda-expofeira.html` | HTML de referência com a identidade visual do evento |
| `eventos.json` | Base de programação para integração com a agenda |
| `README.md` | Documentação do projeto |
| `AGENTS.md` | Instruções compartilhadas para agentes de IA (Claude, Codex, Antigravity) |
| `TAREFAS.md` | Quadro de tarefas e passagem de bastão entre agentes |

Esta tabela descreve os arquivos previstos para o projeto. A presença de cada arquivo depende de seu envio ao repositório.

## Programação

A base preparada reúne:

- **109 registros** da transcrição dos stories fornecida para o projeto.
- **19 registros complementares** obtidos em outras fontes e nas imagens fornecidas.
- **128 registros no total**, incluindo ocorrências diárias de atividades recorrentes.

A programação completa ainda não foi verificada. A quantidade de registros pode mudar após revisões.

### Categorias

- Palestras e seminários
- Cultura
- Atividades infantis
- Exposições
- Leilões
- Cerimônias
- Gastronomia
- Reuniões
- Julgamentos
- Manejo de animais
- Provas e demonstrações

As categorias são uma organização editorial do projeto. Elas não necessariamente reproduzem classificações oficiais do evento.

## Estrutura do JSON

O arquivo `eventos.json` contém:

| Campo | Descrição |
|---|---|
| `titulo` | Nome do evento |
| `ano` | Ano da edição |
| `timezone` | Fuso horário da programação |
| `atualizado_em` | Data da última revisão dos dados |
| `programacao_completa_verificada` | Indica se a programação integral foi conferida |
| `campos` | Define a ordem dos valores de cada registro |
| `fonte_padrao` | Fonte utilizada quando o registro não informa outra |
| `fontes` | Descrições e links das fontes |
| `feriados` | Datas tratadas como feriado (horário de fim de semana) |
| `funcionamento` | Horários por área: `areas` = `[area, seg_a_sex, sab_dom_feriado]`, cada período `["abre", "fecha"]` |
| `restaurantes` | Restaurantes e seus horários (texto) |
| `experiencias` | Experiências que apontam para eventos de `dias` por data e título |
| `fonte_informacoes` | Fonte de restaurantes e experiências |
| `tipos` | Categorias, ícones e cores |
| `dias` | Eventos agrupados por data |

### Formato de um registro

Cada evento é representado por um array nesta ordem:

    ["hora", "titulo", "tipo", "local", "fonte", "observacao"]

Exemplo:

    [
      "14:00",
      "Territórios, Conexões e Inovação",
      "palestra",
      "Arena da Conferência",
      "stories",
      "Conteúdo detalhado não informado."
    ]

Regras:

- Datas usam o formato `AAAA-MM-DD`.
- Horários usam o formato `HH:mm`, de 24 horas.
- O fuso horário é `America/Sao_Paulo`.
- Horário ou local desconhecido deve ser `null`.
- `fonte` e `observacao` podem ser omitidos ao final do array.
- Quando `fonte` é omitida, aplica-se `fonte_padrao`.
- O campo `tipo` deve corresponder a uma chave cadastrada em `tipos`.
- O campo `fonte`, quando informado, deve corresponder a uma chave de `fontes`.
- Não inserir comentários ou vírgulas após o último elemento: o arquivo deve ser JSON válido.

## Atualizar a programação

1. Abra `eventos.json`.
2. Localize a data em `dias`.
3. Adicione ou edite o registro.
4. Informe a fonte e eventuais dúvidas.
5. Atualize `atualizado_em`.
6. Valide a sintaxe do JSON.
7. Faça o commit na branch usada pelo GitHub Pages.
8. Aguarde a publicação e confira a alteração no site.

O HTML carrega o arquivo por caminho relativo:

    fetch("./eventos.json")

O HTML e o JSON precisam estar na mesma pasta para esse caminho funcionar.

## Executar localmente

Para a versão que utiliza `fetch`, sirva os arquivos por HTTP. Abrir o HTML diretamente com duplo clique pode impedir o carregamento do JSON.

Uma opção é usar a extensão **Live Server** no Visual Studio Code:

1. Abra a pasta do projeto no VS Code.
2. Instale a extensão Live Server, caso ainda não esteja instalada.
3. Clique com o botão direito no HTML.
4. Selecione **Open with Live Server**.

## Publicar no GitHub Pages

1. Envie os arquivos para o repositório.
2. Acesse **Settings → Pages**.
3. Em **Source**, selecione **Deploy from a branch**.
4. Escolha a branch `main` e a pasta `/ (root)`.
5. Clique em **Save**.
6. Acompanhe a execução em **Actions**.

Para disponibilizar a agenda na raiz do site, use `index.html` como arquivo principal.

## Critérios de qualidade dos dados

- Preservar atividades simultâneas.
- Não eliminar sessões repetidas em horários diferentes.
- Não inventar horários de término.
- Mostrar “Local não informado” quando o valor for `null`.
- Exibir eventos sem horário em uma seção identificada.
- Manter a origem dos registros complementares.
- Registrar divergências entre fontes.
- Não apresentar temas inferidos como conteúdo confirmado de uma palestra.
- Revisar alterações de nomes, locais e horários antes de publicá-las.

### Divergência conhecida

O leilão **Só Angus**, em 11 de outubro, aparece às **18h** no Jornal Tradição e às **19h** na agenda da TV El Campo.

Até a confirmação, seu horário deve permanecer como `null`, acompanhado da observação sobre a divergência.

## Fontes

- Transcrição dos stories fornecida para o projeto.
- [Instagram da Expofeira Pelotas](https://www.instagram.com/expofeirapelotas/)
- [Jornal Tradição Regional](https://www.jornaltradicao.com.br/pelotas/rural/expofeira-pelotas-chega-a-sua-100a-edicao-com-muitas-atracoes/)
- [Agenda da TV El Campo](https://tvelcampo.com.br/agenda)
- [UCPel — Crédito Rural na Expofeira](https://ucpel.edu.br/index.php/noticias/ucpel-promove-conferencia-rural-na-100a-expofeira)

A programação pode sofrer alterações. Consulte os canais oficiais antes de se deslocar.

## Próximas melhorias

- [x] Integrar o HTML ao `eventos.json`.
- [ ] Aplicar e revisar a identidade visual do evento.
- [x] Exibir estados de carregamento, erro e busca sem resultados.
- [ ] Verificar a programação completa.
- [ ] Permitir salvar eventos em uma agenda pessoal.
- [ ] Adicionar exportação para calendário.
- [ ] Melhorar a navegação por teclado e testar acessibilidade.

## Responsável

**João Santos**

[GitHub — ssjoaootavio](https://github.com/ssjoaootavio)

## Informações para visita

O bloco de funcionamento é montado a partir de `funcionamento` e `feriados`, de acordo com o dia selecionado. As seções de restaurantes e experiências são carregadas dos campos `restaurantes` e `experiencias` do JSON, com a fonte em `fonte_informacoes`. As experiências referenciam eventos existentes por data e título; horários e locais vêm da programação, evitando duplicação.

As imagens fornecidas em 05/10/2026 acrescentam o Workshop de Carnes Angus (06/10, 19h, local não informado). O jantar e o concurso de assado já constavam na agenda. A abertura da Fazendinha nos dias úteis foi atualizada para 13h30 conforme a imagem, preservando em observação a divergência com a fonte anterior (14h).
