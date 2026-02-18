📦 Databricks ML Project: End‑to‑End ML Pipeline with MLflow, Feature Engineering, and Workflows
Using TPCDS_SF1000 Retail & E‑Commerce Dataset
📘 Overview
This project demonstrates a full end‑to‑end Machine Learning workflow on Databricks, showcasing:

PySpark‑based feature engineering at scale

MLflow experiment tracking

Model Registry lifecycle management

Databricks Workflows orchestration

Delta Lake–based data pipelines

Production‑ready batch inference

The dataset used is TPCDS_SF1000, a large‑scale synthetic retail + e‑commerce dataset commonly used for benchmarking. It includes customer behavior, orders, web interactions, store sales, and more — ideal for demonstrating real‑world ML engineering.

🎯 Project Goal
Predict customer spending behavior using historical store and web sales data.

This is a realistic business problem for omnichannel retailers and allows you to demonstrate:

Multi‑table joins

Time‑based aggregations

Customer‑level feature engineering

Regression modeling

Model deployment workflows

🏗️ Architecture
Code
Databricks Workspace
│
├── Data Layer (Delta Lake)
│     ├── Raw TPCDS tables
│     ├── Cleaned & joined feature tables
│     └── Predictions output table
│
├── Notebooks / Scripts
│     ├── 01_data_ingestion.py
│     ├── 02_feature_engineering.py
│     ├── 03_model_training.py
│     ├── 04_batch_inference.py
│
├── MLflow
│     ├── Experiment tracking
│     ├── Model artifacts
│     └── Model Registry (Staging → Production)
│
└── Databricks Workflows
      ├── Ingest
      ├── Feature Engineering
      ├── Train & Register Model
      └── Batch Inference
📂 Repository Structure
Code
/ml_project/
    ├── notebooks/
    │     ├── 01_data_ingestion.py
    │     ├── 02_feature_engineering.py
    │     ├── 03_model_training.py
    │     ├── 04_batch_inference.py
    │
    ├── src/
    │     ├── features.py
    │     ├── train.py
    │     ├── utils.py
    │
    ├── conf/
    │     ├── config.yaml
    │
    ├── tests/
    └── README.md
🧱 1. Data Ingestion
The project uses the TPCDS_SF1000 dataset available in Databricks sample data.

Example tables used:

store_sales

web_sales

customer

date_dim

item

The ingestion notebook:

Reads raw tables

Selects relevant columns

Writes cleaned Delta tables to /mnt/raw/tpcds/

🧪 2. Feature Engineering (PySpark)
This is the heart of the project and demonstrates your Data Engineering strength.

Key transformations:

Customer‑level aggregations (total spend, avg spend, recency)

Store vs. web channel behavior

Time‑based features using date_dim

Window functions for rolling metrics

Joining multiple fact + dimension tables

Writing feature tables to Delta Lake

Example engineered features:

Feature	Description
total_store_sales	Total spend in physical stores
total_web_sales	Total spend online
avg_basket_value	Mean order value
days_since_last_purchase	Recency metric
rolling_30d_spend	30‑day rolling spend window
Output is saved to:

Code
/mnt/features/tpcds/customer_features
🤖 3. Model Training with MLflow
The training notebook:

Loads engineered features

Splits into train/test

Trains a regression model (Random Forest, XGBoost, or Spark ML)

Logs parameters, metrics, and artifacts to MLflow

Registers the model in the Model Registry

Example MLflow logging:

python
with mlflow.start_run():
    mlflow.log_params(model_params)
    mlflow.log_metric("rmse", rmse)
    mlflow.spark.log_model(model, "model")
🏛️ 4. Model Registry Workflow
The model is registered as:

Code
models:/tpcds_customer_spend_model
Lifecycle stages:

None → initial registration

Staging → validation

Production → used for inference

You can promote/demote versions through the UI or API.

📦 5. Batch Inference Pipeline
The inference notebook:

Loads the Production model

Scores new customer data

Writes predictions to Delta Lake

Output table:

Code
/mnt/predictions/tpcds/customer_spend_predictions
This simulates a real production scoring job.

🔄 6. Databricks Workflow (Job Orchestration)
A Databricks Workflow orchestrates the entire pipeline:

Ingest Data

Feature Engineering

Train Model

Register Model

Batch Inference

Each task depends on the previous one, forming a DAG.

🧪 Testing
Unit tests cover:

Feature transformations

Utility functions

Schema validation

Tests run locally or in Databricks Repos.
