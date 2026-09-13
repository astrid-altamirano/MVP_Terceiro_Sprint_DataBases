# 🥔 MVP: Construção de um Pipeline de Dados na Nuvem
### Estudo do Impacto Financeiro dos Agrotóxicos e dos Fertilizantes no Custo de Produção da Batata-Inglesa em Minas Gerais

* **Aluna:** Astrid Eleanor Altamirano Junqueira
* **Data:** 12 de Setembro de 2026

---

## 1. Contexto de Negócios e Perguntas (Etapa 2 e 4.1)

### Contexto e Objetivo
O objetivo deste MVP é analisar o **impacto financeiro estrutural** que as classes de insumos (fertilizantes e agrotóxicos) exercem sobre o custo de produção por hectare da batata-inglesa (*Solanum tuberosum*) nas regiões de **Bueno Brandão** e **Santa Rita de Caldas**, no estado de **Minas Gerais**. O entendimento dessa dinâmica permite avaliar a vulnerabilidade do produtor rural diante de choques de oferta internacionais.

### Perguntas de Negócio que o Pipeline Responde
* **Pergunta Principal:** O custo dos agrotóxicos ou o custo dos fertilizantes tem maior impacto no aumento do custo total de produção da batata-inglesa em Minas Gerais?
* **PE1:** Qual classe de insumo historicamente representa o maior peso financeiro por hectare na produção de batata em MG?
* **PE2:** Como se comportou a volatilidade e o crescimento dos custos de fertilizantes em comparação com os agrotóxicos no período de 2017 a 2025 em Bueno Brandão?
* **PE3:** Existem diferenças entre os custos dos fertilizantes e agrotóxicos aplicados nas variedades (tipos de safra) de batata-inglesa na região de Bueno Brandão?

### Licença dos Dados
Os dados brutos utilizados são de origem **Pública / Governo Aberto (ODC-By / CC-BY)**, extraídos das séries históricas oficiais de custos de produção agropecuária da **Companhia Nacional de Abastecimento (CONAB)**.

---

## 2. Carga dos Dados (Etapa 4.2)

### Estratégia de Ingestão
Adotou-se uma **abordagem híbrida de ingestão** para a nuvem:
1. Os dados consolidados da CONAB foram baixados e carregados manualmente para a plataforma **Databricks Free Edition (Community)**.
2. Utilizou-se a ferramenta de armazenamento do **Unity Catalog** dentro do catálogo privado da conta (`/Volumes/workspace/default/batata_volume/`).
3. A carga bruta foi executada pelo Spark lendo o arquivo `Batata CONAB.csv`. O pipeline realiza o tratamento imediato dos metadados para evitar falhas de persistência.

---

## 3. Modelagem e Catálogo de Dados (Etapa 4.3)

### Arquitetura Medalhão
* **Camada Bronze:** Os dados são extraídos idênticos à fonte e persistidos na tabela Delta `tabela_custos_batata_bronze` para garantir o histórico e a rastreabilidade.
* **Camada Silver:** Executa-se o processo de limpeza, tipagem correta e renomeação de campos com caracteres especiais inválidos para a tabela analítica `tabela_custos_insumos`.

### Dicionário de Dados (Tabela Silver)

| Nome do Campo | Descrição | Tipo de Dado | Regras de Negócio | Linhagem |
| :--- | :--- | :--- | :--- | :--- |
| **ano** | Ano de referência analítica da safra. | Integer | 2012 a 2025 | Extraído da CONAB. |
| **regiao** | Nome do município produtor em MG. | String | "Bueno Brandao" ou "Santa Rita de Caldas" | Padronizado na Silver. |
| **variedade** | Tipo de safra de cultivo da batata. | String | "Batata das Aguas" ou "Convencional" | Campo original da fonte. |
| **fertilizante** | Custo de fertilizantes por hectare (R\$/ha). | Double | Valores decimais positivos (> 0) | Coluna isolada e limpa. |
| **agrotoxico** | Custo de agrotóxicos por hectare (R\$/ha). | Double | Valores decimais positivos (> 0) | Coluna isolada e limpa. |

---

## 4. Pipeline de Dados (Etapa 4.4)

O pipeline ETL foi unificado em um notebook estruturado no Databricks utilizando **PySpark** para o processamento distribuído de dados e **Pandas/Plotly** para a renderização de gráficos. O fluxo lê os dados do Volume, realiza a sanitização de strings, padroniza as colunas econômicas e persiste os resultados de forma definitiva no Data Lakehouse via tabelas Delta organizadas semanticamente.

