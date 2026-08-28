# Day 1: ML Environment Setup & Baseline Model

## 📌 Overview
This directory contains the Day 1 task for the AI/ML Internship. The primary focus was setting up the Python machine learning workspace, understanding the standard ML workflow (**Data $\rightarrow$ Model $\rightarrow$ Evaluation**), and training a baseline classifier on the built-in Iris dataset.

---

## 🛠️ Tech Stack & Dependencies
* **Language:** Python 3.12
* **IDE:** PyCharm
* **Libraries:** `scikit-learn`, `pandas`, `numpy`

---

## 🚀 Model Details & Workflow
1. **Dataset:** Iris dataset (150 samples across 3 flower species).
2. **Preprocessing:** Split data into **80% Training** and **20% Testing** sets using `train_test_split(random_state=42)` for strict reproducibility.
3. **Algorithm:** Decision Tree Classifier (`DecisionTreeClassifier`).
4. **Evaluation:** Measured predictive performance using `accuracy_score`.

---

## 📊 Key Results
* **Training Status:** Successfully completed without errors.
* **Model Accuracy:** `93.33%` on unseen test data.
