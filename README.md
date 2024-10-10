Here’s a well-structured README file for your Airflow DAG project with explanations about Snowflake, Google Cloud Composer, and your ETL pipeline:

---

# Stock Price ETL Pipeline using Apache Airflow, Snowflake, and Google Cloud Composer

This repository contains an ETL pipeline built using Apache Airflow, integrated with **Snowflake** as the data warehouse and orchestrated via **Google Cloud Composer**. The pipeline extracts stock price data from the Alpha Vantage API, transforms it, and loads it into Snowflake for further analysis and storage.

---

## Table of Contents
- [Overview](#overview)
- [Components](#components)
  - [Apache Airflow](#apache-airflow)
  - [Google Cloud Composer](#google-cloud-composer)
  - [Snowflake](#snowflake)
- [Pipeline Workflow](#pipeline-workflow)
- [Airflow DAG Details](#airflow-dag-details)
  - [DAG Structure](#dag-structure)
  - [Tasks](#tasks)
- [Prerequisites](#prerequisites)
- [Setup and Installation](#setup-and-installation)
  - [Google Cloud Composer](#google-cloud-composer)
  - [Snowflake](#snowflake-setup)
  - [Airflow Setup](#airflow-setup)
- [How to Run the DAG](#how-to-run-the-dag)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

This ETL pipeline fetches daily stock price data from Alpha Vantage API, processes the data, and stores it in Snowflake for long-term storage and future analysis. The pipeline is orchestrated using **Apache Airflow**, which is managed through **Google Cloud Composer**, a fully managed workflow orchestration service in Google Cloud. Snowflake serves as the data warehouse for storing stock price information.

---

## Components

### Apache Airflow
**Apache Airflow** is an open-source platform that helps in programmatically authoring, scheduling, and monitoring workflows. In this project, Airflow is used to orchestrate the ETL pipeline, ensuring the data extraction, transformation, and load process is seamless and efficient.

### Google Cloud Composer
**Google Cloud Composer** is a fully managed version of Apache Airflow. It helps in orchestrating workflows across cloud and on-premises environments. In this project, Cloud Composer manages the Airflow DAG that performs the ETL pipeline, ensuring scalability and reliability.

### Snowflake
**Snowflake** is a cloud-based data warehouse that supports highly scalable data storage and analytics. In this project, Snowflake is used to store the stock price data extracted by the ETL pipeline, allowing for efficient querying and long-term data retention.

---

## Pipeline Workflow

The Airflow DAG is scheduled to run every day at 2:30 AM. Here's an overview of the pipeline:

1. **Extract**: The pipeline extracts the last 90 days of stock prices from the [Alpha Vantage API](https://www.alphavantage.co/) for the symbol **LMT** (Lockheed Martin).
2. **Transform**: The extracted data is processed to retain important fields like `open`, `high`, `low`, `close`, `volume`, and `date`.
3. **Load**: The transformed data is loaded into a Snowflake table named `stock_prices` for analysis and long-term storage.

---

## Airflow DAG Details

### DAG Structure

- **DAG Name**: `Stock_DAG`
- **Schedule**: Runs daily at 2:30 AM
- **Tasks**: 
   - Extract stock price data for the last 90 days
   - Transform the data into a Snowflake-compatible format
   - Load the data into a Snowflake table (`stock_data_db.raw_data.stock_prices`)

### Tasks

1. **`extract_stock_data`**:
   - This task makes a request to the Alpha Vantage API to retrieve daily stock prices for the symbol **LMT**.
   - The raw data is returned in JSON format.
  
2. **`transform_stock_data`**:
   - This task processes the raw JSON data, extracting fields such as `open`, `high`, `low`, `close`, and `volume` for each day in the last 90 days.
   - The data is transformed into a list of records that are suitable for insertion into Snowflake.

3. **`load_to_snowflake`**:
   - This task creates the database, schema, and table in Snowflake (if they don’t exist).
   - It then inserts the transformed stock price data into the Snowflake table `stock_data_db.raw_data.stock_prices`.

---

## Prerequisites

- **Google Cloud Platform (GCP)** account.
- **Snowflake** account with access credentials.
- **Alpha Vantage API Key** for accessing stock price data.

---

## Setup and Installation

### Google Cloud Composer

1. **Create a Google Cloud Composer environment**:
   - In the Google Cloud Console, go to **Composer**.
   - Create a new environment with the necessary configurations.

2. **Install the Snowflake Provider in Cloud Composer**:
   - Add the `apache-airflow-providers-snowflake` package in the **PYPI packages** section of your Composer environment.

3. **Configure Airflow Variables and Connections**:
   - Set up the **Snowflake connection** in Airflow:
     - Go to **Admin > Connections** in the Airflow UI.
     - Add a new connection with the ID `snowflake_conn`, and provide your Snowflake credentials (account, username, password, etc.).
   - Add the **Alpha Vantage API key** as an Airflow variable:
     - Go to **Admin > Variables**, and create a variable with the key `vantage_api_key` and your API key as the value.

### Snowflake Setup

1. **Create Snowflake Database and Schema**:
   - You don’t need to manually create the database, schema, or table since the DAG automatically creates them if they don’t exist.
   - The database will be named `stock_data_db`, and the schema will be `raw_data`.

### Airflow Setup

1. **Clone this repository**:
   ```bash
   git clone https://github.com/your-username/your-repo-name.git
   ```

2. **Upload the DAG**:
   - Upload the Airflow DAG to the `/dags` folder of your Composer environment.
   - This will make the DAG visible in the Airflow UI.

---

## How to Run the DAG

1. **Activate the DAG**:
   - Go to the Airflow UI in Cloud Composer.
   - Toggle the DAG `Stock_DAG` to the **on** position.

2. **Monitor the DAG**:
   - You can monitor the DAG's progress and logs in the Airflow UI to ensure each task runs successfully.

---

## Contributing

Contributions to improve this project are always welcome. If you’d like to add a new feature or fix an issue, please submit a pull request with a clear description of the changes.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
