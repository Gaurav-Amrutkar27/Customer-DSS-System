# Customer Segmentation Decision Support System

A full-stack **Customer Segmentation Decision Support System (DSS)** that uses machine learning to analyze customer purchasing behavior, identify behavioral customer segments, visualize segment-level analytics, and predict the segment of a new customer based on their first-order purchase pattern.

The project combines a **Python/FastAPI machine-learning backend** with a **React/Vite frontend dashboard** to provide an interactive interface for customer segmentation and prediction.

---

## 📌 Project Overview

Customer segmentation is the process of dividing customers into meaningful groups based on similarities in their behavior.

This project focuses on **first-order purchasing behavior** and uses engineered behavioral features to create customer profiles.

The system has two major machine-learning stages:

1. **Unsupervised segmentation**

   * Customer behavior is transformed into numerical features.
   * A preprocessing pipeline is applied.
   * K-Means clustering is used to generate customer segment assignments.

2. **Supervised new-customer prediction**

   * The frozen K-Means cluster assignments are treated as segment labels.
   * Classification models are evaluated.
   * The deployed classifier predicts the segment of a new customer from their first-order behavior.

The resulting models and processed data are exposed through a FastAPI backend and consumed by a React dashboard.

---

## ✨ Key Features

### 1. Customer Segmentation

The system organizes customers into four behavioral segments based on first-order purchasing characteristics:

* **Multi-Category Basket**
* **Higher-Value Single-Item**
* **Lower-Value Single-Item**
* **Larger Single-Category Basket**

The segment names are application-level interpretations of the frozen cluster IDs.

---

### 2. Customer Overview Dashboard

The frontend provides an overview of the existing customer profiles, including:

* Total customer profiles
* Number of customer segments
* Segment distribution
* Percentage of customers in each segment
* Segment-level behavioral profiles

The dashboard retrieves this information from the FastAPI backend.

---

### 3. Segment Analytics

The analytics section allows users to compare customer segments using behavioral statistics such as:

* Average order value
* Average number of items
* Average number of categories
* Freight ratio
* Unique products
* Unique categories
* Average item price
* Average installments

Charts and tables are used to make the differences between segments easier to understand.

---

### 4. New Customer Prediction

The system provides a dedicated **New Customer** interface.

A user can enter a customer's first-order information:

| Input             | Description                           |
| ----------------- | ------------------------------------- |
| Product Spend     | Amount spent on products              |
| Freight Value     | Shipping/freight cost                 |
| Number of Items   | Total items in the order              |
| Unique Products   | Number of distinct products           |
| Unique Categories | Number of distinct product categories |
| Installments      | Number of payment installments        |

The application derives additional features automatically.

### Derived Features

The backend calculates:

* `order_value`
* `freight_ratio`
* `avg_item_price`

These features, together with the supplied behavioral inputs, form the model input.

---

### 5. Prediction Probabilities

The prediction API returns:

* Predicted cluster
* Predicted segment name
* Class probabilities
* Derived features

This allows the frontend to show not only the predicted segment but also the model's probability distribution across the available segments.

---

## 🧠 Machine Learning Pipeline

The machine-learning workflow consists of the following stages:

```text
Customer Transaction Data
          │
          ▼
Data Processing / Feature Engineering
          │
          ▼
Customer First-Order Features
          │
          ▼
Saved Preprocessing Pipeline
          │
          ▼
Transformed Feature Matrix
          │
          ├───────────────┐
          ▼               ▼
      K-Means       Classification
      Clustering       Models
          │               │
          ▼               ▼
   Segment Labels    Logistic Regression
                     Random Forest
                          │
                          ▼
                  Final Prediction API
```

---

## 📊 Model Features

The deployed prediction pipeline uses the following seven behavioral features:

```text
order_value
freight_ratio
item_count
unique_product_count
unique_category_count
avg_item_price
avg_installments
```

These features describe the monetary value, basket size, product diversity, category diversity, pricing behavior, shipping contribution, and installment behavior of a customer's first order.

---

## 🔬 Clustering

The project uses **K-Means clustering** to establish the original customer segments.

The training workflow loads the saved preprocessing pipeline and final K-Means model, transforms the engineered customer features, and obtains the cluster assignments.

The cluster IDs are then used as the target labels for the supervised classification stage.

The project therefore follows:

```text
Behavioral Features
        ↓
Preprocessing
        ↓
K-Means Clustering
        ↓
Customer Segment Labels
        ↓
Supervised Classification
        ↓
New Customer Segment Prediction
```

---

## 🤖 Classification

Two classification approaches were evaluated during the model experimentation stage:

### Logistic Regression

