# 📊 Desafio Power BI — Relatório Financeiro com Foco na Experiência do Usuário (Versão Expandida)

## 📑 Índice
- Contexto
- Objetivos
- Fontes
- Estrutura do Relatório
- Decisões de Experiência do Usuário
- Paleta de Cores
- Medidas e Colunas Criadas
- Principais Resultados
- Arquivos
- Autor

# Contexto:
- Este projeto é a versão expandida da entrega do desafio "Atualizando Relatório Financeiro com Foco na Experiência do Usuário", da trilha de Analista de Dados da [DIO](https://www.dio.me/). A partir de um template e da tabela `financials` (base Financial Sample), o relatório foi reconstruído e ampliado para **6 páginas**, com atenção à navegação, à hierarquia visual, ao contraste e à separação das análises por tema.
- A versão básica, com 3 páginas, foi entregue separadamente. Aqui as páginas de análise avançada (TOPN & Outliers, Data Analytics e Categorias) foram reorganizadas e ajustadas ao mesmo padrão visual.

# Objetivos:
- Aplicar princípios de experiência do usuário (UX) em um relatório de Power BI;
- Criar navegação clara entre as páginas, com botões, menu e retorno à Home;
- Organizar a leitura com hierarquia visual, agrupamento por tema e cores com função definida;
- Alternar visões do mesmo dado (barras/pizza, treemap/mapa, semestres/meses) sem poluir a página;
- Explorar recursos analíticos: Top N, outliers, linha de tendência, clusters, eixo de reprodução e agrupamentos personalizados;
- Evitar gráficos repetidos entre páginas, dando a cada visual uma pergunta própria para responder.

# Fontes:
- Tabela `financials` (Financial Sample), fornecida no desafio, com vendas entre 01/09/2013 e 01/12/2014;
- Template e páginas de referência fornecidos pela DIO;
- Conteúdo de referência: módulos do curso de Power BI Analyst da DIO.

# Estrutura do Relatório

**Página 1 — Home**
- Capa "Report Financeiro" com o botão **Explorar análise**.

**Página 2 — Relatório de Vendas**
- Segmentador de data e cartões (Total de Vendas, Unidades Vendidas, Descontos, Lucro e COGS);
- Vendas por mês, por segmento (barras ou rosca), por produto e por país (treemap ou mapa).

**Página 3 — Detalhes de Vendas**
- Vendas por Semestre, com botões para alternar entre **Semestres** e **Meses**;
- Matriz de Trimestre por Ano, com totais;
- Lucro e Margem por Produto (colunas + linha);
- Vendidos por Produto (barras).

**Página 4 — TOPN & Outliers**
- Cartões: Máximo de Unidades Vendidas e Segmento do Máximo;
- Top 3 Produtos, Top 3 Países por Máximo Vendido e Top 5 Meses (Vendas x Lucro);
- Vendas e Top 3 Produtos por País.

**Página 5 — Data Analytics**
- Histograma de Unidades Vendidas (compartimentos de unidades);
- Clusters: Unidades x Vendas (3 grupos automáticos);
- Unidades e Vendas por Mês, com linha de tendência;
- Vendas, Unidades e Lucro por Produto e Mês (dispersão com eixo de reprodução).

**Página 6 — Categorias**
- Vendas por Continente e Ano (Sankey);
- Vendas por País e Continente (grupo personalizado de continentes);
- Vendas por segmento (Destaque x Outro), com agrupamento personalizado.

# Decisões de Experiência do Usuário
- **Navegação:** botão de entrada na capa, menu em todas as páginas e atalho para a Home;
- **Hierarquia:** título e indicadores principais no topo, detalhes abaixo, seguindo a leitura em Z;
- **Organização por tema:** resumo (Relatório), detalhamento (Detalhes), destaques (TOPN & Outliers), análise estatística (Data Analytics) e agrupamentos (Categorias);
- **Sem repetição:** gráficos que apareciam em mais de uma página foram removidos ou substituídos por visuais que respondem a perguntas diferentes;
- **Alternância de visões:** botões com indicadores (bookmarks) trocam um visual por outro no mesmo espaço;
- **Contraste:** textos e gráficos claros sobre o fundo roxo-escuro, evitando tons escuros sobre fundo escuro;
- **Cor com função:** uma cor para o dado, outra para o destaque e outra só para ações;
- **Rótulos de dados** nos gráficos, para ler o valor sem passar o mouse.

# Paleta de Cores

| Função | Cor | Uso |
|---|---|---|
| Dado padrão | `#EBDDF7` (lilás claro) | barras e séries comuns |
| Destaque | `#FFC857` (âmbar) | maior valor, segunda série, picos |
| Degradê | `#F3E2FF` → `#FFC857` | treemap, histograma e produtos por valor |
| Ação | `#B8527A` (rosa) | botões, ícone Home e estado selecionado |
| Texto | `#FFFFFF` / `#2A1050` | texto sobre fundo escuro / sobre cores claras |

# Medidas e Colunas Criadas

```DAX
Semestre = IF(MONTH(financials[Date]) <= 6, "1º Semestre", "2º Semestre")
```

```DAX
Margem % = DIVIDE(SUM(financials[Profit]), SUM(financials[Sales]))
```

```DAX
TOP3 PRODUCT =
CALCULATE(
    SUM(financials[Sales]),
    TOPN(3, VALUES(financials[Product]), CALCULATE(SUM(financials[Sales])))
)
```

```DAX
Segmento do Max =
CALCULATE(
    SELECTEDVALUE(financials[Segment]),
    FILTER(ALL(financials), financials[Units Sold] = MAX(financials[Units Sold]))
)
```

Também foram criados compartimentos (bins) sobre `Units Sold` para o histograma, grupos personalizados de **Continentes** e de **Segmentos** (Destaque x Outro), e a coluna `Sales` foi renomeada para remover um espaço no início do nome, que impedia o uso nas medidas.

# Principais Resultados
- Total de vendas no período: **118,73 Mi**, com lucro de **16,89 Mi** e COGS de **101,83 Mi**;
- **Paseo** é o produto mais vendido (33 Mi), com o **Government** como segmento de maior receita (53 Mi);
- **Estados Unidos** (25,03 Mi), **Canadá** (24,89 Mi) e **França** (24,35 Mi) lideram as vendas por país;
- O máximo de unidades vendidas em uma venda foi **4,49 Mil**, no segmento Government;
- O **4º trimestre** é o mais forte (51,69 Mi somando os dois anos), e **outubro** lidera entre os 5 meses de maior venda (22 Mi);
- A linha de tendência mostra relação crescente entre unidades vendidas e vendas por mês;
- Os dados começam em setembro de 2013, então 2013 aparece só no 2º semestre.

# Arquivos

| Arquivo | Descrição |
|---|---|
| `Sales_Report_Expandido_-_UX.pbix` | Projeto completo do Power BI Desktop, com as 6 páginas |
| `Sales_Report_Expandido_-_UX.pdf` | Exportação em PDF das 6 páginas do relatório |
| `Sales_Report_Expandido_-_UX.pptx` | Apresentação com as 6 páginas (um slide por página) |

# Autor
- Kelwin Paschoal
