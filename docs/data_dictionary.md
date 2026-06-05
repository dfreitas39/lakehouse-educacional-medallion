\# Dicionário de Dados



\## Camada Bronze — bronze\_student\_performance



Dado bruto ingerido sem transformações. Colunas com nomes

originais do dataset.



| Coluna | Tipo | Descrição |

|--------|------|-----------|

| school | string | Identificador da escola |

| sex | string | Sexo do aluno (F/M) |

| age | integer | Idade do aluno |

| address | string | Tipo de endereço (U/R) |

| famsize | string | Tamanho da família |

| Pstatus | string | Status dos pais (T=juntos, A=separados) |

| Medu | integer | Escolaridade da mãe (0-4) |

| Fedu | integer | Escolaridade do pai (0-4) |

| Mjob | string | Profissão da mãe |

| Fjob | string | Profissão do pai |

| reason | string | Motivo de escolha da escola |

| guardian | string | Responsável pelo aluno |

| traveltime | integer | Tempo de deslocamento (1-4) |

| studytime | integer | Horas de estudo por semana (1-4) |

| failures | integer | Reprovações anteriores (0-4) |

| schoolsup | string | Suporte escolar (yes/no) |

| famsup | string | Suporte familiar (yes/no) |

| paid | string | Aulas pagas (yes/no) |

| activities | string | Atividades extracurriculares (yes/no) |

| nursery | string | Frequentou creche (yes/no) |

| higher | string | Quer ensino superior (yes/no) |

| internet | string | Tem internet em casa (yes/no) |

| romantic | string | Tem relacionamento (yes/no) |

| famrel | integer | Qualidade da relação familiar (1-5) |

| freetime | integer | Tempo livre após escola (1-5) |

| goout | integer | Frequência de saídas (1-5) |

| Dalc | integer | Consumo de álcool durante semana (1-5) |

| Walc | integer | Consumo de álcool no fim de semana (1-5) |

| health | integer | Estado de saúde (1-5) |

| absences | integer | Número de faltas no ano |

| G1 | integer | Nota do 1º bimestre (0-20) |

| G2 | integer | Nota do 2º bimestre (0-20) |

| G3 | integer | Nota final (0-20) |

| \_ingestion\_timestamp | timestamp | Data e hora da ingestão |

| \_source\_file | string | Nome do arquivo de origem |

| \_layer | string | Identificador da camada |



\---



\## Camada Silver — silver\_student\_performance



Dado limpo, tipado e enriquecido com regras de negócio.

Colunas renomeadas para português descritivo.



| Coluna | Tipo | Descrição | Valores |

|--------|------|-----------|---------|

| escola | string | Identificador da escola | gp, ms |

| sexo | string | Sexo do aluno | feminino, masculino |

| idade | integer | Idade do aluno | 15-22 |

| tipo\_endereco | string | Localização da residência | urbano, rural |

| tamanho\_familia | string | Tamanho da família | GT3, LE3 |

| status\_pais | string | Status dos pais | T, A |

| escolaridade\_mae | integer | Escolaridade da mãe | 0-4 |

| escolaridade\_pai | integer | Escolaridade do pai | 0-4 |

| profissao\_mae | string | Profissão da mãe | - |

| profissao\_pai | string | Profissão do pai | - |

| motivo\_escolha\_escola | string | Motivo de escolha da escola | - |

| responsavel | string | Responsável pelo aluno | - |

| tempo\_deslocamento | integer | Tempo de deslocamento | 1-4 |

| horas\_estudo\_semana | integer | Horas de estudo por semana | 1-4 |

| reprovacoes\_anteriores | integer | Reprovações anteriores | 0-4 |

| suporte\_escolar | string | Suporte escolar | yes, no |

| suporte\_familiar | string | Suporte familiar | yes, no |

| aulas\_pagas | string | Aulas pagas | yes, no |

| atividades\_extracurriculares | string | Atividades extracurriculares | yes, no |

| frequentou\_creche | string | Frequentou creche | yes, no |

| quer\_ensino\_superior | string | Quer ensino superior | yes, no |

| tem\_internet | string | Tem internet em casa | yes, no |

| tem\_relacionamento | string | Tem relacionamento | yes, no |

| qualidade\_relacao\_familiar | integer | Qualidade da relação familiar | 1-5 |

| tempo\_livre | integer | Tempo livre após escola | 1-5 |

| frequencia\_saidas | integer | Frequência de saídas | 1-5 |