Configured with:

* `max_iter = 2000`
* `class_weight = "balanced"`
* `random_state = 42`

### Random Forest

Configured with:

* `n_estimators = 300`
* `min_samples_split = 5`
* `min_samples_leaf = 2`
* `class_weight = "balanced_subsample"`
* `random_state = 42`

The models are evaluated using:

* Accuracy
* Balanced Accuracy
* Macro Precision
* Macro Recall
* Macro F1
* Weighted F1
* Multiclass ROC-AUC
* Classification Report
* Confusion Matrix

The training workflow also uses **5-fold Stratified Cross-Validation**.

The final application uses the saved classifier artifact rather than retraining the model every time the application starts.

---

## 🗂️ Project Structure

The project is organized into separate frontend, backend, data, and model layers.

A representative structure is:

```text
CUSTOMER-SEGMENTATION-DSS/
│
├── backend/
│   ├── api/
│   │   └── routes/
│   │       ├── analytics.py
│   │       ├── dashboard.py
│   │       ├── predictions.py
│   │       └── segments.py
│   │
│   ├── services/
│   │   ├── dashboard_service.py
│   │   └── prediction_service.py
│   │
│   └── main.py
│
├── data/
│   ├── raw/
│   │
│   └── processed/
│       ├── customer_features.csv
│       ├── final_cluster_assignments.csv
│       ├── classifier_comparison.csv
│       └── random_forest_feature_importance.csv
│
├── models/
│   ├── preprocessing/
│   │   └── customer_preprocessor.joblib
│   │
│   ├── clustering/
│   │   └── kmeans_final.joblib
│   │
│   ├── classifier/
│   │   ├── logistic_regression.joblib
│   │   └── random_forest.joblib
│   │
│   └── metadata/
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Overview.jsx
│   │   │   ├── Prediction.jsx
│   │   │   └── Analytics.jsx
│   │   │
│   │   ├── App.jsx
│   │   └── App.css
│   │
│   ├── package.json
│   ├── package-lock.json
│   ├── vite.config.js
│   └── index.html
│
├── scripts/
│   └── model training / experimentation scripts
│
└── README.md
```

> The exact filenames may differ depending on the version of the project uploaded to GitHub. The important architectural separation is between the **frontend**, **FastAPI backend**, **processed data**, and **saved ML artifacts**.

---

## 🖥️ Frontend

The frontend is built using:

* **React**
* **Vite**
* **Recharts**
* **Axios / Fetch API**
* CSS

The main application contains three views:

```text
Overview
   │
   ├── Customer profile statistics
   ├── Segment distribution
   └── Segment profiles

New Customer
   │
   ├── First-order input form
   ├── Feature derivation
   ├── Segment prediction
   └── Prediction probabilities

Analytics
   │
   ├── Segment comparisons
   ├── Charts
   └── Detailed segment table
```

The main React application switches between these views using the navigation provided in `App.jsx`.

---

## ⚙️ Backend

The backend is implemented using **FastAPI**.

The API is responsible for:

* Loading saved ML models
* Loading processed customer data
* Validating prediction inputs
* Generating customer predictions
* Providing dashboard statistics
* Providing segment profiles
* Providing analytical metrics

The FastAPI application registers separate routers for:

```text
Predictions
Dashboard
Segments
Analytics
```

It also provides basic endpoints such as:

```text
GET /
GET /health
```

The health endpoint can be used to verify that the backend is running.

---

## 🔌 API Endpoints

The frontend communicates with the backend using endpoints under `/api`.

### Prediction

```text
POST /api/predictions/predict
```

Used to predict the segment of a new customer.

Example request:

```json
{
  "product_spend": 500,
  "freight_value": 50,
  "item_count": 2,
  "unique_product_count": 2,
  "unique_category_count": 1,
  "installments": 3
}
```

The API returns the predicted cluster, segment, probabilities, and derived features.

---

### Dashboard Overview

```text
GET /api/dashboard/overview
```

Provides:

* Total customer profiles
* Number of segments
* Customer count per segment
* Segment percentages

---

### Segment Profiles

```text
GET /api/segments
```

Returns behavioral profiles and summary statistics for the customer segments.

---

### Analytics

The analytics routes provide the aggregated information required by the frontend to display segment-level comparisons and charts.

---

## 📦 Data

The application operates on processed customer-level feature data.

The main processed files include:

### `customer_features.csv`

Contains the engineered behavioral features used by the ML pipeline.

Important columns include:

```text
order_value
freight_ratio
item_count
unique_product_count
unique_category_count
avg_item_price
avg_installments
```

### `final_cluster_assignments.csv`

