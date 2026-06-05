\# Lakehouse Educacional — Arquitetura Medallion



Projeto de engenharia de dados que implementa um lakehouse corporativo

usando arquitetura Medallion (Bronze → Silver → Gold) com Apache Spark,

Delta Lake e Databricks.



\---



\## Contexto do Problema



Instituições de ensino geram dados em múltiplas fontes: sistemas de

matrícula, plataformas de aulas e avaliações. A falta de centralização

impede análises confiáveis e decisões baseadas em dados.



Este projeto simula o ambiente de dados de uma instituição de ensino,

centralizando, tratando e disponibilizando dados educacionais para

consumo analítico.



\---



\## Objetivo



Construir um lakehouse com três camadas progressivas de qualidade,

partindo do dado bruto até tabelas analíticas prontas para consumo

por ferramentas de BI e modelos de Machine Learning.



\---



\## Arquitetura



CSV original

↓

🥉 Bronze → dado bruto com metadados de auditoria

↓

🥈 Silver → dado limpo, tipado e com regras de negócio

↓

🥇 Gold   → tabelas analíticas prontas para consumo



\### Camadas



| Camada | Descrição | Formato |

|--------|-----------|---------|

| Bronze | Dado bruto ingerido sem transformações | Delta Lake |

| Silver | Dado limpo, tipado e padronizado | Delta Lake |

| Gold | Tabelas agregadas por domínio de negócio | Delta Lake |



\---



\## Tabelas do Catálogo



| Tabela | Camada | Descrição |

|--------|--------|-----------|

| bronze\_student\_performance | Bronze | Dado bruto com metadados |

| silver\_student\_performance | Silver | Dado tratado e enriquecido |

| gold\_desempenho\_por\_escola | Gold | Desempenho por escola e sexo |

| gold\_fatores\_risco\_reprovacao | Gold | Fatores associados à reprovação |

| gold\_perfil\_alunos | Gold | Perfil completo com ranking |



\---



\## Principais Insights



\### Taxa de aprovação geral

\- 67.1% dos alunos aprovados

\- 23.3% reprovados

\- 94.9% querem ensino superior — 87 alunos querem continuar

&#x20; estudando mas estão reprovando



\### Horas de estudo

\- Alunos que estudam 3 ou mais horas têm taxa de aprovação

&#x20; de 83% contra 70% dos que estudam menos de 2 horas

\- O salto mais significativo acontece entre 2 e 3 horas de estudo



\### Faltas como preditor de reprovação

\- Reprovados faltam em média 9.6 vezes contra 5.2 dos aprovados

\- Reprovados já começam o ano com notas baixas no 1º bimestre

&#x20; e mantêm trajetória descendente até o final



\### Perfil de risco

\- Alunos com reprovações anteriores, tempo livre máximo e

&#x20; frequência de saídas máxima concentram os piores desempenhos

\- Histórico de reprovação é o preditor mais forte de nova reprovação



\### Escolaridade da mãe

\- Quanto maior a escolaridade da mãe maior a taxa de aprovação

\- Mães com ensino superior completo têm filhos com 78.4%

&#x20; de taxa de aprovação contra 68% de mães sem ensino fundamental



\### Alunos rurais

\- Representam 22% do dataset mas ocupam 30% do top 10

\- Probabilidade 49% maior de aparecer no ranking de melhores

&#x20; alunos em comparação com alunos urbanos



\---



\## Estrutura do Repositório



lakehouse-educacional-medallion/

│

├── README.md

├── .gitignore

├── requirements.txt

│

├── data/

│   └── raw/

│       └── student-mat.csv

│

├── notebooks/

│   ├── 01\_bronze\_ingestion.ipynb

│   ├── 02\_silver\_transformation.ipynb

│   ├── 03\_gold\_aggregation.ipynb

│   └── 04\_sql\_analysis.ipynb

│

├── docs/

│   ├── architecture.md

│   └── data\_dictionary.md

│

└── assets/

└── screenshots/



\---



\## Tecnologias Utilizadas



| Tecnologia | Versão | Uso |

|------------|--------|-----|

| Apache Spark | 4.1.0 | Processamento distribuído |

| Delta Lake | Nativo Databricks | Formato de armazenamento |

| PySpark | 4.1.0 | Transformações em Python |

| Spark SQL | 4.1.0 | Consultas analíticas |

| Databricks | Free Edition | Plataforma de execução |

| Python | 3.x | Linguagem principal |

| Git | - | Controle de versão |



\---



\## Dataset



\- \*\*Nome:\*\* Student Performance Dataset

\- \*\*Fonte:\*\* UCI Machine Learning Repository via Kaggle

\- \*\*Link:\*\* https://www.kaggle.com/datasets/uciml/student-alcohol-consumption

\- \*\*Arquivo utilizado:\*\* student-mat.csv

\- \*\*Volume:\*\* 395 registros, 33 colunas

\- \*\*Contexto:\*\* Dados de desempenho de alunos em Matemática

&#x20; coletados em Portugal



\---



\## Como Executar



1\. Criar conta gratuita em https://community.cloud.databricks.com

2\. Criar um schema chamado `lakehouse\_edu` no catálogo `workspace`

3\. Criar um volume chamado `bronze` dentro do schema

4\. Fazer upload do arquivo `student-mat.csv` para o volume

5\. Importar os notebooks da pasta `/notebooks`

6\. Executar na ordem: 01 → 02 → 03 → 04



\---



\## Autor



Daniel Freitas

www.linkedin.com/in/daniel-ccordeiro/

