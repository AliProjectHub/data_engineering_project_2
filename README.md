# Advanced Weather Data Pipeline using Apache Airflow

## Overview

This project sets up an advanced Apache Airflow DAG to extract weather data from an API, join it with city data from a Postgres database, and load the joined data into an AWS S3 bucket. The weather data is for the city of Houston and includes various metrics such as temperature, humidity, wind speed, and more.

## Features

- **Task Grouping**: Organizes related tasks into groups for better readability and maintainability.
- **HTTP Sensor**: Checks if the weather API is ready.
- **Data Extraction**: Extracts weather data using an HTTP GET request.
- **Data Transformation**: Transforms the extracted data into a structured format.
- **Data Loading**: Loads the transformed data into a Postgres database.
- **Data Joining**: Joins the weather data with city data.
- **Data Export**: Exports the joined data into an AWS S3 bucket.

## Installation

1. **Clone the Repository**:
    ```bash
    git clone https://github.com/your-repo/weather-data-pipeline.git
    cd weather-data-pipeline
    ```

2. **Install Dependencies**:
    Ensure you have Apache Airflow and other necessary dependencies installed. You can use the following command to install Airflow:
    ```bash
    pip install apache-airflow
    ```

3. **Set Up AWS Credentials**:
    Replace the placeholder values for AWS credentials in the `save_joined_data_s3` function with your actual AWS access key, secret key, and session token.

## Configuration

1. **Airflow Connections**:
    - Set up an Airflow HTTP connection named `weathermap_api` with the base URL of the weather API.
    - Set up a Postgres connection named `postgres_conn` for the Postgres database.
    - Add your API key to the endpoint in the DAG script.

2. **DAG Configuration**:
    - Modify the `default_args` dictionary as needed, particularly the email configuration.
    - Set the `start_date` to your desired start date.
    - Adjust the `schedule_interval` to control how frequently the DAG runs.

## Code Explanation

### Imports

- **Airflow**: For creating and managing the DAG.
- **Datetime and Timedelta**: For managing date and time operations.
- **DummyOperator**: For marking the start and end of the pipeline.
- **TaskGroup**: For grouping related tasks.
- **PostgresOperator**: For executing SQL commands in a Postgres database.
- **HTTP Sensor and Operator**: For making HTTP requests and handling responses.
- **Pandas**: For data manipulation and transformation.
- **PostgresHook**: For interacting with the Postgres database.

### Functions

- **kelvin_to_fahrenheit**: Converts temperature from Kelvin to Fahrenheit.
- **transform_load_data**: Transforms the extracted weather data and loads it into a Postgres database.
- **load_weather**: Loads weather data from a CSV file into the Postgres database.
- **save_joined_data_s3**: Saves the joined weather and city data into an AWS S3 bucket.

### Default Arguments

Defines default arguments for the DAG, including the owner, start date, retry configuration, and email settings.

### DAG Definition

- **start_pipeline**: Marks the start of the pipeline.
- **join_data**: Joins weather data with city data in the Postgres database.
- **load_joined_data**: Saves the joined data to an AWS S3 bucket.
- **end_pipeline**: Marks the end of the pipeline.
- **group_a**: A task group for handling the extraction and loading of weather and city data.

### DAG Workflow

Defines the task dependencies to ensure the tasks run in the correct order:
```plaintext
start_pipeline >> group_A >> join_data >> load_joined_data >> end_pipeline
