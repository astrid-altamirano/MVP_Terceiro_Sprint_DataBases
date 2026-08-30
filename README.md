# MVP_Terceiro_Sprint_DataBases

# Tópico 1: Contexto de Negócios e Perguntas (Etapa 2 e 4.1)
•	## O Problema: Ajudar os produtores de batata-inglesa de Santa Catarina a entender se o maior peso financeiro no custo de produção vem dos fungicidas (essenciais contra requeima e pinta-preta devido ao clima úmido do Sul) ou dos fertilizantes nitrogenados.
•	## As Perguntas de Negócio (Guias do Pipeline):
1.	Qual insumo representa a maior porcentagem do custo operacional total (COT) por hectare?
2.	Qual é a correlação percentual histórica entre o aumento do preço do Nitrogênio (Ureia/Adubos NPK) e o custo total da batata em SC?
3.	Existe sazonalidade anual (safra de inverno vs. verão) que torne um insumo mais caro que o outro devido ao clima?
•	## Os Dados Brutos: Bases oficiais de Custos de Produção da CONAB (regiões como Joaçaba ou Canoinhas) e da Epagri/CEPA.
•	## Licença: Pública / Governo Aberto (ODC-By / CC-BY).
# Tópico 2: Carga dos Dados (Etapa 4.2)
•	## Como foi feito: Download manual dos arquivos .csv ou .xlsx dos portais da CONAB/Epagri e upload direto para os Volumes do Unity Catalog (ou DBFS) no Databricks.
•	## Instrução do PDF: Colar aqui o link do seu script do GitHub que faz a leitura desse arquivo.
# Tópico 3: Modelagem e Catálogo de Dados (Etapa 4.3)
Criação de duas tabelas estruturadas no Databricks:
1.	tabela_custos_insumos: Colunas como ano, mes, tipo_insumo (Fungicida ou Nitrogênio) e valor_por_hectare.
2.	tabela_producao_total: Colunas como ano, custo_total_producao e regiao_sc.
•	Instrução do PDF: Criar o dicionário de dados (ex: ano = Ano da safra, formato Inteiro) e colar um print da tela do Databricks com as tabelas criadas.
# Tópico 4: Pipeline de Dados / ETL (Etapa 4.4)
•	Extract: Leitura do arquivo bruto carregado no Databricks.
•	Transform: Filtragem para "Santa Catarina", remoção de linhas em branco e padronização de termos (ex: transformar "FUNG." em "Fungicida").
•	Load: Salvamento dos dados limpos nas tabelas finais da etapa de modelagem.
•	Instrução do PDF: Organizar tudo em um único Notebook em PySpark/SQL e colar um print provando o salvamento com sucesso.
# Tópico 5: Qualidade de Dados (Etapa 4.5)
•	Completude: Verificação de custos nulos ou em branco (substituídos por zero).
•	Consistência: Verificação se os valores monetários estão em ponto flutuante (Ex: 1500.50).
•	Unicidade: Remoção de dados duplicados da CONAB utilizando a função .dropDuplicates() do Spark.
# Tópico 6: Análise de Dados (Etapa 4.5)
•	Execução: Consultas SQL ou PySpark para somar e comparar os custos operacionais.
•	Exemplo de Resposta esperado: Os fungicidas representam cerca de 25% do custo operacional em SC, superando os fertilizantes nitrogenados (18%), devido ao clima úmido que exige mais aplicações.
•	Instrução do PDF: Colar um print dos gráficos ou tabelas de resultados gerados.
# Tópico 7: Autoavaliação
•	Relato pessoal sobre o cumprimento dos objetivos, principais dificuldades (como mexer no Databricks pela primeira vez) e propostas de melhorias futuras (como conectar uma API de previsão do tempo).