---

## 5. Qualidade de Dados (Etapa 4.5)

Para garantir a confiabilidade analítica exigida pelo MVP, o pipeline executa testes automatizados de qualidade na camada intermediária antes de alimentar os painéis:
* **Completude (Valores Nulos):** O notebook rodou uma verificação em todas as colunas e confirmou a existência de **0% de valores nulos**, comprovando a integridade da captura.
* **Unicidade (Duplicados):** O Spark validou o total de 14 registros históricos contra 14 linhas únicas, constatando a **ausência completa de linhas duplicadas** no conjunto de dados.

---

## 6. Análise de Dados e Resposta às Perguntas (Etapa 4.5)

### Justificativa Metodológica sobre o Uso do ANOVA
O teste estatístico ANOVA clássico **não foi mantido** na execução do pipeline final deste MVP, uma vez que a inconsistência na série temporal gerava distorções matemáticas (`SingularMatrixWarning`). A ferramenta é mencionada nesta análise exclusivamente com o objetivo de explicar o embasamento metodológico que levou à identificação dos vieses amostrais e à escolha da abordagem analítica de médias agrupadas puras.

### Resposta à Pergunta Principal & PE1 (Peso Estrutural)
Os dados reais demonstram que os **Fertilizantes exercem o maior impacto financeiro** no custo de produção da batata-inglesa em Minas Gerais. 
* Na distribuição de gastos registrada para a safra de 2025, os fertilizantes abocanharam **56,7% do orçamento total** de insumos, contra **43,3% dos agrotóxicos**.
* Mesmo no período pré-crise, a média histórica de gastos com fertilização superava consistentemente o custo de defesa fitossanitária.

### Resposta à PE2 (Comportamento e Volatilidade em Bueno Brandão)
Entre 2017 e 2025, o município de Bueno Brandão conseguiu um crescimento agressivo nos custos. 
* **Fertilizantes:** Mostraram-se muito mais instáveis e imprevisíveis, registrando um **Coeficiente de Variação (CV) de 52,73%** (Média: R\$ 8.655,37/ha).
* **Agrotóxicos:** Apresentaram maior previsibilidade de mercado na série temporal, com volatilidade de **47,72%** (Média: R\$ 6.710,50/ha).

### Resposta à PE3 (Diferença entre Ciclos de Safra)
Ao isolar o viés amostral causado pelo choque de oferta da crise internacional de 2022, a análise econômica real comprova que a safra de **'Batata das Águas' (verão) é historicamente mais onerosa** do que o ciclo Convencional em condições de mercado equivalentes. Por ser cultivada obrigatoriamente no período de altas temperaturas e pluviosidade do verão de MG, essa safra exige um volume drástico de aplicações defensivas contra fungos e pragas. 

> ⚠️ **Nota Estatística:** O modelo estatístico ANOVA clássico declarava erroneamente o ciclo Convencional como mais caro simplesmente porque a CONAB registrou seus dados durante o ápice da crise global de preços de 2022.

---

## 7. Autoavaliação

A execução deste MVP proporcionou um aprendizado prático e profundo das responsabilidades diárias de um Engenheiro de Dados. O objetivo principal foi atingido com sucesso: construir um pipeline funcional de ponta a ponta na nuvem capaz de responder perguntas reais de negócio.

### Dificuldades Encontradas
A principal barreira técnica enfrentada decorreu das restrições de arquitetura e segurança do cluster gratuito do Databricks Community Edition (Spark Connect). O sistema gerava erros repetitivos de barramento de caminhos (`[DBFS_DISABLED]` e `[PATH_NOT_FOUND]`), causados pelo bloqueio de segurança à raiz pública do DBFS e pela sensibilidade a caracteres especiais e espaços vazios no nome dos arquivos (`Batata CONAB.csv`). Essas dificuldades foram superadas com a reestruturação e descoberta dos catálogos corretos via **Unity Catalog (workspace)**.

### Trabalhos Futuros
Para enriquecimento profissional do portfólio, projeta-se:
* A **automação da ingestão de dados** da CONAB diretamente via requisições de API para eliminar o processo de upload manual.
* O **balanceamento sistemático** das séries tempos para futuros modelos de Machine Learning preditivo.

