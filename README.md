# 🏥 Healthcare Claims ETL Pipeline

An end-to-end data engineering project to process large volumes of healthcare insurance claims, detect fraud, and optimize cost. Built with a modern stack:

**Apache NiFi → AWS S3 → PySpark → DBT → Snowflake → Airflow → Power BI**

---

## 📦 Architecture Overview

```text
[NiFi] → [S3 Raw Layer] → [PySpark ETL] → [S3 Processed Layer] → [DBT] → [Snowflake] → [Power BI Dashboard]
                      ↘ [Airflow Orchestration & Scheduling] ↙
```

---

## 🛠️ Stack

| Component       | Technology     |
|----------------|----------------|
| Ingestion      | Apache NiFi    |
| Raw Storage    | AWS S3         |
| Processing     | PySpark        |
| Modeling       | DBT            |
| Warehouse      | Snowflake      |
| Orchestration  | Apache Airflow |
| Dashboarding   | Power BI       |
| Containerization| Docker & Compose |
| CI/CD          | GitHub Actions |

---

## 📁 Directory Structure

```bash
.
├── docker-compose.yml
├── nifi/
│   ├── Dockerfile
│   └── flow/            # Optional flow template or flow.json.gz
├── spark/
│   ├── Dockerfile
│   └── spark_etl.py
├── dbt/
│   ├── Dockerfile
│   ├── profiles.yml
│   └── healthcare_project/
│       ├── dbt_project.yml
│       └── models/
│           └── patient_summary.sql
├── airflow/
│   ├── Dockerfile
│   └── dags/
│       └── healthcare_etl_dag.py
└── .github/workflows/ci.yml
```

---

## 🚀 How to Run Locally

### Prerequisites
- Docker & Docker Compose
- AWS credentials for S3 access (via `.env` file)
- Snowflake account credentials

### Steps
```bash
git clone https://github.com/your-repo/healthcare-etl
cd healthcare-etl
docker compose up --build
```

Access:
- **NiFi UI**: http://localhost:8081
- **Airflow UI**: http://localhost:8080

---

## ⚙️ Services Details

### 🔄 NiFi
- Ingests CSV files and uploads to `s3a://healthcare-etl-raw/staging/`
- Uses processors: `GetFile`, `PutS3Object`

### 🔥 PySpark
```python
# spark/spark_etl.py
from pyspark.sql import SparkSession
...
df = spark.read.option("header", True).csv("s3a://healthcare-etl-raw/staging/healthcare_data.csv")
...
df_clean.write.mode("overwrite").parquet("s3a://healthcare-etl-cleaned/processed/")
```

### 📊 DBT
- Connects to Snowflake
- Models built in `models/patient_summary.sql`

### ⏱ Airflow
```python
# airflow/dags/healthcare_etl_dag.py
run_spark_etl >> run_dbt
```

### 📉 Power BI
- Connect to Snowflake with ODBC or native connector
- Example dashboard:
  - Fraudulent Claims Count
  - Avg. Cost per Diagnosis
  - Provider vs Payer Ratios

---

## 🧪 CI/CD

### `.github/workflows/ci.yml`
- Lint `spark_etl.py` with `flake8`
- Build Docker images

---

## 🙌 Contribution
PRs are welcome! Raise issues if you need help setting up.

---

## 📄 License
MIT License
