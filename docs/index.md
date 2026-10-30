# Classificação de renda anual — Adult Income Census

- **Integrantes:** Enzo Santoro e Victor Dolnikoff
- **Disciplina:** Machine Learning — Insper
- **Entregas:** APS1 (EDA), prazo 14/09 · APS2 (Classificação), entregue em 16/10/2026, prazo 26/10

Projeto de Machine Learning em duas etapas sobre o dataset [Adult Income Census](https://www.kaggle.com/datasets/anaghakp/adult-income-census) (Kaggle). O objetivo é prever se a renda anual de uma pessoa é **maior que 50 mil dólares** (classe positiva `>50K`). A base tem 31.947 registros e 11 preditores, e a classe positiva representa cerca de 24% dos casos.

## Páginas

| Etapa | Conteúdo |
|---|---|
| [APS1 — EDA](aps1.ipynb) | Inspeção dos dados, análises univariada, bivariada e multivariada, PCA e pipeline de pré-processamento |
| [APS2 — Classificação](aps2.ipynb) | Três modelos, busca de hiperparâmetros, mudança no pipeline, curvas de aprendizado, avaliação no teste, SHAP e discussão crítica |

Cada página mostra o notebook com as saídas da execução e tem um botão para baixar o `.ipynb`. Os notebooks também rodam no Colab com **Ambiente de execução → Executar tudo**. Os dados da APS2 estão incorporados ao notebook, então não é preciso baixar nada.

## Principais resultados da APS2

**Protocolo:** split estratificado 80/20 com semente 42, validação cruzada estratificada de 3 dobras e **F1 da classe `>50K`** como métrica de seleção. O conjunto de teste só foi usado na avaliação final.

| Modelo | F1 em CV |
|---|---|
| Dummy (classe majoritária) | 0,000 (accuracy de 76%) |
| Regressão logística L2, ajustada | 0,655 |
| **Random Forest, ajustado** | **0,663** |

O modelo final é um Random Forest com `class_weight="balanced"`, sem limite de profundidade e com `min_samples_leaf=2`.

| Métrica no teste | Valor |
|---|---|
| Accuracy | 0,795 |
| Precision (`>50K`) | 0,550 |
| Recall (`>50K`) | 0,806 |
| F1 (`>50K`) | 0,654 |
| ROC-AUC | 0,881 |

**Achados principais:**

- **Mudança no pipeline:** a limpeza do sentinela `occupation` não melhorou o F1 (ΔF1 = −0,0016), então o pipeline original foi mantido, conforme a regra definida antes da comparação. O diagnóstico, porém, mostrou que esse valor inválido está associado ao alvo. Por isso recomendamos a versão com limpeza para qualquer uso real.
- **Interpretação:** estado civil, escolaridade e idade dominam as explicações, tanto no SHAP quanto na importância por impureza da floresta.
- **Auditoria por sexo:** o modelo gera mais falsos positivos para homens e mais falsos negativos para mulheres.

## Referências principais

As referências completas estão na seção 9 da APS2.

- BECKER, B.; KOHAVI, R. *Adult [Dataset]*. UCI Machine Learning Repository, 1996.
- LUNDBERG, S. M.; LEE, S.-I. *A Unified Approach to Interpreting Model Predictions*. NeurIPS, 2017.
- MITCHELL, M. et al. *Model Cards for Model Reporting*. FAT*, 2019.
