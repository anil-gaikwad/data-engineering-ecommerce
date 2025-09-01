# 🛒 E-Commerce Analytics Data Platform
# 📌 Overview

This project is an end-to-end Data Engineering pipeline built with Python + PySpark.
It simulates a real-world E-commerce Data Platform, handling both batch and streaming data from multiple sources:

🗂️ Transactions (CSV files – batch ingestion)

🧑‍🤝‍🧑 Customer Data (PostgreSQL – batch ingestion)

⚡ Clickstream Logs (Kafka – real-time streaming)

The pipeline processes data into Bronze → Silver → Gold layers (raw → cleaned → analytics-ready) and enables insights such as daily sales, active users, and customer behavior.


⚙️ Architecture
```diff
          +-------------+
          |  PostgreSQL |
          +-------------+
                 |
        +-------------------+      +----------------+
        |   Kafka Stream    | ---> | Clickstream    |
        | (User Activity)   |      | Logs (JSON)    |
        +-------------------+      +----------------+
                 |
                 v
        =======================
         Ingestion Layer
        =======================
                 |
                 v
        =======================
         Processing Layer
        (PySpark ETL)
        =======================
         Bronze → Silver → Gold
                 |
                 v
        =======================
         Storage Layer
        (Delta Lake / Parquet)
        =======================
                 |
                 v
        =======================
         Analytics Layer
        (Superset / BI Tool)
        =======================
```


📂 Project Structure

```bash
ecommerce_data_platform/
│── configs/
│    ├── spark_config.yaml
│    └── db_config.yaml
│
│── data/
│    ├── raw/          # Raw files (CSV, JSON)
│    ├── bronze/       # Raw ingested data
│    ├── silver/       # Cleaned & transformed
│    └── gold/         # Aggregated analytics
│
│── src/
│    ├── ingestion/
│    │    ├── load_transactions.py
│    │    ├── load_customers.py
│    │    └── stream_clickstream.py
│    │
│    ├── processing/
│    │    ├── transform_transactions.py
│    │    ├── enrich_data.py
│    │    └── aggregate_metrics.py
│    │
│    ├── utils/
│    │    ├── logger.py
│    │    ├── config_loader.py
│    │    └── db_connector.py
│    │
│    ├── main_batch.py      # Batch pipeline entry
│    └── main_stream.py     # Streaming pipeline entry
│
│── docker-compose.yml      # Local PostgreSQL + Kafka
│── requirements.txt
│── README.md

```

🚀 Features

* ✅ Batch ingestion of CSV + PostgreSQL data
* ✅ Real-time ingestion of Kafka clickstream logs
* ✅ ETL pipeline with PySpark (Bronze → Silver → Gold)
* ✅ Aggregated business KPIs (daily sales, unique customers, product demand)
* ✅ Streaming analytics for user behavior
* ✅ Pluggable storage (Parquet / Delta Lake)
* ✅ Ready for orchestration with Airflow / Prefect
* ✅ Can scale to AWS EMR / Databricks


🔧 Setup & Installation

1️⃣ Clone Repository

```bash
git clone https://github.com/anil-gaikwad/data-engineering-ecommerce.git
cd ecommerce_data_platform
```

2️⃣ Install Dependencies
```bash 
pip install -r requirements.txt

```
3️⃣ Start Services (PostgreSQL + Kafka)
```bash
docker-compose up -d
```
4️⃣ Load Sample Data

* Place sample transactions.csv inside data/raw/
* PostgreSQL customers table will be auto-created by docker-compose (or run SQL script manually).

▶️ Running the Pipelines

🔹 Batch Processing (Transactions + Customers)
```bash
python src/main_batch.py
```

* Reads raw transactions + customers

* Cleans & enriches data

* Saves processed data in data/silver/

* Writes daily sales KPIs in data/gold/analytics/

🔹 Streaming Processing (Clickstream from Kafka)

```bash
python src/main_stream.py

```

* Reads JSON clickstream events from Kafka

* Processes in real-time

* Outputs to console / storage


👨‍💻 Tech Stack

* Python 3.9+
* PySpark
* PostgreSQL (via JDBC)
* Apache Kafka
* Docker + docker-compose
* Parquet / Delta Lake
