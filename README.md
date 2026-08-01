# Mental-Health-Score-Prediction
# LIVE LINK: https://mental-health-score-prediction-3-bvjs.onrender.com/

# **🧠 Mental Health Score Predictor**

A machine learning web app that predicts a student's **Mental Health Score** from their social media habits, study time, sleep, physical activity, and stress level — trained in scikit-learn, served via FastAPI, and deployed as a live web app.

> ⚠️ **Disclaimer:** This project is a data science / ML engineering demo built on a synthetic/survey-style dataset. It is **not** a clinically validated psychometric tool and should not be used for medical or diagnostic purposes.

---

## **📌 Overview**

Excessive screen time, poor sleep, and high stress are commonly linked to declining student mental well-being. This project explores that relationship end-to-end:

1. **Analyze** a dataset of ~5,000 students' lifestyle and social media habits
2. **Train** a regression model to predict a continuous Mental Health Score (~3–10)
3. **Serve** the model behind a validated REST API
4. **Deliver** predictions through a simple web form

---

## **🚀 Demo**

Fill in a few lifestyle details (screen time, sleep, study hours, stress level, etc.) and get an instant predicted mental health score.

> ⏳ Note: if hosted on Render's free tier, the app may take 30–50 seconds to wake up on the first request after a period of inactivity.

---

## **🏗️ Architecture**

```
Dataset (CSV)
   │
   ▼
EDA + Cleaning + Feature Engineering  (Jupyter Notebook)
   │
   ▼
ColumnTransformer (impute → scale → encode) + Model
   │  wrapped together in a single sklearn Pipeline
   ▼
joblib.dump() → model.pkl
   │
   ▼
FastAPI app
   ├── loads model.pkl once at startup
   ├── Pydantic request/response validation
   ├── POST /predict
   ├── GET  /health
   └── serves static frontend (same origin → no CORS needed)
   │
   ▼
Browser (HTML/CSS/JS) → fetch() → JSON response → rendered on page
```

---

## **🛠️ Tech Stack**

| Layer | Tools |
|---|---|
| Data & Modeling | Python, pandas, numpy, scikit-learn |
| Visualization | matplotlib, seaborn |
| Model Persistence | joblib |
| Backend / API | FastAPI, Pydantic, Uvicorn |
| Frontend | HTML, CSS, JavaScript (vanilla, no framework) |
| Deployment | Render |

---

## **📊 Dataset**

**File:** `Student_Social_Media_And_Mental_Health_Impact.csv`

~5,000 rows covering:

- **Demographics:** Age, Gender, Country
- **Platform usage:** Avg_Daily_Usage_Hours, Daily_Unlocks, Most_Used_Platform
- **Lifestyle:** Study_Hours, Sleep_Hours_Per_Night, Physical_Activity_Hours
- **Psychological:** Stress_Level
- **Target:** Mental_Health_Score

---

## **🔬 ML Pipeline**

- Exploratory Data Analysis — target distribution, correlation heatmap, key feature relationships
- Data cleaning — removed duplicate rows, fixed an invalid negative value
- Skewness check → log-transformed the one skewed numeric column
- Feature engineering — grouped a high-cardinality categorical column into top categories + "Other"
- Encoding — ordinal encoding for ordered categories, one-hot encoding for unordered categories
- Train/test split performed **before** preprocessing to prevent data leakage
- Preprocessing built with `ColumnTransformer`, combined with the model in a single `Pipeline`
- Hyperparameter tuning via `RandomizedSearchCV`
- Model evaluation with standard regression metrics
- Feature importance check to confirm the model learned sensible, intuitive patterns
- Final pipeline (preprocessing + model) persisted with `joblib`

---

## **📁 Project Structure**

```
Mental-Health-Score/
├── ML_Project.ipynb                                          # EDA, preprocessing, model training
├── Student Social Media And Mental Health Impact.csv          # Dataset
├── ML Project.html                                            # Project build blueprint / notes
├── app/ (or main.py)                                          # FastAPI application
│   ├── model.pkl                                              # Saved sklearn pipeline
│   ├── schemas.py                                              # Pydantic request/response models
│   └── static/                                                 # index.html, style.css, script.js
├── requirements.txt
└── README.md
```

*(Adjust the above to match your actual file/folder layout.)*

---

## **⚙️ Getting Started**

### **Prerequisites**
- Python 3.9+
- pip

### **Installation**

```bash
git clone https://github.com/tanishq-latent/Mental-Health-Score.git
cd Mental-Health-Score
pip install -r requirements.txt
```

### **Run locally**

```bash
uvicorn main:app --reload
```

Then open **http://127.0.0.1:8000** in your browser.

### **Retrain the model (optional)**

Open `ML_Project.ipynb` in Jupyter, run all cells to reproduce EDA, training, and evaluation, and re-save `model.pkl` with `joblib.dump()`.

---

## **🔌 API Reference**

### **`POST /predict`**

Accepts a JSON payload of lifestyle features and returns a predicted mental health score.

**Request body:**
```json
{
  "Age": 21,
  "Gender": "Female",
  "Country": "India",
  "Avg_Daily_Usage_Hours": 4.5,
  "Daily_Unlocks": 60,
  "Most_Used_Platform": "Instagram",
  "Study_Hours": 3,
  "Sleep_Hours_Per_Night": 6,
  "Physical_Activity_Hours": 1,
  "Stress_Level": "High"
}
```

**Response:**
```json
{
  "Mental_Health_Score": 6.2
}
```

Invalid input (e.g., negative age) automatically returns an HTTP `422` with a detailed validation error, thanks to Pydantic.

### **`GET /health`**

Simple uptime check — returns `200 OK`.

Interactive API docs are auto-generated by FastAPI at **`/docs`**.

---

## **🚢 Deployment (Render)**

1. Push the repo to GitHub
2. Create a new **Web Service** on [Render](https://render.com) and connect the repo
3. **Build command:** `pip install -r requirements.txt`
4. **Start command:** `uvicorn main:app --host 0.0.0.0 --port $PORT`

---

## **🧩 Key Design Decisions**

- **Single sklearn `Pipeline`** (preprocessing + model) is saved as one artifact — the API never re-implements feature engineering, avoiding training/serving skew.
- **Frontend and API served from the same FastAPI app** — same-origin requests, so no CORS configuration is needed.
- **Pydantic validation** rejects malformed input before it ever reaches the model.

---

## **🔭 Future Improvements**

- [ ] Try gradient boosting models (XGBoost / LightGBM) and compare performance
- [ ] Add SHAP-based explainability for individual predictions
- [ ] Add automated tests (pytest) for the pipeline and API routes
- [ ] Add CI/CD for automatic retraining and redeployment
- [ ] Add request logging and basic monitoring
- [ ] Fairness checks across demographic subgroups
- [ ] Track scores over time for a returning user instead of single-shot predictions

---

## **⚠️ Limitations**

- Dataset is not a clinically validated psychometric survey — treat predictions as a wellness indicator, not a diagnosis.
- Correlational, not causal — the model reflects patterns in this dataset, not proven cause-effect relationships.
- No authentication, database, or user history layer — a single-model inference service.

---

## **📄 License**

Add your preferred license here (e.g., MIT).

---

