# Coleta, Processamento e Análise de Dados Meteorológicos com Python

**Autor:** Carlos Souza do Nascimento  
**Disciplina:** Introdução à Ciência de Dados  
**Professor:** Danilo Pereira  
**Ambiente de desenvolvimento:** Google Colab (Python 3)  
**Data indicada no cabeçalho do notebook:** 07/11/2026  
**Data registrada para o snapshot de referência:** 08/11/2026  
**Período analisado:** 01/10/2026 a 31/10/2026

## 1. Objetivo e justificativa

O projeto teve como objetivo coletar, organizar, limpar e analisar dados meteorológicos de dez cidades do Nordeste brasileiro, utilizando uma API pública e ferramentas de Ciência de Dados. Foram consideradas temperaturas, precipitação e evapotranspiração de referência (ET₀).

**Pergunta orientadora:** como as condições meteorológicas observadas em outubro de 2026 diferiram entre as dez cidades selecionadas?

**Justificativa da escolha:** o tema Clima consta dos assuntos sugeridos na aula e apresenta relação com a Biologia Vegetal, pois temperatura, chuva e disponibilidade hídrica são relevantes para o estudo da germinação, do crescimento e do desenvolvimento das plantas. Este projeto analisa somente variáveis meteorológicas; a integração com dados experimentais vegetais é uma possibilidade para projetos futuros.

## 2. Fonte de dados

| Fonte | Endereço | Método | Conteúdo |
|---|---|---|---|
| Open-Meteo — Historical Weather API | https://archive-api.open-meteo.com/v1/archive | API HTTP GET; resposta JSON | Temperaturas diárias, precipitação e ET₀ |
| Documentação técnica | https://open-meteo.com/en/docs/historical-weather-api | Consulta documental | Descrição de variáveis, unidades e parâmetros |

**Modelo de dados utilizado:** ERA5 (`models=era5`).  
**Fuso horário da consulta:** `America/Maceio`.  
**URL de consulta registrada no notebook:**

`https://archive-api.open-meteo.com/v1/archive?latitude=-10.9162%2C-9.6499%2C-8.0476%2C-7.1153%2C-5.7945%2C-3.7319%2C-5.0919%2C-2.5307%2C-12.9777%2C-9.3891&longitude=-37.0731%2C-35.7089%2C-34.877%2C-34.861%2C-35.211%2C-38.5267%2C-42.8034%2C-44.3068%2C-38.5016%2C-40.5031&start_date=2026-10-01&end_date=2026-10-31&daily=temperature_2m_mean%2Ctemperature_2m_max%2Ctemperature_2m_min%2Cprecipitation_sum%2Cet0_fao_evapotranspiration&timezone=America%2FMaceio&models=era5`

**Proveniência da execução registrada:** a saída salva da célula de coleta no notebook indica *“Validação aprovada: 10 cidades × 31 dias = 310 observações diárias”* e *“Coleta feita diretamente pela API”*. O notebook também inclui um snapshot agregado de referência, previsto como alternativa quando a API estiver indisponível. Os arquivos desta entrega foram gerados a partir dos resultados salvos no notebook e do CSV correspondente, sem executar uma nova requisição à API nesta etapa de documentação.

## 3. Metodologia

### 3.1 Planejamento e coleta

Foram selecionadas dez cidades com coordenadas geográficas: Aracaju, Maceió, Recife, João Pessoa, Natal, Fortaleza, Teresina, São Luís, Salvador e Petrolina. A consulta definiu o intervalo de 1º a 31 de outubro de 2026, com cinco variáveis diárias. A API retornou dados em JSON, que foram convertidos em uma tabela com registros por cidade e dia.

### 3.2 Limpeza dos dados

O notebook emprega `to_datetime()` para as datas, `to_numeric()` para variáveis numéricas e `drop_duplicates()` para evitar duplicações por cidade e data. Os valores ausentes são examinados antes da agregação. Para precipitação, um valor ausente não é convertido automaticamente em zero, pois ausência de dado não equivale à ausência de chuva.

Na execução salva, foram registrados **zero valores ausentes**, **zero duplicatas removidas** e **310 linhas diárias válidas**.

### 3.3 Processamento e agregação

Os 310 registros correspondem a **10 cidades × 31 dias**. Para cada cidade, as observações diárias foram agregadas em um único resumo mensal:

- `temp_media_c`: média aritmética das temperaturas médias diárias;
- `temp_max_c`: maior temperatura máxima diária do período;
- `temp_min_c`: menor temperatura mínima diária do período;
- `precip_total_mm`: soma da precipitação diária em milímetros;
- `et0_total_mm`: soma da evapotranspiração de referência diária em milímetros.

O resultado foi um dataset final de **10 linhas (uma por cidade) e 9 colunas**.

### 3.4 Auditoria de integridade

