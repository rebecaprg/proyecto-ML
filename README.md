# 🏦 ML_Churn_Bancario

## Predicción de Bajas de Clientes Bancarios | Bank Customer Churn Prediction

---

## 📋 Descripción / Description

**ES:** Proyecto de Machine Learning para predecir si un cliente bancario abandonará la entidad (`Exited = 1`) o permanecerá (`Exited = 0`). A partir de datos reales de 10.000 clientes de un banco europeo con presencia en Francia, Alemania y España, se construye un pipeline completo de ML: desde la exploración de datos hasta el despliegue del modelo optimizado.

**EN:** Machine Learning project to predict whether a bank customer will churn (`Exited = 1`) or stay (`Exited = 0`). Using real data from 10,000 customers of a European bank operating in France, Germany and Spain, a complete ML pipeline is built: from data exploration to the deployment of the optimized model.

---

## 🎯 Objetivo / Objective

**ES:** Construir un modelo que permita al banco identificar con antelación a los clientes con mayor riesgo de baja, para que el equipo comercial pueda activar campañas de retención personalizadas antes de que la decisión de irse sea irreversible. Retener un cliente cuesta entre 5 y 7 veces menos que captar uno nuevo.

**EN:** Build a model that enables the bank to identify high-risk customers before they leave, allowing the commercial team to activate personalized retention campaigns before the decision to leave becomes irreversible. Retaining a customer costs 5 to 7 times less than acquiring a new one.

---

## 📁 Estructura del Repositorio / Repository Structure

```
ML_Churn_Bancario/
│
├── main.ipynb                          ← Notebook principal — pipeline completo
│
├── src/
│   ├── data_sample/
│   │   └── churn.csv                  ← Dataset (10.000 registros, 14 columnas)
│   │
│   ├── models/
│   │   ├── modelo_churn_bancario.joblib  ← Modelo XGBoost optimizado
│   │   ├── scaler.joblib                 ← StandardScaler ajustado en train
│   │   ├── capping_bounds.joblib         ← Límites P1-P99 para outliers
│   │   └── feature_names.joblib          ← Lista ordenada de features del modelo
│   │
│   └── notebooks/
│       ├── 01_EDA.ipynb               ← Análisis exploratorio de datos
│       ├── 02_Preprocesado.ipynb      ← Preprocesado y feature engineering
│       ├── 03_Modelos.ipynb           ← Modelado y optimización
│       └── 04_Optimizacion.ipynb      ← Optimización adicional de hiperparámetros
│
└── README.md
```

---

## 📊 Dataset

| Característica | Detalle |
|---|---|
| **Fuente** | Kaggle — Bank Customer Churn Dataset |
| **Registros** | 10.000 clientes |
| **Variables originales** | 14 columnas |
| **Variables usadas** | 11 (sin identificadores) |
| **Target** | `Exited` — 1 = abandona, 0 = permanece |
| **Desbalanceo** | ~80% permanece / ~20% abandona |
| **Mercados** | Francia, Alemania, España |

### Variables del Dataset

| Variable | Tipo | Descripción |
|---|---|---|
| `CreditScore` | Numérica | Puntuación crediticia del cliente |
| `Geography` | Categórica | País (France / Germany / Spain) |
| `Gender` | Categórica | Género del cliente |
| `Age` | Numérica | Edad del cliente |
| `Tenure` | Numérica | Años como cliente del banco |
| `Balance` | Numérica | Saldo en cuenta (USD) |
| `NumOfProducts` | Numérica | Nº de productos bancarios contratados |
| `HasCrCard` | Binaria | Tiene tarjeta de crédito (1/0) |
| `IsActiveMember` | Binaria | Es miembro activo (1/0) |
| `EstimatedSalary` | Numérica | Salario estimado anual (USD) |
| `Exited` | **TARGET** | 1 = abandona · 0 = permanece |

---

## 🔧 Pipeline de ML / ML Pipeline

El notebook `main.ipynb` ejecuta los siguientes 16 pasos en orden:

