# SmartMod: LLM-Powered Q&A Moderation

An advanced moderation system that classifies and enhances community Q&A content using large language models (LLMs), feature-rich classifiers, explainability tools, and automated rewriting.

---

## 🚀 Features

- Multi-view learning (BERT, SoftNER, stats)
- XGBoost classification with SHAP interpretability
- T5-based rewriting of low-quality posts
- Automated, iterative improvement loop

---

## 📊 Tech Stack

- 🤖 HuggingFace Transformers: BERT, RoBERTa, Flan-T5
- 📈 XGBoost, SHAP
- 🔍 YAKE, TextBlob, Pandas, Scikit-learn
- 🐍 Python

---

## 📉 Project Flow

```mermaid
graph TD

A[Raw Stack Overflow Dataset (60K)] --> B[Preprocessing: HTML Parsing, Cleaning]
B --> C[Feature Extraction]
C --> C1[Subjective Features (BERT)]
C --> C2[Software Features (SoftNER)]
C --> C3[Statistical Features]

C1 --> D[Feature Engineering]
C2 --> D
C3 --> D

D --> E[XGBoost Classifier]
E --> F[Labels: HQ / LQ_EDIT / LQ_CLOSE]

F --> G{LQ_EDIT?}
G -- Yes --> H[Rewriting via Flan-T5]
H --> I[Reclassification (XGBoost)]
I --> J[Upgraded to HQ (68%)]

F --> K[SHAP Explainability]

J --> R[Results: 96% Precision, 68% LQ_EDIT Upgrade, 50% Less Manual Review]
K --> R

R --> S[Future Work]
S --> S1[Train T5 on real edit histories]
S --> S2[Apply to answers, duplicates]
S --> S3[Add UI/API for real-time use]

S3 --> L[MIT License]
````

---

## ✅ Results

* **96% precision** on LQ\_EDIT classification
* **68% of rewritten** low-quality posts upgraded to high-quality
* **50%+ reduction** in manual moderation workload

---

## 🔭 Future Work

* Use Stack Overflow’s real edit history to improve T5 rewriting
* Extend pipeline to moderate answers and detect duplicates
* Add real-time deployment with UI/API

---

## 📝 License

MIT © 2025 – Vraj Shah, Ayush Dodia, Nithik Pandya

```

Let me know if you want the README downloaded as a file or want to embed charts/images from your SHAP analysis!
```
