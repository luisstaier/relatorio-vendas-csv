# Relatório de Vendas — CSV

Script Python puro (sem dependências) que lê um CSV de vendas e gera um relatório resumido em texto:

- Total de vendas por região
- Top 5 produtos por valor total
- Média diária de vendas

## Como usar

```bash
python relatorio_vendas.py vendas.csv
```

Para salvar o relatório em um arquivo:

```bash
python relatorio_vendas.py vendas.csv -o relatorio.txt
```

## Formato esperado do CSV

O CSV deve ter as colunas: `data, regiao, produto, valor`

```
data,regiao,produto,valor
01/01/2026,Sudeste,Notebook,4500.00
02/01/2026,Sul,Mouse,89.90
```

### Flexibilidade do parser

- **Separador:** vírgula (`,`) ou ponto-e-vírgula (`;`) — detectado automaticamente
- **Valor:** aceita `1234.56`, `1.234,56` e `R$ 1.234,56`
- **Data:** aceita `DD/MM/AAAA`, `DD-MM-AAAA`, `AAAA-MM-DD` e `AAAA/MM/DD`
- **Encoding:** UTF-8 com fallback para Latin-1
- **Linhas inválidas:** ignoradas e contabilizadas no relatório

## Exemplo de saída

```
============================================================
  RELATORIO DE VENDAS
============================================================
Periodo: 01/01/2026 a 28/06/2026
Registros validos: 1240  |  Registros ignorados: 3

--- Total por Regiao ---
Sudeste ................: R$ 45.300,00
Sul ....................: R$ 22.150,50
Nordeste ...............: R$ 18.720,30
Centro-Oeste ...........: R$ 12.450,00
Norte ..................: R$ 8.900,75

--- Top 5 Produtos (por valor total) ---
 1. Notebook Ultra ..........: R$ 21.500,00
 2. Monitor 4K ..............: R$ 12.300,00
 3. Teclado Mecanico .......: R$ 5.670,00
 4. Mouse Gamer .............: R$ 4.320,00
 5. Webcam HD ...............: R$ 3.150,00

--- Media Diaria de Vendas ---
R$ 3.420,75  (em 28 dias de venda)
============================================================
```

## Premissas

- **Top 5:** ordenado por soma total do valor de cada produto
- **Média diária:** total geral dividido pela quantidade de **dias distintos** presentes no CSV
- **Colunas:** `data`, `regiao`, `produto`, `valor` (case-insensitive, aceita espaços)
- **Encoding:** tenta UTF-8 primeiro; se falhar, usa Latin-1
