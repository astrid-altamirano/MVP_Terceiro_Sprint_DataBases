## Autoavaliação

Ao finalizar o desenvolvimento deste MVP, apresento uma reflexão crítica sobre os objetivos alcançados, os desafios técnicos superados e as perspectivas de evolução para o projeto.

### Atingimento de Objetivos
Os objetivos centrais traçados no início do projeto foram integralmente atingidos. Foi possível construir um pipeline funcional de ponta a ponta utilizando a plataforma Databricks na nuvem. A transformação e a modelagem dos dados brutos permitiram responder com precisão matemática às perguntas de negócio. O teste estatístico ANOVA confirmou de forma robusta que os fertilizantes exercem o maior impacto financeiro estrutural na produção de batata-inglesa em Minas Gerais, superando os agrotóxicos independentemente de crises sazonais.

### Dificuldades Encontradas na Execução
O principal desafio técnico esteve relacionado à manipulação do ecossistema Apache Spark dentro das limitações da subconta *Databricks Free Edition*. Por se tratar de um volume de dados enxuto (dados históricos consolidados por município), a arquitetura distribuída do Spark exige atenção redobrada para evitar problemas de sobreposição de metadados. 

Além disso, no campo da análise estatística avançada, a amostra restrita gerou uma colinearidade natural entre as variáveis categóricas (anos específicos atrelados a determinadas variedades), o que resultou em um aviso de matriz deficitária (`SingularMatrixWarning`) no pacote `statsmodels`. Esse obstáculo foi superado isolando os fatores e interpretando os valores-p com foco no impacto puramente bi-fatorial (Classe de Insumo e Variedade), neutralizando distorções.

### Trabalhos Futuros para o Portfólio
Para enriquecer esta solução e transformá-la em um produto de dados de nível empresarial (Enterprise), planejo implementar as seguintes melhorias no futuro:
1. **Ingestão Automatizada Avançada:** Substituir o upload manual de arquivos consolidados pela construção de um pipeline de extração automatizado via API ou um robô de *web scraping* conectado diretamente ao portal de dados abertos da CONAB.
2. **Expansão da Granularidade Territorial:** Expandir o pipeline para englobar todos os estados produtores de batata do Brasil, permitindo uma análise comparativa de custo logístico e impacto macroeconômico regional.
3. **Orquestração de Pipeline:** Implementar o fluxo de tarefas utilizando o *Databricks Workflows* ou *Apache Airflow*, adicionando alertas automáticos de falhas e monitoramento de qualidade em tempo real (Data Quality Gates com ferramentas como *Great Expectations*).
