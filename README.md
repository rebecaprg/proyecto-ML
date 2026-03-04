# Customer Churn Prediction | Predicción de Abandono de Clientes
## 🇬🇧 English Version
## 📌 Problem Description

In the financial sector, customer churn has a direct impact on revenue and long-term profitability. Acquiring new customers is significantly more expensive than retaining existing ones.

The goal of this project is to develop a Machine Learning model capable of predicting which customers are most likely to leave the company, enabling data-driven retention strategies.

---

## 📂 Dataset

- Public banking customer dataset

- 10,000 records and 14 features

- Includes demographic, financial, and behavioral information

- Target variable: Exited (1 = churn, 0 = retained)

The dataset is publicly available on platforms such as Kaggle (Churn Modelling Dataset).

---

## 🤖 Proposed Solution

The problem is formulated as a binary supervised classification task.

Project workflow:

- Exploratory Data Analysis (EDA)

- Data cleaning and preprocessing

- Categorical encoding

- Feature scaling

- Train/test split

- Model training

- Performance evaluation

Special attention is given to Recall, as correctly identifying customers who are likely to churn is critical from a business perspective.

---

## 🗂️ Repository Structure
```bash
├── src/                # Source directory containing the rest of the folders
│   ├── data_sample/    # Sample data files to allow the code to run
│   ├── img/            # Images used in the project
│   ├── models/         # Saved models in pickle or joblib format
│   ├── notebooks/      # Development and testing notebooks
│   ├── utils/          # Modules, helper functions, or classes created for the project
├── main.ipynb          # Final notebook
├── Presentation.pdf    # Supporting document for the video presentation
├── README.md           # README file summarizing the project
```
---

## 🛠️ Technologies Used

- Python

- Pandas

- NumPy

- Matplotlib / Seaborn

- Scikit-learn

- Jupyter Notebook / VS Code

---

## ▶️ How to Reproduce

### 1️⃣ Clone the repository:

```bash
git clone https://github.com/your-username/your-repository.git
```

### 2️⃣ Install dependencies:

```bash
pip install -r requirements.txt
```

### 3️⃣ Run the notebook:

```bash
jupyter notebook Notebook_FINAL.ipynb
```

---

## 📈 Key Results

A classification model was developed to predict customer churn probability.

Evaluated using Accuracy, Precision, Recall, and F1-score.

The model enables identification of high-risk customers.

From a business perspective, it supports targeted retention strategies and cost optimization.


---
---

## 🇪🇸 Versión en Español

## 📌 Descripción del problema

En el sector financiero, la pérdida de clientes (customer churn) supone un impacto directo en los ingresos y en la rentabilidad de la empresa. Captar nuevos clientes es significativamente más costoso que retener a los existentes.

El objetivo de este proyecto es desarrollar un modelo de Machine Learning capaz de predecir qué clientes tienen mayor probabilidad de abandonar la entidad, permitiendo así implementar estrategias de retención más eficientes y basadas en datos.

---

## 📂 Dataset utilizado

- Dataset público de clientes bancarios.

- Contiene 10.000 registros y 14 variables.

- Incluye información demográfica, financiera y de comportamiento.

- Variable objetivo: Exited (1 = abandona, 0 = permanece).

El dataset puede encontrarse en plataformas públicas como Kaggle (Churn Modelling Dataset).

---

## 🤖 Solución adoptada

El problema se aborda como un problema de clasificación supervisada binaria.

Proceso seguido:

- Análisis exploratorio de datos (EDA)

- Limpieza y preprocesamiento

- Codificación de variables categóricas

- Escalado de variables numéricas

- División en train/test

- Entrenamiento de modelos de clasificación

- Evaluación mediante métricas adecuadas

Se presta especial atención al Recall, ya que en problemas de churn es crítico detectar correctamente a los clientes que realmente van a abandonar.

---

## 🗂️ Estructura del repositorio
```bash
├── src/                # El directorio source que contiene el resto de carpetas
│   ├── data_sample/    # Archivos de datos de muestra que permiten ejecutar el código
│   ├── img/            # Imágenes utilizadas en el proyecto
│   ├── models/         # Modelos guardados en formato pickle o joblib
│   ├── notebooks/      # Notebooks de desarrollo y pruebas
│   ├── utils/          # Módulos, funciones auxiliares o clases creadas para el proyecto
├── main.ipynb          # Notebook final
├── Presentacion.pdf    # Documento soporte de la exposición en vídeo
├── README.md           # Fichero README resumen del proyecto
```

---

## 🛠️ Tecnologías utilizadas

- Python

- Pandas

- NumPy

- Matplotlib / Seaborn

- Scikit-learn

- Jupyter Notebook

---

## ▶️ Instrucciones de reproducción

### 1️⃣ Clonar el repositorio:

```bash
git clone https://github.com/tu-usuario/tu-repositorio.git
```

### 2️⃣ Instalar dependencias:

```bash
pip install -r requirements.txt
```

### 3️⃣ Ejecutar el notebook:

```bash
jupyter notebook Notebook_FINAL.ipynb
```

---

## 📈 Principales resultados

Se construyó un modelo de clasificación capaz de predecir la probabilidad de abandono.

Se evaluó mediante Accuracy, Precision, Recall y F1-score.

El modelo permite identificar clientes con alto riesgo de churn, facilitando estrategias de retención dirigidas.

Desde el punto de vista de negocio, la solución permite optimizar campañas y reducir costes asociados a la pérdida de clientes.

---

## 👤 Autores

Rebeca Prior Garrell – https://github.com/rebecaprg

Jorge Martínez - https://github.com/jorgemd2698

Kelly Escalante - https://github.com/Kelly481
