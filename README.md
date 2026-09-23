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