| Paso | Descripción |
|---|---|
| 1 | Importación de librerías |
| 2 | Carga y exploración inicial de datos |
| 3 | División Train / Test (80/20 estratificado) |
| 4 | Análisis de la variable objetivo |
| 5 | EDA univariante |
| 6 | EDA bivariante |
| 7 | Eliminación de features sin valor predictivo |
| 8 | Tratamiento de duplicados |
| 9 | Tratamiento de missings (imputación con mediana) |
| 10 | Detección y tratamiento de outliers (Capping P1-P99) |
| 11 | Feature Engineering + OHE + StandardScaler |
| 12 | Selección de métrica (ROC-AUC) |
| 13 | Baseline + comparativa de 6 modelos con CV-5 |
| 14 | Optimización de hiperparámetros (RandomizedSearchCV) |
| 15 | Evaluación final sobre test |
| 16 | Persistencia del modelo (joblib) |

---

## 🤖 Modelos Evaluados / Models Evaluated

| Modelo | ROC-AUC CV | F1-Score CV |
|---|---|---|
| DummyClassifier (baseline) | ~0.50 | ~0.20 |
| Logistic Regression | — | — |
| Decision Tree | — | — |
| Random Forest | — | — |
| Gradient Boosting | — | — |
| KNN | — | — |
| **XGBoost ✅ Seleccionado** | **~0.87** | **~0.62** |

> Los valores exactos se generan al ejecutar `main.ipynb`

---

## 🏆 Resultados / Results

| Métrica | Valor (Test) |
|---|---|
| **ROC-AUC** | > 0.86 |
| **F1-Score clase 1** | ~0.62 |

**Modelo seleccionado:** XGBoost optimizado con RandomizedSearchCV

**Motivo de la elección:** XGBoost incorpora regularización L1/L2 nativa, maneja el desbalanceo 80/20 mediante `scale_pos_weight` y supera sistemáticamente a los demás algoritmos tras la optimización de hiperparámetros.

**Variables más predictivas:**
- `Age` — clientes de mayor edad tienen más capacidad de comparar y cambiar de banco
- `NumOfProducts` — 3-4 productos generan insatisfacción y sensación de sobrecontratación
- `Balance` — clientes con más saldo son los más fáciles de captar para la competencia
- `IsActiveMember` — la inactividad es la señal más temprana de desenganche
- `balance_per_product` — feature creada en el proyecto que captura la concentración de dinero

---

## ▶️ Cómo Ejecutar / How to Run

**ES:**
1. Clona el repositorio: `git clone https://github.com/[usuario]/ML_Churn_Bancario`
2. Instala las dependencias: `pip install -r requirements.txt`
3. Coloca el dataset en `src/data_sample/churn.csv`
4. Ejecuta `main.ipynb` de principio a fin

**EN:**
1. Clone the repository: `git clone https://github.com/[username]/ML_Churn_Bancario`
2. Install dependencies: `pip install -r requirements.txt`
3. Place the dataset in `src/data_sample/churn.csv`
4. Run `main.ipynb` from top to bottom

---

## 🛠️ Tecnologías / Technologies

![Python](https://img.shields.io/badge/Python-3.10-blue)
![Pandas](https://img.shields.io/badge/Pandas-2.0-green)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-1.3-orange)
![XGBoost](https://img.shields.io/badge/XGBoost-2.0-red)

| Librería | Uso |
|---|---|
| `pandas`, `numpy` | Manipulación de datos |
| `matplotlib`, `seaborn` | Visualización |
| `scikit-learn` | Preprocesado, modelos y métricas |
| `xgboost` | Modelo principal |
| `joblib` | Persistencia del modelo |

---

## 👥 Equipo / Team

| Nombre / Name | Github |
|---|---|
| Kelly Escalante | https://github.com/Kelly481 |
| Jorge Martinez | https://github.com/jorgemd2698 |
| Rebeca Prior| https://github.com/rebecaprg |

---

## 📄 Licencia / License

Este proyecto ha sido desarrollado con fines académicos en el marco del Bootcamp de Data Science.

*This project was developed for academic purposes as part of the Data Science Bootcamp.*
