
# 🏥 Medical Readmission Prediction and Clinical NER

It combines structured predictive modelling to predict medical readmission and unstructured text extraction for clinical insights.

---

## Project Overview

The project addresses two major tasks:

1. **Predicting 30-day hospital readmission** using structured EHR data.  
2. **Extracting key clinical entities** (Diagnosis, Treatment, Symptoms, Medications, Follow-up Actions) from free-text discharge notes

---

## Repository Structure

```
├── medical_readmission_prediction.ipynb  # Binary classification modelling (Task 1)
├── NER_LLM_spaCy.ipynb                   # Extracting key clinical entities (Task 2)
├── output_spacy.csv                      # spaCy rule-based NER results
├── output_llm.xlsx                       # LLM (Phi-3-mini) NER results
├── Report.pdf                            # 1 page project summary
├── README.md                             # Project documentation

````
---

## Tasks and Approach

### 1. Predictive Modelling

**Goal:** Predict whether a patient will be readmitted within 30 days.

**Approach:**

* Performed data cleaning, outlier handling, and encoding on the **training set only** to avoid leakage.
* Designed reusable **feature engineering functions** for modularity and repeatability.
* Created **one-level and two-level history features** via target encoding to capture past readmission tendencies.
* Addressed **class imbalance** using class weights in all models.
* Trained multiple classifiers: **Logistic Regression, Decision Tree, Random Forest, LightGBM, XGBoost**.
* Evaluated with **ROC-AUC, PR-AUC, F1-score, and Brier score**.
* Built a **threshold-based performance table** showing how precision, recall, F1, and % of patients flagged change with different probability cutoffs.
* Threshold optimization was done on the validation set to support **clinical calibration** between hospital admins and physicians.
* Emphasized **minimizing false negatives** (to avoid missing high-risk patients).
* Used **logistic regression coefficients** for feature interpretability.

**Best Model:** Logistic Regression

* ROC-AUC = **0.62**, F1 = **0.55**
* Top predictors: *prior admissions* and *diagnosis × medication complexity*, aligning with medical intuition.

---

### 2. Named Entity Recognition (NER)

#### **LLM-based Approach (Phi-3-mini)**

* Implemented with **LangChain + HuggingFace pipeline** for structured JSON extraction.
* Extracted entities: Diagnosis, Treatment, Symptoms, Medications, Follow-up Actions.
* Captured richer contextual details and relations in text.

#### **Rule-based Approach (spaCy)**

* Created **pattern-based matchers** for diagnosis, treatment, symptoms, medications, and follow-up.
* Provides deterministic, high-precision extraction with interpretable patterns.

| Method               | Strength                          | Limitation                        |
| -------------------- | --------------------------------- | --------------------------------- |
| **LLM (Phi-3-mini)** | Contextual, richer coverage       | May hallucinate or overgeneralize |
| **spaCy Rules**      | High precision, transparent rules | Narrow domain coverage            |

---

## Key Results

| Task                   | Metric           | Result                  |
| ---------------------- | ---------------- | ----------------------- |
| Readmission Prediction | ROC-AUC          | **0.62**                |
| Readmission Prediction | F1-Score         | **0.55**                |
| NER (LLM)              | Broader coverage | Context-rich extraction |
| NER (spaCy)            | High precision   | Rule-dependent          |

---

## Future Work

* Add **k-fold cross-validation** and richer clinical variables.
* Fine-tune **domain-specific LLMs** for improved accuracy.
* Introduce **cost-based thresholding** to balance workload and risk.
* Combining probability and age supports personalized risk based follow up. 
  * For example, a patient aged >70 years with >=80 % predicted readmission risk could trigger a more intensive follow up plan than younger, lower risk individuals.
* Create a **gold-standard labeled subset** of discharge notes for evaluation.
* Build a **hybrid NER pipeline**: use spaCy for anchors and LLM for contextual enrichment.

---

## Author

**Sailesh Dwivedy**
📧 [[sdwivedy30@gmail.com](mailto:sdwivedy30@gmail.com)]