Contains the transformed feature representation together with the frozen K-Means cluster assignments.

The backend validates the relationship between the feature data and cluster assignment data before using them together.

This validation helps prevent unsafe positional alignment if the two files become inconsistent.

---

## 🧩 Saved Model Artifacts

The project stores trained model artifacts using `joblib`.

### Preprocessor

```text
models/preprocessing/customer_preprocessor.joblib
```

The same preprocessing pipeline used during model development is reused during prediction.

### K-Means Model

```text
models/clustering/kmeans_final.joblib
```

Used to establish the original customer segment assignments.

### Logistic Regression

```text
models/classifier/logistic_regression.joblib
```

Used by the prediction service for new-customer segment prediction.

### Random Forest

```text
models/classifier/random_forest.joblib
```

Stored as a candidate classifier from the model experimentation stage.

---

# 🚀 How to Run the Project

## Prerequisites

Install the following before running the project:

* Python 3.10+
* Node.js 18+
* npm
* Git

It is recommended to use a Python virtual environment.

---

## 1. Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd CUSTOMER-SEGMENTATION-DSS
```

Replace `<YOUR_GITHUB_REPOSITORY_URL>` with the URL of your GitHub repository.

---

# 🐍 2. Setup the Backend

Open a terminal in the project root.

Create a virtual environment:

### Windows

```bash
python -m venv venv
```

Activate it:

```bash
venv\Scripts\activate
```

### macOS / Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## 3. Install Python Dependencies

Install the backend dependencies:

```bash
pip install fastapi uvicorn pandas numpy scikit-learn joblib
```

If the repository contains a backend `requirements.txt`, use:

```bash
pip install -r requirements.txt
```

---

## 4. Start the FastAPI Backend

From the project root, run:

```bash
uvicorn backend.main:app --reload
```

Depending on the final location/name of the FastAPI entry file, the command may instead be:

```bash
uvicorn main:app --reload
```

The backend should become available at:

```text
http://127.0.0.1:8000
```

FastAPI's interactive API documentation should be available at:

```text
http://127.0.0.1:8000/docs
```

You can also check:

```text
http://127.0.0.1:8000/health
```

Expected response:

```json
{
  "status": "healthy"
}
```

---

# ⚛️ 5. Setup the Frontend

Open another terminal.

Navigate to the frontend:

```bash
cd frontend
```

Install the Node.js dependencies:

```bash
npm install
```

---

## 6. Start the React Application

Run:

```bash
npm run dev
```

Vite will provide a local development URL, normally similar to:

```text
http://localhost:5173
```

Open the displayed URL in your browser.

---

# 🔄 Running Both Parts

The application requires **both the backend and frontend**.

### Terminal 1 — Backend

```bash
cd CUSTOMER-SEGMENTATION-DSS
venv\Scripts\activate
uvicorn backend.main:app --reload
```

### Terminal 2 — Frontend

```bash
cd CUSTOMER-SEGMENTATION-DSS/frontend
npm install
npm run dev
```

Then open the frontend URL shown by Vite.

---

## 🔗 Application Architecture

```text
                 ┌──────────────────────────┐
                 │      React Frontend      │
                 │                          │
                 │  Overview                │
                 │  New Customer Prediction │
                 │  Analytics               │
                 └────────────┬─────────────┘
                              │
                         HTTP / JSON
                              │
                              ▼
                 ┌──────────────────────────┐
                 │       FastAPI API        │
                 │                          │
                 │  Dashboard Routes        │
                 │  Segment Routes          │
                 │  Analytics Routes        │
                 │  Prediction Routes       │
                 └────────────┬─────────────┘
                              │
                ┌─────────────┴─────────────┐
                │                           │
                ▼                           ▼
       ┌─────────────────┐       ┌──────────────────┐
       │ Processed Data  │       │  ML Artifacts    │
       │                 │       │                  │
       │ Customer        │       │ Preprocessor     │
       │ Features        │       │ K-Means          │
       │ Assignments     │       │ Classifier       │
       └─────────────────┘       └──────────────────┘
