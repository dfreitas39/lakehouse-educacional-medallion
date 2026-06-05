\# Arquitetura do Projeto



\## Visão Geral



Este projeto implementa a arquitetura Medallion sobre um Lakehouse

usando Apache Spark, Delta Lake e Databricks Free Edition.



A arquitetura Medallion organiza os dados em três camadas progressivas

de qualidade, onde cada camada tem uma responsabilidade clara e

bem definida.



\---



\## Diagrama



FONTE EXTERNA

student-mat.csv

↓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥉 BRONZE

bronze\_student\_performance

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

→ Dado bruto sem transformações

→ Metadados de auditoria adicionados

→ Formato: Delta Lake

&#x20;    ↓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥈 SILVER

silver\_student\_performance

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

→ Colunas renomeadas para português

→ Tipos de dados garantidos

→ Campos categóricos padronizados

→ Regras de negócio aplicadas

→ Formato: Delta Lake

↓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥇 GOLD

gold\_desempenho\_por\_escola

gold\_fatores\_risco\_reprovacao

gold\_perfil\_alunos

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

→ Tabelas analíticas por domínio

→ Prontas para BI e Machine Learning

→ Formato: Delta Lake



Cola esse conteúdo no arquivo:

markdown# Arquitetura do Projeto



\## Visão Geral



Este projeto implementa a arquitetura Medallion sobre um Lakehouse

usando Apache Spark, Delta Lake e Databricks Free Edition.



A arquitetura Medallion organiza os dados em três camadas progressivas

de qualidade, onde cada camada tem uma responsabilidade clara e

bem definida.



\---



\## Diagrama

FONTE EXTERNA

student-mat.csv

↓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥉 BRONZE

bronze\_student\_performance

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

→ Dado bruto sem transformações

→ Metadados de auditoria adicionados

→ Formato: Delta Lake

↓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥈 SILVER

silver\_student\_performance

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

→ Colunas renomeadas para português

→ Tipos de dados garantidos

→ Campos categóricos padronizados

→ Regras de negócio aplicadas

→ Formato: Delta Lake

↓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🥇 GOLD

gold\_desempenho\_por\_escola

gold\_fatores\_risco\_reprovacao

gold\_perfil\_alunos

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

→ Tabelas analíticas por domínio

→ Prontas para BI e Machine Learning

→ Formato: Delta Lake



\---



\## Detalhamento das Camadas



\### Bronze

\*\*Responsabilidade:\*\* Fidelidade ao dado original



\*\*O que acontece:\*\*

\- Leitura do CSV bruto com inferSchema

\- Adição de metadados de auditoria

\- Salvamento como Delta Table no catálogo



\*\*Metadados adicionados:\*\*

| Coluna | Descrição |

|--------|-----------|

| \_ingestion\_timestamp | Data e hora da ingestão |

| \_source\_file | Nome do arquivo de origem |

| \_layer | Identificador da camada |



\*\*Princípio:\*\* O dado entra exatamente como veio da fonte.

Se algo der errado nas camadas seguintes, a Bronze permite

reprocessamento sem perda.



\---



\### Silver

\*\*Responsabilidade:\*\* Confiabilidade do dado



\*\*O que acontece:\*\*

\- Renomeação de 33 colunas para nomes descritivos em português

\- Verificação e tratamento de nulos e duplicatas

\- Cast explícito de tipos numéricos

\- Padronização de campos categóricos

\- Aplicação de regras de negócio



\*\*Regras de negócio aplicadas:\*\*

| Regra | Lógica |

|-------|--------|

| status\_aprovacao | nota\_final >= 10 → aprovado |

| status\_aprovacao | nota\_final == 0 → sem\_avaliacao |

| status\_aprovacao | demais casos → reprovado |

| media\_notas | média das três avaliações arredondada |



\*\*Padronizações aplicadas:\*\*

| Original | Transformado |

|----------|-------------|

| U | urbano |

| R | rural |

| F | feminino |

| M | masculino |



\---



\### Gold

\*\*Responsabilidade:\*\* Valor para o negócio



\*\*Tabelas criadas:\*\*



\*\*gold\_desempenho\_por\_escola\*\*

\- Agrupamento por escola, sexo e status de aprovação

\- Métricas: total de alunos, médias de nota e faltas

\- Percentual proporcional por grupo

\- Resolve disparidade de volume entre escolas



\*\*gold\_fatores\_risco\_reprovacao\*\*

\- Cruzamento de status de aprovação com perfil do aluno

\- Variáveis: ensino superior, internet, suporte escolar

\- Métricas: reprovações anteriores, faltas, consumo de álcool

\- Base para identificação de perfis de risco



\*\*gold\_perfil\_alunos\*\*

\- Registro individual por aluno

\- Ranking global por média de notas usando Window Function

\- Classificação por faixa de desempenho

\- Base para modelo de Machine Learning futuro



\---



\## Decisões Técnicas



\### Por que Delta Lake ao invés de Parquet ou CSV?

Delta Lake adiciona transações ACID, schema enforcement e

versionamento sobre arquivos Parquet. Isso garante confiabilidade

em todas as camadas e permite reprocessamento seguro sem

corrupção de dados.



\### Por que saveAsTable ao invés de LOCATION?

O ambiente Databricks Free Edition utiliza storage gerenciado

internamente. Em ambiente corporativo com Azure ou AWS, o

LOCATION apontaria para o caminho do storage externo:

\- Azure: abfss://container@storage.dfs.core.windows.net/caminho

\- AWS: s3://bucket/caminho



\### Por que DataFrame em memória ao invés de múltiplas leituras?

Carregar o df\_silver uma única vez e criar todos os DataFrames

Gold a partir dele reduz operações de I/O. Em datasets de grande

volume isso representa redução significativa de tempo de

processamento e custo de cluster.



\### Por que percentual proporcional na Gold 1?

Comparar GP (349 alunos) com MS (46 alunos) em valores absolutos

seria enganoso. O percentual proporcional garante que a comparação

entre escolas seja justa independente do volume de cada uma.