| consumo\_alcool\_semana | integer | Consumo de álcool na semana | 1-5 |

| consumo\_alcool\_fds | integer | Consumo de álcool no fim de semana | 1-5 |

| saude | integer | Estado de saúde | 1-5 |

| faltas | integer | Número de faltas no ano | 0-93 |

| nota\_1\_bimestre | integer | Nota do 1º bimestre | 0-20 |

| nota\_2\_bimestre | integer | Nota do 2º bimestre | 0-20 |

| nota\_final | integer | Nota final | 0-20 |

| status\_aprovacao | string | Status calculado | aprovado, reprovado, sem\_avaliacao |

| media\_notas | double | Média das três avaliações | 0-20 |

| \_ingestion\_timestamp | timestamp | Data e hora da ingestão original | - |

| \_source\_file | string | Arquivo de origem | - |

| \_silver\_timestamp | timestamp | Data e hora da transformação Silver | - |

| \_layer | string | Identificador da camada | silver |



\### Escalas ordinais



\*\*escolaridade\_mae / escolaridade\_pai:\*\*

| Valor | Descrição |

|-------|-----------|

| 0 | Sem educação |

| 1 | Ensino fundamental incompleto |

| 2 | Ensino fundamental completo |

| 3 | Ensino médio completo |

| 4 | Ensino superior completo |



\*\*horas\_estudo\_semana:\*\*

| Valor | Descrição |

|-------|-----------|

| 1 | Menos de 2 horas |

| 2 | Entre 2 e 5 horas |

| 3 | Entre 5 e 10 horas |

| 4 | Mais de 10 horas |



\*\*qualidade\_relacao\_familiar / tempo\_livre / frequencia\_saidas /

consumo\_alcool\_semana / consumo\_alcool\_fds / saude:\*\*

| Valor | Descrição |

|-------|-----------|

| 1 | Muito baixo |

| 2 | Baixo |

| 3 | Médio |

| 4 | Alto |

| 5 | Muito alto |



\---



\## Camada Gold



\### gold\_desempenho\_por\_escola



| Coluna | Tipo | Descrição |

|--------|------|-----------|

| escola | string | Identificador da escola |

| sexo | string | Sexo do grupo |

| status\_aprovacao | string | Status de aprovação do grupo |

| total\_alunos | integer | Total de alunos no grupo |

| media\_nota\_final | double | Média da nota final do grupo |

| media\_geral | double | Média das três avaliações |

| media\_faltas | double | Média de faltas do grupo |

| media\_horas\_estudo | double | Média de horas de estudo |

| total\_escola | integer | Total de alunos na escola |

| percentual\_grupo | double | Percentual proporcional do grupo |



\### gold\_fatores\_risco\_reprovacao



| Coluna | Tipo | Descrição |

|--------|------|-----------|

| status\_aprovacao | string | Status de aprovação |

| quer\_ensino\_superior | string | Aspiração ao ensino superior |

| tem\_internet | string | Acesso à internet |

| suporte\_escolar | string | Recebe suporte escolar |

| total\_alunos | integer | Total de alunos no grupo |

| media\_nota\_final | double | Média da nota final |

| media\_reprovacoes | double | Média de reprovações anteriores |

| media\_faltas | double | Média de faltas |

| media\_alcool\_semana | double | Média de consumo de álcool na semana |



\### gold\_perfil\_alunos



| Coluna | Tipo | Descrição |

|--------|------|-----------|

| escola | string | Identificador da escola |

| sexo | string | Sexo do aluno |

| idade | integer | Idade do aluno |

| tipo\_endereco | string | Tipo de endereço |

| horas\_estudo\_semana | integer | Horas de estudo por semana |

| reprovacoes\_anteriores | integer | Reprovações anteriores |

| tem\_internet | string | Tem internet em casa |

| quer\_ensino\_superior | string | Quer ensino superior |

| faltas | integer | Número de faltas |

| nota\_1\_bimestre | integer | Nota do 1º bimestre |

| nota\_2\_bimestre | integer | Nota do 2º bimestre |

| nota\_final | integer | Nota final |

| media\_notas | double | Média das três avaliações |

| status\_aprovacao | string | Status de aprovação |

| ranking | integer | Ranking global por média de notas |

| faixa\_desempenho | string | Classificação de desempenho |



\*\*faixa\_desempenho:\*\*

| Valor | Critério |

|-------|----------|

| excelente | média >= 16 |

| bom | média >= 12 |

| regular | média >= 10 |

| insuficiente | média < 10 |