```

---

# 🧪 Model Development Workflow

The project also contains an experimentation workflow for evaluating supervised models.

The workflow:

1. Loads engineered customer features.
2. Loads the saved preprocessing pipeline.
3. Transforms the features.
4. Loads the frozen K-Means model.
5. Generates cluster labels.
6. Performs a stratified 80/20 train-test split.
7. Performs 5-fold stratified cross-validation.
8. Trains candidate classifiers.
9. Calculates classification metrics.
10. Generates classification reports and confusion matrices.
11. Saves trained candidate models.
12. Saves model comparison results.

The model experimentation stage evaluates both Logistic Regression and Random Forest rather than assuming that one model is automatically superior.

---

# 📈 Evaluation Metrics

The project uses several metrics to evaluate the classification models:

* **Accuracy**
* **Balanced Accuracy**
* **Macro Precision**
* **Macro Recall**
* **Macro F1**
* **Weighted F1**
* **Multiclass ROC-AUC**

Using multiple metrics is important because customer segments may not have identical distributions.

The project also generates:

* Classification reports
* Confusion matrices
* Cross-validation summaries
* Random Forest feature importance

---

# 🛡️ Input Validation

The prediction service performs basic business-rule validation before generating a prediction.

For example:

```text
unique_product_count <= item_count
unique_category_count <= item_count
```

The backend also derives features consistently with the model's expected input representation.

This ensures that the frontend does not have to reproduce the machine-learning feature engineering logic.

---

# 🔐 Model Reproducibility

A key design decision in the project is that the application does **not retrain the model every time it receives a prediction request**.

Instead, it loads saved artifacts:

```text
Preprocessor
     ↓
Transform Input
     ↓
Saved Classifier
     ↓
Prediction
```

This keeps prediction behavior consistent with the trained model.

---

# 🎯 Intended Use

This project is primarily intended as an educational and demonstrative **machine-learning decision-support application**.

It demonstrates how a machine-learning workflow can be converted into an application containing:

* Data processing
* Feature engineering
* Unsupervised learning
* Supervised learning
* Model evaluation
* Model persistence
* REST APIs
* Interactive dashboards
* New-user prediction

It is not intended to represent a production-ready enterprise customer analytics platform without additional security, monitoring, data governance, and deployment infrastructure.

---

# 🧰 Technologies Used

### Machine Learning

* Python
* Pandas
* NumPy
* Scikit-learn
* Joblib

### Backend

* FastAPI
* Uvicorn
* Pydantic / FastAPI validation
* REST API

### Frontend

* React
* Vite
* Recharts
* Axios / Fetch API
* CSS

### Development

* Git
* GitHub
* VS Code

---

# 📚 Main Components

| Component              | Purpose                                   |
| ---------------------- | ----------------------------------------- |
| K-Means                | Creates customer segments                 |
| Preprocessing Pipeline | Applies consistent feature transformation |
| Logistic Regression    | Predicts segment for new customers        |
| Random Forest          | Candidate classifier for model comparison |
| FastAPI                | Serves ML functionality through APIs      |
| React                  | Provides the user interface               |
| Recharts               | Visualizes customer analytics             |
| Pandas                 | Data manipulation and analysis            |
| Joblib                 | Stores and loads ML artifacts             |

---

# ⚠️ Important Notes

### 1. Saved model files are required

The prediction API expects the saved preprocessing and classifier artifacts to exist at their configured paths.

For example:

```text
models/preprocessing/customer_preprocessor.joblib
models/classifier/logistic_regression.joblib
```

### 2. Processed data is required

The dashboard expects:

```text
data/processed/customer_features.csv
data/processed/final_cluster_assignments.csv
```

### 3. Frontend and backend must both be running

Opening the React frontend without starting FastAPI will result in dashboard/prediction API errors.

### 4. API URL

The current frontend implementation expects the backend at:

```text
http://127.0.0.1:8000
```

If the backend is deployed somewhere else, update the API base URL in the frontend configuration/components.

---

# 🔮 Possible Future Improvements

The project can be extended with:

* Automated model retraining
* Model/data drift monitoring
* Authentication and authorization
* Database integration
* Cloud deployment
* Docker support
* Environment-based API configuration
* Automated testing
* CI/CD pipeline
* Customer-level historical tracking
* Real-time transaction ingestion
* Explainable AI for prediction results
* More advanced segmentation strategies
* Automated segment naming
* Model monitoring and performance tracking

---

# 👨‍💻 Author

**Gaurav Amrutkar**

Master of Computer Application (MCA)
Modern College of Engineering, Pune

---

# ⭐ Project Summary

**Customer Segmentation DSS** demonstrates an end-to-end machine-learning application that transforms customer purchasing behavior into meaningful behavioral segments and exposes those segments through an interactive decision-support dashboard.

The project combines:

```text
Data
  ↓
Feature Engineering
  ↓
Preprocessing
  ↓
K-Means Segmentation
  ↓
Classifier Training
  ↓
Saved ML Models
  ↓
FastAPI
  ↓
React Dashboard
  ↓
Customer Analytics + New Customer Prediction
```

The result is a complete demonstration of how a machine-learning segmentation workflow can be integrated into a usable web application.
