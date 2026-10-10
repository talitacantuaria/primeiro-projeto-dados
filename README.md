# Dashboard de análise de vendas

Dashboard interativo para a atividade de Fundamentos de IA e Análise de Dados. Usa HTML, CSS e JavaScript puro, sem backend, banco de dados ou compilação.

## Abrir e usar

1. Abra `public/index.html` no navegador. A demonstração aparece imediatamente e funciona também via `file://`: os dados estão incorporados em `public/script.js` e correspondem a `public/dados-exemplo.csv`.
2. Use os filtros de período, categoria e dimensão disponível. A busca da tabela também atualiza cartões e gráficos. Clique nos títulos das colunas para ordenar; os controles no rodapé navegam pelos registros.
3. Clique em **Importar CSV** para carregar outra base. O navegador lê o arquivo localmente; ele não é enviado a um servidor. São reconhecidos cabeçalhos como `id_venda`, `data`, `produto`, `categoria`, `regiao` ou `canal`, `quantidade` e `preco_unitario`. Os campos principais também aceitam nomes equivalentes em inglês. Datas ISO (`AAAA-MM-DD`) e `DD/MM/AAAA` são aceitas. Quantidades e preços devem ser números não negativos.
4. **Exportar dados** baixa as linhas que correspondem aos filtros e à busca atuais, incluindo a coluna calculada `faturamento_linha`.

Chart.js 4.4.8 e Papa Parse 5.4.1 são carregados por CDN. É necessária conexão com a internet para carregar as bibliotecas e usar o dashboard.

## Dados e adaptação

O projeto já continha `vendas_hamburgueria.csv`, com 1.367 linhas de venda e as colunas `id_venda`, `data`, `dia_semana`, `periodo`, `produto`, `categoria`, `quantidade`, `preco_unitario`, `forma_pagamento`, `canal`, `tempo_espera_min` e `avaliacao`. O arquivo original foi preservado. A cópia em `public/dados-exemplo.csv` e a amostra incorporada ao JavaScript têm o mesmo conteúdo.

Como já havia uma base, ela foi usada em vez de gerar os 200 registros fictícios previstos para o caso sem dados. A base cobre julho a setembro de 2026 e não contém `regiao`. O filtro e o gráfico de comparação usam `canal` (Delivery e Balcão), adaptação que também é identificada na interface. Com um CSV que tenha `regiao`, a interface usa essa coluna; se não houver região nem canal, a comparação fica indisponível. A tabela e a exportação preservam as outras colunas da base.

Linhas sem ID, com data inválida, quantidade ausente/inválida/negativa ou preço ausente/inválido/negativo são descartadas, com contagem e motivos exibidos na tela. Quantidades e preços iguais a zero são aceitos. IDs repetidos não removem linhas: cada registro é mantido, mas a quantidade de vendas conta IDs únicos.

## Indicadores

- **Faturamento por linha:** `quantidade × preco_unitario`.
- **Faturamento total:** soma do faturamento das linhas após aplicar período, categoria, dimensão e busca.
- **Quantidade de vendas:** contagem de IDs de venda distintos no recorte.
- **Unidades vendidas:** soma das quantidades no recorte.
- **Ticket médio:** faturamento total ÷ quantidade de vendas; sem vendas, o indicador mostra R$ 0,00.

O gráfico temporal agrupa pela coluna `data`; a linha pontilhada sobre o faturamento mensal mostra uma tendência linear ajustada aos valores mensais do recorte. Ela resume a direção média no período e não é uma previsão. Os gráficos de barras comparam faturamento por `categoria` e pela dimensão disponível (`regiao` ou `canal`).

## Publicar no Render pelo GitHub

Envie o projeto para um repositório GitHub e crie no Render um serviço **Static Site**, conectado ao repositório. Configure:

- **Tipo de serviço:** Static Site
- **Branch:** a branch que contém o projeto
- **Root Directory:** vazio
- **Build Command:** `echo "Site pronto"`
- **Publish Directory:** `public`
