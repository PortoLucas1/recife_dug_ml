# recife_dug_ml — ML na Databricks na prática (churn de telecom)

Duas demos **hands-on de ponta a ponta** de Machine Learning no Lakehouse Databricks: dos dados brutos ao modelo registrado, servindo em tempo real e monitorado em produção. Tudo em português, com dados **100% sintéticos** e **seeds fixas** (reprodutível em qualquer execução).

Público: profissionais e estudantes que já conhecem SQL/Python e querem ver **como ML funciona de verdade na plataforma Databricks** — Unity Catalog, MLflow, Feature Engineering, Model Serving e monitoramento.

## O que tem aqui

| Notebook | Foco | Duração aproximada |
| --- | --- | --- |
| `demo_churn_mlflow_simple.ipynb` | **Comece por aqui.** Pipeline simplificado em 8 seções: feature table no Unity Catalog, LightGBM com Optuna, explicabilidade com SHAP, registro de modelo com aliases, endpoint de serving **real**, inferência em batch e monitoramento com Lakehouse Monitoring. | ~15 min (serverless) |
| `demo_churn_ml.ipynb` | **Demo avançada** (12 seções): dados com problemas de qualidade propositais, features point-in-time com FeatureEngineeringClient, vazamento de dados (data leakage) demonstrado na prática, comparação de modelos, gate de promoção candidate/champion, scoring em batch com plano de ação por faixa de risco e detecção de drift. | ~30 min (serverless) |

## Resultados de referência (o que você deve ver ao executar)

* **demo_churn_ml**: GradientBoosting com AUC 94,5%; no limiar de negócio 0,10, precisão 58% e recall 82%. O trio do demo ilustra as faixas de ação: Ana 31,6% (risco Elevado → priorizar contato), Carlos 5,4% (Médio → revisar antes de contatar), Bruno 0,19% (Baixo → manter acompanhamento).
* **demo_churn_mlflow_simple**: LightGBM com F1 0,79 e ROC-AUC 0,92. Os fatores mais importantes segundo SHAP: tickets de prioridade alta, pedidos de cancelamento e reclamações de cobrança.

## Pré-requisitos

* Workspace Databricks com **Unity Catalog** habilitado e acesso a compute **serverless**.
* Permissão para criar schemas, tabelas, modelos e endpoints no catálogo `workspace`.
* As bibliotecas extras (`databricks-feature-engineering`, `optuna`, `lightgbm`, `shap`) são instaladas pela primeira célula com `%pip`.

## Como executar

1. Copie os notebooks para o seu workspace (ou abra direto neste Git folder).
2. Anexe um compute serverless e execute as células **na ordem, de cima para baixo** — cada demo é um roteiro único.
3. Os objetos são criados nos schemas `workspace.recife_dug` (demo avançada) e `workspace.recife_dug_simple` (demo simples). Nada externo é consumido.

## Conceitos que você vai ver na prática

* **Point-in-time correctness e vazamento de dados**: por que agregar eventos apenas até a data do rótulo — e o que acontece se você errar isso (métrica bonita, modelo inútil).
* **Feature tables no Unity Catalog**: features reutilizáveis, com linhagem e descoberta centralizada, sem train/serve skew.
* **MLflow Tracking**: experimentos, runs, parâmetros e métricas comparáveis lado a lado.
* **Model Registry com aliases**: candidate vs champion, promoção com gate de critérios de negócio.
* **Model Serving**: endpoint real com scale-to-zero (custo parado ≈ 0) e validação de payload contra a assinatura do modelo.
* **Monitoramento**: baseline, drift e qualidade do modelo em produção (Lakehouse Monitoring / dashboard de drift).

## Avisos

* Dados sintéticos gerados no próprio notebook — não há dados reais de clientes.
* Endpoints usam scale-to-zero: a primeira requisição após um período parado tem cold start de alguns segundos.
* Cada demo termina com uma célula de limpeza (comentada, protegida por confirmação) para remover os objetos criados, se você quiser.
* Avatares dos clientes do demo (Ana, Bruno e Carlos): ícones criados por [Magnific](https://www.flaticon.com/authors/magnific), distribuídos via [Flaticon](https://www.flaticon.com/).

