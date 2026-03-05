# 🏦 ML_Churn_Bancario

## Predicción de Bajas de Clientes Bancarios | Bank Customer Churn Prediction

---

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.10-blue?logo=python" />
  <img src="https://img.shields.io/badge/Scikit--learn-1.3-orange?logo=scikit-learn" />
  <img src="https://img.shields.io/badge/XGBoost-2.0-red" />
  <img src="https://img.shields.io/badge/ROC--AUC->0.86-brightgreen" />
  <img src="https://img.shields.io/badge/Estado-Completado-success" />
</p>

---

## 🇪🇸 Español

### Descripción

Proyecto de Machine Learning para predecir si un cliente bancario abandonará la entidad (`Exited = 1`) o permanecerá (`Exited = 0`). A partir de datos reales de 10.000 clientes de un banco europeo con presencia en Francia, Alemania y España, se construye un pipeline completo de ML: desde la exploración de datos hasta el modelo optimizado guardado en disco.

### Problema de negocio

Los bancos pierden entre un 15% y 25% de sus clientes cada año. El problema principal no es solo la pérdida, sino que cuando el cliente ya ha tomado la decisión de irse, es demasiado tarde para actuar. **Retener a un cliente cuesta entre 5 y 7 veces menos que captar uno nuevo.**

Este modelo permite identificar con antelación a los clientes con mayor riesgo de baja, para que el equipo comercial pueda activar campañas de retención personalizadas mientras todavía hay tiempo de actuar.

### Planteamiento técnico

Se trata de un problema de **clasificación binaria supervisada**. El target (`Exited`) indica si el cliente abandonó el banco (1) o se quedó (0). El desbalanceo del 80/20 entre clases condiciona la elección de métricas y algoritmos.

---

### 📊 Dataset

