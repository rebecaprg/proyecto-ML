# Customer Churn Prediction | Predicción de Abandono de Clientes
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
├── Notebook_FINAL.ipynb
├── README.md
└── data/ (optional)
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
