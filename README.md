# Dashboard de Prestadores & Faturamento — CompesaPrev

Painel interativo (HTML + Chart.js + SheetJS, bibliotecas locais em `vendor/`) que cruza o cadastro de prestadores com o envio de PEG, a situação do faturamento e as notas fiscais.

## Estrutura
```
index.html        painel
logo.png          logo CompesaPrev
vendor/           Chart.js e SheetJS (locais, sem CDN)
dados/            as 4 planilhas exportadas dos relatórios SQL
```

## Como atualizar os dados
Exporte os 4 relatórios do Oracle e salve em `dados/` com estes nomes:

| Arquivo em dados/      | Relatório SQL |
|------------------------|---------------|
| `prestadores.xlsx`     | Todos os prestadores (SAM_PRESTADOR) |
| `peg_situacao.xlsx`    | Prestadores que enviaram PEG, com a situação de cada PEG |
| `peg_quantidade.xlsx`  | Prestadores que enviaram PEG e quantos (QTD_PEG / QTD_GUIA) |
| `notas_fiscais.xlsx`   | Prestadores que enviaram nota fiscal |

Depois: `git add . && git commit -m "atualiza dados" && git push`.

Sem renomear: o botão **📁 Carregar planilhas** aceita os 4 arquivos de uma vez, com qualquer nome — o painel reconhece cada relatório pelas colunas.

Para trocar o período, basta mudar as datas no `WHERE` das consultas; as competências do painel se ajustam sozinhas.

## Data de atualização dos dados
A barra de status (abaixo do cabeçalho) mostra a data do **último commit que alterou a pasta `dados/`** (API pública do GitHub), então reflete quando os dados foram realmente publicados.
Se a API não responder (repositório privado, limite de 60 consultas/hora por rede, bloqueio do navegador), usa a data de publicação do GitHub Pages.
No upload manual, usa a data de modificação das planilhas selecionadas. Passando de 35 dias aparece um aviso ⚠ (ajustável em `DIAS_ALERTA_DESATUALIZADO`).

## Filtro padrão
O painel abre filtrado em todas as categorias de prestador **menos reembolso (PF e PJ) e descredenciados**.
"Limpar filtros" mostra tudo; "↺ Filtro padrão" volta ao recorte inicial. Para mudar, edite `CATEGORIAS_FORA_DO_PADRAO` no `index.html`.

## Liberação para nota fiscal
A aba usa as colunas TEM_PAGAMENTO e TEM_NOTA da consulta de PEG. Etapas de cada PEG:
Em análise (sem pagamento gerado) → Liberado, aguardando nota (pagamento gerado, sem nota) → Nota recebida → Faturado.

## Consultas SQL
As consultas ficam fixas no `index.html` (constante `SQL_FIXO`) e aparecem na aba "Consultas SQL".
Não é preciso exportar a aba SQL. Se alterar uma consulta no Oracle, atualize o texto em `SQL_FIXO`.