| Característica | Detalle |
|---|---|
| **Fuente** | [Kaggle — Bank Customer Churn Dataset](https://www.kaggle.com/datasets/shantanudhakadd/bank-customer-churn-prediction) |
| **Registros** | 10.000 clientes |
| **Variables originales** | 14 columnas |
| **Variables usadas en el modelo** | 11 (sin identificadores: RowNumber, CustomerId, Surname) |
| **Target** | `Exited` — 1 = abandona · 0 = permanece |
| **Distribución del target** | 79.6% permanece / 20.4% abandona |
| **Mercados** | Francia, Alemania, España |

#### Variables del dataset

| Variable | Tipo | Descripción |
|---|---|---|
| `CreditScore` | Numérica | Puntuación crediticia del cliente |
| `Geography` | Categórica | País (France / Germany / Spain) |
| `Gender` | Categórica | Género del cliente |
| `Age` | Numérica | Edad del cliente |
| `Tenure` | Numérica | Años como cliente del banco |
| `Balance` | Numérica | Saldo en cuenta (USD) |
| `NumOfProducts` | Numérica | Número de productos bancarios contratados |
| `HasCrCard` | Binaria | Tiene tarjeta de crédito (1/0) |
| `IsActiveMember` | Binaria | Es miembro activo (1/0) |
| `EstimatedSalary` | Numérica | Salario estimado anual (USD) |
| `Exited` | **TARGET** | 1 = abandona · 0 = permanece |

---

### 🔍 Hallazgos clave del EDA

- **La edad es el predictor más potente**: mediana 45 años (abandona) vs 36 años (permanece)
- **Alemania destaca como mercado crítico**: tasa de abandono del 32.4%, frente al 16.7% de España y 16.2% de Francia
- **Paradoja del saldo alto**: los clientes que se van tienen un saldo medio de 91.109€ vs 72.745€ de los que permanecen — a más dinero, más atractivo para la competencia

---

### 🔧 Pipeline de ML — 16 pasos

El notebook `main.ipynb` ejecuta los siguientes pasos en orden:

| Paso | Descripción |
|---|---|
| 1 | Importación de librerías |
| 2 | Carga y exploración inicial del dataset |
| 3 | División Train / Test (80/20 estratificado) — **siempre primero** |
| 4 | Análisis de la variable objetivo |
| 5 | EDA univariante |
| 6 | EDA bivariante |
| 7 | Eliminación de features sin valor predictivo |
| 8 | Tratamiento de duplicados |
| 9 | Tratamiento de valores faltantes (imputación con mediana) |
| 10 | Detección y tratamiento de outliers (Capping P1-P99) |
| 11 | Feature Engineering + OHE + StandardScaler |
| 12 | Selección de métrica (ROC-AUC) |
| 13 | Baseline + comparativa de 6 modelos con CV-5 |
| 14 | Optimización de hiperparámetros (RandomizedSearchCV) |
| 15 | Evaluación final sobre test |
| 16 | Persistencia del modelo con joblib |

#### Features nuevas creadas en el Feature Engineering

| Feature | Fórmula | Justificación |
|---|---|---|
| `balance_per_product` | `Balance / (NumOfProducts + 1)` | Concentración de dinero por producto — perfil de mayor riesgo |
| `HasBalance` | `Balance > 0 → 1/0` | El 36% tiene saldo 0 — señal de posible desenganche |
| `EngagedCustomer` | `IsActiveMember=1 AND NumOfProducts>1` | Perfil más comprometido con el banco |
| `SalaryAgeRatio` | `EstimatedSalary / (Age + 1)` | Riqueza relativa según etapa de vida |

---

### 🤖 Modelos evaluados

| Modelo | ROC-AUC CV-5 |
|---|---|
| DummyClassifier (baseline) | 0.500 |
| Logistic Regression | 0.762 |
| K-Nearest Neighbors | 0.741 |
| Decision Tree | 0.786 |
| Random Forest | 0.858 |
| Gradient Boosting | 0.871 |
| **XGBoost ✅ Seleccionado** | **0.874** |

**Motivo de la elección de XGBoost:** incorpora regularización L1/L2 nativa, maneja el desbalanceo 80/20 mediante `scale_pos_weight` y ofrece más hiperparámetros de optimización que sus competidores directos.

---

### 🏆 Resultados finales

| Métrica | Valor |
|---|---|
| **ROC-AUC en test** | > 0.86 |
| **F1-Score (clase 1)** | ~ 0.62 |
| **Diferencia CV – Test** | < 0.03 (sin sobreajuste) |

**Impacto de negocio:** de cada 100 clientes que realmente van a abandonar el banco, el modelo identifica a más de 60 antes de que se vayan.

**Variables más predictivas:** Age · NumOfProducts · balance_per_product · IsActiveMember · Balance

---

### ▶️ Cómo ejecutar el proyecto

```bash
# 1. Clona el repositorio
git clone https://github.com/Kelly481/ML_Churn_Bancario

# 2. Instala las dependencias
pip install -r requirements.txt

# 3. Coloca el dataset en la ruta correcta
# src/data_sample/churn.csv

# 4. Ejecuta el notebook principal de principio a fin
jupyter notebook main.ipynb
```

---

### 🛠️ Tecnologías utilizadas

| Librería | Versión | Uso |
|---|---|---|
| `pandas` | 2.0+ | Manipulación y análisis de datos |
| `numpy` | 1.24+ | Operaciones numéricas |
| `matplotlib` | 3.7+ | Visualización |
| `seaborn` | 0.12+ | Visualización estadística |
| `scikit-learn` | 1.3+ | Preprocesado, modelos y métricas |
| `xgboost` | 2.0+ | Modelo principal |
| `joblib` | 1.3+ | Persistencia del modelo |

---

### 📁 Estructura del repositorio

```
ML_Churn_Bancario/
│
├── main.ipynb                           ← Notebook principal — pipeline completo ejecutable
├── Presentacion.pdf                     ← Diapositivas del vídeo de presentación
├── README.md                            ← Este archivo
│
└── src/
    ├── data_sample/
    │   └── churn.csv                    ← Dataset (10.000 registros, 14 columnas)
    │
    ├── img/                             ← Gráficos e imágenes del proyecto
    │
    ├── models/
    │   ├── modelo_churn_bancario.joblib ← Modelo XGBoost optimizado
    │   ├── scaler.joblib                ← StandardScaler ajustado en train
    │   ├── capping_bounds.joblib        ← Límites P1-P99 para outliers
    │   └── feature_names.joblib         ← Lista ordenada de features del modelo
    │
    ├── notebooks/
    │   ├── 01_EDA.ipynb                 ← Análisis exploratorio de datos
    │   ├── 02_Preprocesado.ipynb        ← Preprocesado y feature engineering
    │   ├── 03_Modelos.ipynb             ← Modelado y comparativa
    │   └── 04_Optimizacion.ipynb        ← Optimización de hiperparámetros
    │
    └── utils/                           ← Funciones auxiliares
```

---

### 👥 Equipo

| Nombre | GitHub |
|---|---|
| Kelly Escalante | [@Kelly481](https://github.com/Kelly481) |
| Jorge Martínez | [@jorgemd2698](https://github.com/jorgemd2698) |
| Rebeca Prior | [@rebecaprg](https://github.com/rebecaprg) |

---

---

## 🇬🇧 English

### Description

Machine Learning project to predict whether a bank customer will churn (`Exited = 1`) or stay (`Exited = 0`). Using real data from 10,000 customers of a European bank operating in France, Germany and Spain, a complete ML pipeline is built: from data exploration to the optimized model saved to disk.

### Business Problem

Banks lose between 15% and 25% of their customers every year. The main issue is not just the loss itself, but that by the time a customer has decided to leave, it is already too late to act. **Retaining a customer costs 5 to 7 times less than acquiring a new one.**

This model enables the bank to identify at-risk customers in advance, so the commercial team can launch personalized retention campaigns while there is still time to act.

### Technical Approach

This is a **supervised binary classification** problem. The target (`Exited`) indicates whether the customer churned (1) or stayed (0). The 80/20 class imbalance drives the choice of metrics and algorithms.

---

### 📊 Dataset

| Feature | Detail |
|---|---|
| **Source** | [Kaggle — Bank Customer Churn Dataset](https://www.kaggle.com/datasets/shantanudhakadd/bank-customer-churn-prediction) |
| **Records** | 10,000 customers |
| **Original variables** | 14 columns |
| **Variables used in the model** | 11 (without identifiers: RowNumber, CustomerId, Surname) |
| **Target** | `Exited` — 1 = churns · 0 = stays |
| **Target distribution** | 79.6% stays / 20.4% churns |
| **Markets** | France, Germany, Spain |

#### Dataset variables

| Variable | Type | Description |
|---|---|---|
| `CreditScore` | Numeric | Customer credit score |
| `Geography` | Categorical | Country (France / Germany / Spain) |
| `Gender` | Categorical | Customer gender |
| `Age` | Numeric | Customer age |
| `Tenure` | Numeric | Years as a bank customer |
| `Balance` | Numeric | Account balance (USD) |
| `NumOfProducts` | Numeric | Number of banking products contracted |
| `HasCrCard` | Binary | Has credit card (1/0) |
| `IsActiveMember` | Binary | Is active member (1/0) |
| `EstimatedSalary` | Numeric | Estimated annual salary (USD) |
| `Exited` | **TARGET** | 1 = churns · 0 = stays |

---

### 🔍 Key EDA Findings

- **Age is the strongest predictor**: median 45 years (churners) vs 36 years (stayers)
- **Germany stands out as a critical market**: churn rate of 32.4%, compared to 16.7% in Spain and 16.2% in France
- **High balance paradox**: churners have an average balance of €91,109 vs €72,745 for stayers — the more money a customer has, the more attractive they are to the competition

---

### 🔧 ML Pipeline — 16 steps

The `main.ipynb` notebook runs the following steps in order:

| Step | Description |
|---|---|
| 1 | Library imports |
| 2 | Dataset loading and initial exploration |
| 3 | Train / Test split (80/20 stratified) — **always first** |
| 4 | Target variable analysis |
| 5 | Univariate EDA |
| 6 | Bivariate EDA |
| 7 | Removal of features with no predictive value |
| 8 | Duplicate handling |
| 9 | Missing value treatment (median imputation) |
| 10 | Outlier detection and treatment (P1-P99 Capping) |
| 11 | Feature Engineering + OHE + StandardScaler |
| 12 | Metric selection (ROC-AUC) |
| 13 | Baseline + comparison of 6 models with CV-5 |
| 14 | Hyperparameter optimization (RandomizedSearchCV) |
| 15 | Final evaluation on test set |
| 16 | Model persistence with joblib |

#### New engineered features

| Feature | Formula | Rationale |
|---|---|---|
| `balance_per_product` | `Balance / (NumOfProducts + 1)` | Money concentration per product — highest risk profile |
| `HasBalance` | `Balance > 0 → 1/0` | 36% have zero balance — potential disengagement signal |
| `EngagedCustomer` | `IsActiveMember=1 AND NumOfProducts>1` | Most committed customer profile |
| `SalaryAgeRatio` | `EstimatedSalary / (Age + 1)` | Relative wealth by life stage |

---

### 🤖 Models evaluated

| Model | ROC-AUC CV-5 |
|---|---|
| DummyClassifier (baseline) | 0.500 |
| Logistic Regression | 0.762 |
| K-Nearest Neighbors | 0.741 |
| Decision Tree | 0.786 |
| Random Forest | 0.858 |
| Gradient Boosting | 0.871 |
| **XGBoost ✅ Selected** | **0.874** |

**Why XGBoost:** it includes native L1/L2 regularization, handles the 80/20 imbalance through `scale_pos_weight`, and offers more hyperparameters for tuning than its closest competitors.

---

### 🏆 Final Results

| Metric | Value |
|---|---|
| **ROC-AUC on test** | > 0.86 |
| **F1-Score (class 1)** | ~ 0.62 |
| **CV – Test difference** | < 0.03 (no overfitting) |

**Business impact:** out of every 100 customers who will actually churn, the model identifies more than 60 before they leave.

**Top predictive variables:** Age · NumOfProducts · balance_per_product · IsActiveMember · Balance

---

### ▶️ How to run the project

```bash
# 1. Clone the repository
git clone https://github.com/Kelly481/ML_Churn_Bancario

# 2. Install dependencies
pip install -r requirements.txt

# 3. Place the dataset in the correct path
# src/data_sample/churn.csv

# 4. Run the main notebook from top to bottom
jupyter notebook main.ipynb
```

---

### 🛠️ Technologies used

| Library | Version | Use |
|---|---|---|
| `pandas` | 2.0+ | Data manipulation and analysis |
| `numpy` | 1.24+ | Numerical operations |
| `matplotlib` | 3.7+ | Visualization |
| `seaborn` | 0.12+ | Statistical visualization |
| `scikit-learn` | 1.3+ | Preprocessing, models and metrics |
| `xgboost` | 2.0+ | Main model |
| `joblib` | 1.3+ | Model persistence |

---

### 📁 Repository structure

```
ML_Churn_Bancario/
│
├── main.ipynb                           ← Main notebook — complete executable pipeline
├── Presentacion.pdf                     ← Video presentation slides
├── README.md                            ← This file
│
└── src/
    ├── data_sample/
    │   └── churn.csv                    ← Dataset (10,000 records, 14 columns)
    │
    ├── img/                             ← Project charts and images
    │
    ├── models/
    │   ├── modelo_churn_bancario.joblib ← Optimized XGBoost model
    │   ├── scaler.joblib                ← StandardScaler fitted on train
    │   ├── capping_bounds.joblib        ← P1-P99 outlier bounds
    │   └── feature_names.joblib         ← Ordered list of model features
    │
    ├── notebooks/
    │   ├── 01_EDA.ipynb                 ← Exploratory data analysis
    │   ├── 02_Preprocesado.ipynb        ← Preprocessing and feature engineering
    │   ├── 03_Modelos.ipynb             ← Modelling and model comparison
    │   └── 04_Optimizacion.ipynb        ← Hyperparameter optimization
    │
    └── utils/                           ← Helper functions
```

---

### 👥 Team

| Name | GitHub |
|---|---|
| Kelly Escalante | [@Kelly481](https://github.com/Kelly481) |
| Jorge Martínez | [@jorgemd2698](https://github.com/jorgemd2698) |
| Rebeca Prior | [@rebecaprg](https://github.com/rebecaprg) |

---

*Proyecto Final — Bootcamp de Data Science · Marzo 2025*  
*Final Project — Data Science Bootcamp · March 2025*
