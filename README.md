# É possível antecipar dificuldades financeiras em empresas brasileiras com Machine Learning?

**Estudo de Estatística e Machine Learning aplicado a informações contábeis da CVM.**


## Problema investigado

A investigação procura responder: **quais indicadores financeiros ajudam a antecipar a primeira ocorrência observada da classificação de dificuldade financeira em companhias abertas brasileiras?** Foram examinadas informações de liquidez, cobertura de juros, estrutura de financiamento e crescimento, com atenção à qualidade dos dados e ao desempenho em períodos futuros.

## Fonte dos dados e variável-alvo

Foi utilizado o conjunto *Nonstationary and Imbalanced Dataset for Financial Distress Prediction*, disponibilizado por Rubens Marques Chaves (2025), com informações derivadas de demonstrações da Comissão de Valores Mobiliários (CVM).

- Base original: **32.005 observações**, **1.004 empresas**, **84 indicadores** (`A1`–`A84`).
- Painel após resolução de seis duplicatas empresa/trimestre: **31.999 observações**.
- Alvo construído: `TARGET_DISTRESS_12M`, que representa a primeira ocorrência futura de `LABEL=1` em **até 365 dias**, a partir de observações com `LABEL=0`, respeitando condições de acompanhamento.
- Base de modelagem: **27.408 observações**, **936 empresas**, **188 registros positivos**, distribuídos por **50 empresas** (prevalência de **0,686%**).

O significado geral de `LABEL=1` na fonte é *financial distress*. **A regra operacional integral de construção do rótulo não foi encontrada na documentação consultada.** Não se assume equivalência automática com falência ou recuperação judicial.

## Metodologia

Foram comparados os modelos **Regressão Logística** e **Random Forest** em validação cronológica. As principais métricas foram *Average Precision* (AP) e ROC-AUC. Também foram examinados rankings de risco, importância por permutação, trajetórias pré-evento e cenários hipotéticos de atraso na disponibilidade contábil.

## Resultados preditivos

| Período de teste | Prevalência | AP — Random Forest | AP — Regressão logística |
|---|---:|---:|---:|
| 2017–2018 | 0,468% | **2,737%** | 0,599% |
| 2019–2020 | 0,445% | **4,840%** | 1,099% |
| 2022–2023 (até março) | 1,507% | 5,240% | **5,869%** |


Os modelos superaram a referência da prevalência nos três períodos, mas os valores absolutos de AP foram modestos. No **Top 5% de registros empresa-trimestre**, a Random Forest recuperou pelo menos um registro de 4/9, 3/8 e 6/23 empresas positivas, respectivamente, enquanto a regressão logística recuperou 1/9, 1/8 e 8/23.

## Indicadores e análise de trajetórias

- **A39 (cobertura de juros)** e **A76 (crescimento dos ativos)** apresentaram contribuição média positiva por permutação nos três períodos.
- **A79 (crescimento do lucro operacional)** também apresentou contribuição média positiva nos três períodos, mas de menor magnitude.
- **A42 (patrimônio líquido/dívida)** e **A38 (liquidez imediata)** apresentaram contribuição positiva em dois dos três períodos.
- Na comparação retrospectiva de **47 empresas com evento** com **141 trajetórias de controle**, as evidências mais consistentes corresponderam a **diferenças de nível anteriores ao evento**, e não à demonstração de deterioração progressiva diferenciada.


## Verificações e limitações

A reconstrução do alvo apresentou **zero divergências** em 27.408 observações. Os cortes temporais auditados apresentaram folga de **59 a 60 dias** entre o horizonte do último rótulo de treino e o começo do teste. A ausência das datas efetivas de publicação contábil impede, porém, validar a antecedência operacional de cada alerta.

Na análise com atraso hipotético de 120 dias, **138 dos 188 registros positivos** ainda antecederiam o evento. Essa análise **não reestimou** o desempenho dos modelos. As trajetórias usam controles retrospectivos, sem pareamento por setor ou porte; os intervalos bootstrap são exploratórios.

Os resultados não são uma ferramenta validada de decisão automática nem recomendação de investimento.

## Como acessar os códigos

Os notebooks estão disponíveis em notebook.

## Artigo e referências

- Artigo no Medium: **(https://medium.com/@vitoriakarollinys/%C3%A9-poss%C3%ADvel-antecipar-dificuldades-financeiras-em-empresas-brasileiras-com-machine-learning-93a548cb553a?sharedUserId=vitoriakarollinys)**.
- Chaves, R. M. (2025). *Nonstationary and Imbalanced Dataset for Financial Distress Prediction*. IEEE DataPort. https://doi.org/10.21227/x8ry-gz76
- Dicionário/arquivo original: https://github.com/rubensmchaves/ml-fdp

*Este repositório não redistribui os microdados originais da CVM. Para reproduzir as análises desde a origem, é necessário obter a base na fonte e executar os notebooks em sequência. As verificações registradas foram realizadas sobre versões específicas dos dados e dos notebooks; sua publicação não equivale a um ambiente automatizado de reprodução integral.*
