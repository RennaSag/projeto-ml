# Predição de Salários em Tecnologia

Trabalho da disciplina de Aprendizado de Máquina. O objetivo é construir e comparar modelos de regressão supervisionada capazes de estimar o salário de profissionais de tecnologia com base em características individuais e contextuais.

**Autores:** Leonardo, Mateus, Rennã

---

## Dataset

| Item | Detalhe |
|---|---|
| Nome | Job Salary Prediction Dataset |
| Fonte | Kaggle |
| Link | https://www.kaggle.com/datasets/nalisha/job-salary-prediction-dataset |
| Registros | 250.000 |
| Colunas | 10 |
| Valores nulos | 0 |
| Duplicatas | 0 |

O arquivo CSV deve ser baixado pelo link acima e colocado em `data/job_salary_prediction_dataset.csv`.

---

## Estrutura do Projeto

```
projeto-ml/
│
├── data/                  # Dataset (não versionado)
├── notebooks/             # Notebook principal com EDA e modelos
├── src/                   # Scripts auxiliares (se houver)
├── results/               # Gráficos e outputs gerados
├── requirements.txt       # Dependências
└── README.md
```

---

## Objetivo

Comparar três técnicas de regressão supervisionada para identificar qual minimiza o erro de predição salarial, e identificar quais fatores mais influenciam a remuneração de profissionais de tecnologia.

**Perguntas que guiam o projeto:**
- Quais features mais influenciam o salário?
- Experiência pesa mais que educação?
- Trabalho remoto impacta a remuneração?
- Qual modelo de ML performa melhor?

---

## Técnicas e Modelos

| Modelo | Descrição |
|---|---|
| Regressão Linear | Baseline interpretável; coeficientes indicam o impacto direto de cada variável |
| Random Forest Regressor | Ensemble de árvores de decisão; captura não-linearidades e é robusto a outliers |
| XGBoost Regressor | Gradient boosting; estado da arte em dados tabulares estruturados |

**Configuração do experimento:**
- Divisão: 80% treino / 20% teste
- `random_state = 42`
- `n_estimators = 100` para Random Forest e XGBoost
- Pré-processamento: Label Encoding nas variáveis categóricas

---

## Bibliotecas e Ferramentas

| Biblioteca | Uso |
|---|---|
| pandas | Manipulação de dados |
| numpy | Operações numéricas |
| matplotlib / seaborn | Visualizações |
| scikit-learn | Modelos de ML e métricas |
| xgboost | XGBoost Regressor |

Ambiente: Python 3.x, Jupyter Notebook / Google Colab.

---

## Como Executar

1. Clone o repositório
2. Instale as dependências:
```bash
pip install -r requirements.txt
```
3. Baixe o dataset pelo link acima e coloque em `data/job_salary_prediction_dataset.csv`
4. Abra o notebook em `notebooks/` e execute as células em ordem

---

## Resultados

### Métricas utilizadas

- **RMSE** (Root Mean Squared Error): penaliza erros maiores com mais peso; expresso em USD
- **MAE** (Mean Absolute Error): erro médio absoluto em dólares; mais intuitivo para comunicar precisão
- **R²**: percentual da variância do salário explicada pelo modelo; quanto mais próximo de 1, melhor

### Resultados no conjunto de teste (50.000 registros)

| Modelo | RMSE | MAE | R² |
|---|---|---|---|
| Regressão Linear | $27.498 | $21.741 | 0,4560 |
| Random Forest | $6.557 | $5.184 | 0,9691 |
| **XGBoost** | **$5.568** | **$4.439** | **0,9777** |

### Discussão

O XGBoost obteve o melhor desempenho geral, com RMSE de $5.568 e R² de 0,9777, seguido de perto pelo Random Forest (R² = 0,9691). A Regressão Linear ficou significativamente atrás (R² = 0,4560), o que era esperado dado que as variáveis categóricas foram codificadas com Label Encoding — técnica que introduz ordem artificial em variáveis nominais como `job_title` e `location`, prejudicando modelos lineares.

Os modelos baseados em árvore capturaram bem as não-linearidades do problema. A análise de importância de features do Random Forest indicou que **experiência** e **cargo** são os fatores de maior peso na predição salarial, enquanto certificações e quantidade de skills têm impacto positivo porém menos pronunciado.

O R² elevado nos modelos ensemble é condizente com a natureza do dataset, que é sintético e construído com categorias balanceadas por design — o que tende a facilitar o aprendizado dos modelos.
