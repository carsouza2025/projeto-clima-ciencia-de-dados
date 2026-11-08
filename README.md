# Coleta, Processamento e Análise de Dados Meteorológicos com Python

**Autor:** Carlos Souza do Nascimento  
**Disciplina:** Introdução à Ciência de Dados  
**Professor:** Danilo Pereira  
**Ambiente:** Google Colab (Python 3)  
**Período analisado:** 1º a 31 de outubro de 2026

## Sobre o projeto

Este projeto apresenta um exercício prático de coleta, processamento e análise exploratória de dados meteorológicos de **dez cidades do Nordeste brasileiro**, utilizando Python, a biblioteca Pandas e a API pública Open-Meteo. A escolha do tema se relaciona à **Biologia Vegetal**, pois temperatura e disponibilidade de água são fatores ambientais relevantes para a germinação, o crescimento e o desenvolvimento das plantas.

O estudo analisa dados meteorológicos, **não** respostas biológicas das plantas. A associação entre essas informações poderá ser investigada em projetos posteriores, com dados experimentais de vegetação e solo.

**Pergunta orientadora:** como as condições meteorológicas observadas em outubro de 2026 diferiram entre as dez cidades selecionadas?

## Fonte e método

- **Fonte:** [Open-Meteo — Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api).
- **Endpoint:** `https://archive-api.open-meteo.com/v1/archive`.
- **Modelo:** ERA5 (reanálise meteorológica).
- **Cidades:** Aracaju, Maceió, Recife, João Pessoa, Natal, Fortaleza, Teresina, São Luís, Salvador e Petrolina.
- **Variáveis:** temperatura média, máxima e mínima do ar (°C); precipitação acumulada (mm); evapotranspiração de referência, ET₀ (mm).
- **Método:** requisição HTTP GET → resposta JSON → DataFrame com Pandas → limpeza e validação → agregação mensal → análise e gráficos → exportação CSV.

A coleta diária abrange **31 dias × 10 cidades = 310 observações**. Na etapa de agregação, essas observações são resumidas em **10 registros finais**, um por cidade, com **9 colunas**.

## Arquivos do projeto

| Arquivo | Conteúdo |
|---|---|
| [`Projeto_Clima_Coleta_Processamento_Analise_Carlos_Aula_4_2.ipynb`](Projeto_Clima_Coleta_Processamento_Analise_Carlos_Aula_4_2.ipynb) | Notebook com código, explicações, testes, resultados e gráficos. |
| [`Projeto_Clima_Dataset_Final.csv`](Projeto_Clima_Dataset_Final.csv) | Tabela final com 10 cidades e 9 variáveis/identificadores. |
| [`Projeto_Clima_Documentacao_Final.md`](Projeto_Clima_Documentacao_Final.md) | Metodologia detalhada, dicionário de dados, resultados, limitações e referências. |
| `README.md` | Apresentação do projeto e instruções de uso. |

Os gráficos `grafico_precipitacao_outubro_2026.png` e `grafico_precipitacao_et0_outubro_2026.png` podem ser incluídos no repositório como arquivos adicionais; também são gerados pelo notebook.

## Como executar no Google Colab

1. Abra o [notebook no Google Colab](https://colab.research.google.com/drive/1CnIoqDJaReQGkCdjOW8CINcD2ACcMM4J?usp=sharing) ou carregue o arquivo `.ipynb` deste repositório no [Google Colab](https://colab.research.google.com/).
2. Confira as bibliotecas importadas no início do notebook: `requests`, `pandas` e `matplotlib` (além de módulos da biblioteca padrão do Python).
3. Na célula **Passo C**, mantenha `modo_coleta = "api"` para fazer uma nova consulta à Open-Meteo com conexão à internet. Para examinar os dados de referência sem coleta remota, utilize `modo_coleta = "snapshot"`.
4. Escolha **Ambiente de execução → Executar tudo** e acompanhe as verificações, a tabela e os gráficos.
5. Faça o download do CSV gerado no ambiente do Colab, caso deseje reproduzir a exportação do notebook.

**Nota de reprodutibilidade:** quando a API falha, o notebook utiliza um *snapshot* de **resultados mensais previamente agregados**, registrado em 08/11/2026. Nesse caso, a execução **não** representa uma nova coleta das 310 observações diárias; a origem utilizada é indicada nas saídas do programa. O arquivo CSV disponibilizado neste repositório reflete os resultados registrados no notebook enviado.

## Principais resultados

No conjunto de outubro de 2026 analisado:

- **Teresina** apresentou a maior temperatura média mensal: **30,96 °C**.
- **Salvador** apresentou a maior precipitação acumulada: **38,20 mm**.
- **Petrolina** apresentou a maior ET₀ acumulada: **246,45 mm**.

Os resultados permitem comparar as cidades no período selecionado, mas **não** caracterizam tendências climáticas de longo prazo nem demonstram efeitos sobre a vegetação.

## Limitações e possibilidades futuras

A análise considera um único mês, dez cidades escolhidas para fins didáticos e valores de reanálise meteorológica (não necessariamente dados de estações locais). Não foram coletadas medições de germinação, crescimento vegetal, produtividade ou água no solo.

Como continuidade, podem ser incorporados vários anos de registros e dados experimentais vegetais, informações de solo e índices de vegetação, permitindo investigar a relação entre condições meteorológicas e respostas das plantas.

## Referências

- [Open-Meteo — Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api).
- PEREIRA, Danilo. *Aula 4.1: Coleta de Dados — Projeto Prático Completo* e *Aula 4.2: Processamento e Coleta de Dados*. Material didático da disciplina.
