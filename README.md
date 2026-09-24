# Student Exam Performance Predictor

End-to-end machine learning project that predicts a student’s **math exam score** from demographic and academic features. The solution covers data ingestion, preprocessing, model training, evaluation, and a Flask web app for real-time inference.

---

## Overview

Given inputs such as gender, race/ethnicity, parental education level, lunch type, test preparation course, and reading/writing scores, the trained regressor estimates the student’s **math score on a 0–100 scale**.

This project is designed as a production-oriented ML workflow:

- Modular training pipeline (`ingestion → transformation → training`)
- Serialized artifacts (`model.pkl`, `preprocessor.pkl`) for inference
- Flask UI for interactive predictions
- Docker packaging and CI/CD toward AWS (ECR / Elastic Beanstalk)

---

## Features

- **Regression models**: Random Forest, Gradient Boosting, XGBoost, CatBoost, AdaBoost, Linear Regression, and Decision Tree, with hyperparameter search
- **Preprocessing**: numeric scaling and categorical one-hot encoding via scikit-learn pipelines
- **Web inference**: form-based prediction at `/predictdata`
- **Reproducible environment**: Python 3.10, Docker, and GitHub Actions

---

## Tech Stack

| Layer | Tools |
| --- | --- |
| Language | Python 3.10 |
| ML | scikit-learn, XGBoost, CatBoost |
| Data | pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| API / UI | Flask, Jinja templates |
| Packaging | Docker |
| Cloud / CI | AWS ECR, Elastic Beanstalk, GitHub Actions |

---

## Project Structure

```text
ML_project/
├── app.py                      # Flask entrypoint (local / Docker)
├── application.py              # Alternate entrypoint (e.g. Elastic Beanstalk)
├── artifacts/                  # Trained model & preprocessor
├── notebook/                   # EDA and model experimentation
├── src/
│   ├── components/             # Ingestion, transformation, training
│   ├── pipeline/               # Prediction pipeline
│   ├── exception.py
│   ├── logger.py
│   └── utils.py
├── templates/                  # HTML templates
├── Dockerfile
├── requirements.txt
└── .github/workflows/          # CI/CD
```

---

## Getting Started

### Prerequisites

- Python **3.10**
- `pip` (or Conda)
- Optional: Docker Desktop

### Installation

```bash
# Create and activate a virtual environment
python3.10 -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

Using Conda:

```bash
conda create -p venv python=3.10 -y
conda activate ./venv
pip install -r requirements.txt
```

---

## Training the Model

Run the training pipeline from the project root (after installing dependencies):

```bash
python src/components/data_ingestion.py
```

This will:

1. Load and split the dataset
2. Fit the preprocessor and transform features
3. Train and select the best regressor
4. Persist artifacts under `artifacts/`

---

## Running the Web App

```bash
python app.py
```

Then open:

| URL | Description |
| --- | --- |
| http://localhost:8001/ | Landing page |
| http://localhost:8001/predictdata | Prediction form |

> The Flask app listens on port **8001** by default.

### Input features

| Feature | Type | Description |
| --- | --- | --- |
| `gender` | categorical | male / female |
| `race_ethnicity` | categorical | group A–E |
| `parental_level_of_education` | categorical | e.g. high school, bachelor's, master's |
| `lunch` | categorical | standard / free/reduced |
| `test_preparation_course` | categorical | none / completed |
| `reading_score` | numeric | 0–100 |
| `writing_score` | numeric | 0–100 |

**Output:** predicted `math_score` (continuous value on a 0–100 scale).

---

## Docker

Build the image:

```bash
docker build -t ml_project .
```

Run the container (map host `8001` → container `8001`):

```bash
docker run -d -p 8001:8001 ml_project
```

Open http://localhost:8001/predictdata

---

## How Prediction Works

1. The form submits student features to `/predictdata`
2. `CustomData` builds a single-row DataFrame
3. The saved preprocessor transforms the row
4. The trained model returns the predicted math score

Reading and writing scores are typically the strongest predictors; categorical factors (including parental education) contribute less once academic scores are known.

---

## License

This project is provided for educational and portfolio purposes.
