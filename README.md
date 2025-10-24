# SBDL Project

SBDL (Spark-Based Distributed Loader) is a distributed data processing application that uses PySpark for large-scale data transformations and Kafka for real-time data ingestion. It is designed to run in multiple environments (LOCAL, QA, PROD) with centralized configuration for flexibility and consistency.

The main goal of SBDL is to ingest, transform, and load large volumes of data in a distributed and fault-tolerant way, making it suitable for ETL pipelines, analytics, and batch processing.

## Key Features 

- **Distributed Data Processing**: Uses PySpark for scalable data transformations.
- **Real-Time Integration**: Reads data from Kafka topics for near real-time processing.
- **Environment Flexibility**: Configurable for local development, QA testing, or production.
- **Data Quality Verification**: Includes unit and integration tests using pytest and nutter.
- **CI/CD Ready**: Jenkins pipeline automates build, test, packaging, and deployment.

## Data Flow Overview

- **Kafka ingestion**: Data is read from one or more Kafka topics.
- **Spark processing**: Data is transformed using PySpark (filtering, aggregation, enrichment).
- **Hive or storage output**: Processed data can be saved to Hive tables (for QA/PROD) or local files (for LOCAL).
- **Monitoring & Logging**: Logs are managed via log4j.properties.

## Example Workflow

- Start Kafka locally or connect to the cluster
- Run SBDL locally for development/debugging
- Validate transformations using tests
- Submit to Spark cluster for QA/PROD execution
- Check Hive tables or output files to confirm results

## Project Structure

```bash
.
├── conf/
│   ├── sbdl.conf          # Environment configurations (Kafka, Hive, filters)
│   └── spark.conf         # Spark-specific configurations
├── lib/                   # Main source code
│   └── ...
├── tests/                 # Unit and integration tests
│   └── ...
├── Jenkinsfile            # CI/CD pipeline
├── Pipfile                # Project dependencies
├── Pipfile.lock           # Locked versions of dependencies
├── main.py                # Main script
├── sbdl_submit.sh         # Spark submission script
├── log4j.properties       # Logging configuration
├── test_nutter_sbdl.py    # Nutter tests (root)
└── test_pytest_sbdl.py    # Pytest tests (root)
```

## Dependencies

- Python 3.10 (managed with Pipenv)
- pyspark: Distributed processing framework
- pytest & nutter: Unit and integration testing frameworks
- chispa: Spark DataFrame testing helpers
- faker: Fake data generation for testing

### Installation
```bash
pip install pipenv
pipenv install --dev
```

### Key Packages

- **pyspark**: Distributed processing framework
- **pytest & nutter**: Unit and integration tests
- **chispa**: Spark DataFrame testing helper
- **faker**: Fake data generation for testing

## Running the Application

### Local
```bash
pipenv run python main.py --env LOCAL
```

### QA / PROD

Use the submission script to run on the Spark cluster:

```bash
./sbdl_submit.sh QA
./sbdl_submit.sh PROD
``` 

## CI/CD Pipeline (Jenkins)

The Jenkinsfile defines the following stages:

- **Build**: Create virtual environment and install dependencies
- **Test**: Run unit tests (pytest)
- **Package**: ZIP the lib directory (sbdl.zip) on master or release branches
- **Release**: Copy files to QA server on release branch
- **Deploy**: Copy files to production server on master branch

## Tests

SBDL includes tests both at unit and integration level:

- **Pytest tests**: Focus on validating data transformations
- **Nutter tests**: Ensure end-to-end workflows behave correctly
- **Chispa**: Validates Spark DataFrames

### Run All Tests
```bash
pipenv run pytest
```

## Credits 

- Based on Prashant Kumar Pandey’s course: PySpark - Apache Spark Programming in Python