Foram aplicados testes para verificar: presença de dez cidades distintas; 31 dias válidos por cidade; ausência de valores nulos; precipitação e ET₀ não negativas; e coerência entre temperatura mínima, média e máxima. O notebook registra a mensagem *“Todos os testes de integridade foram aprovados.”*

## 4. Dataset final

**Arquivo:** `Projeto_Clima_Dataset_Final.csv`  
**Formato:** CSV, separado por vírgulas, codificação UTF-8 com BOM  
**Dimensão:** 10 registros × 9 colunas  
**Tamanho do arquivo entregue:** 687 bytes  
**Unidade de observação:** uma cidade, resumida para outubro de 2026.

### Dicionário de dados

| Coluna | Tipo | Descrição |
|---|---|---|
| `cidade` | texto | Nome do município |
| `latitude_consulta` | decimal | Latitude enviada à API |
| `longitude_consulta` | decimal | Longitude enviada à API |
| `dias_validos` | inteiro | Número de dias válidos agregados |
| `temp_media_c` | decimal | Média das temperaturas médias diárias (°C) |
| `temp_max_c` | decimal | Maior máxima diária (°C) |
| `temp_min_c` | decimal | Menor mínima diária (°C) |
| `precip_total_mm` | decimal | Precipitação mensal acumulada (mm) |
| `et0_total_mm` | decimal | Evapotranspiração de referência acumulada no mês (mm) |

## 5. Análise exploratória

As estatísticas foram calculadas a partir do dataset final. Para as dez cidades, a média das temperaturas médias mensais foi **27,70 °C**, a precipitação mensal média foi **18,10 mm** e o valor médio dos totais mensais de ET₀ foi **179,37 mm**. Essas médias descrevem a amostra selecionada e não são estimativas representativas de todo o Nordeste.

| Cidade | Temperatura média (°C) | Precipitação (mm) | ET₀ (mm) |
|---|---:|---:|---:|
| Aracaju | 26,84 | 11,90 | 173,05 |
| Maceió | 26,50 | 30,10 | 158,95 |
| Recife | 27,24 | 25,10 | 162,97 |
| João Pessoa | 26,80 | 22,10 | 163,07 |
| Natal | 26,58 | 32,60 | 165,20 |
| Fortaleza | 27,45 | 8,30 | 177,82 |
| Teresina | 30,96 | 0,90 | 209,93 |
| São Luís | 27,98 | 4,60 | 171,79 |
| Salvador | 26,48 | 38,20 | 164,45 |
| Petrolina | 30,12 | 7,20 | 246,45 |

### Visualizações

- `grafico_precipitacao_outubro_2026.png`: gráfico de barras comparando a precipitação acumulada nas cidades;
- `grafico_precipitacao_et0_outubro_2026.png`: gráfico de dispersão entre precipitação e ET₀, com identificação das cidades.

## 6. Principais resultados

1. **Teresina** apresentou a maior temperatura média mensal, de **30,96 °C**.
2. **Salvador** registrou a maior precipitação acumulada no período, de **38,20 mm**.
3. **Petrolina** apresentou a maior ET₀ mensal acumulada, de **246,45 mm**.

Esses resultados descrevem diferenças meteorológicas durante o período escolhido. Não permitem concluir diretamente sobre crescimento vegetal, produtividade ou disponibilidade de água no solo.

## 7. Limitações

- A análise compreende apenas um mês, não caracterizando o clima de longo prazo nem tendências climáticas.
- Os valores do ERA5 são resultados de reanálise em grade espacial, não necessariamente medições de estações localizadas em cada cidade.
- As dez cidades constituem um recorte didático não aleatório e não representam toda a variabilidade regional.
- O projeto não inclui medições de germinação, crescimento, produtividade, solo ou teor de água no solo.
- O snapshot permite reprocessamento offline, mas não demonstra uma nova coleta ativa quando utilizado.

## 8. Próximos passos

- Ampliar a análise para séries históricas de vários anos.
- Integrar informações meteorológicas com dados de solo, índices de vegetação e experimentos de germinação ou desenvolvimento vegetal.
- Comparar reanálises meteorológicas com registros de estações de superfície, examinando diferenças e incertezas.

## 9. Código

**Notebook:** [Acessar projeto no Google Colab](https://colab.research.google.com/drive/1CnIoqDJaReQGkCdjOW8CINcD2ACcMM4J?usp=sharing)

O notebook contém a coleta pela Open-Meteo, conversão JSON para DataFrame, limpeza, agregação, testes de integridade, análise exploratória, gráficos e exportação do CSV. O link foi extraído do notebook fornecido; não foi testada a permissão pública de acesso.

## 10. Referências

- OPEN-METEO. *Historical Weather API*. https://open-meteo.com/en/docs/historical-weather-api.
- PEREIRA, Danilo. *Aula 4.1: Coleta de Dados — Projeto Prático Completo* e *Aula 4.2: Processamento e Coleta de Dados*. Material didático fornecido na disciplina.
