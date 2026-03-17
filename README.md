# minio-file-ingestion-pipeline-to-fact-table
End-to-end file ingestion pipeline from MinIo to database, including raw loading, staging, transformations, and loading into a fact table.

![](airflow%20head.png)
![](airflow%20tail.png)

## Introduction
Data platforms rely on reliable pipelines to ingest, process, and transform data from various storage systems into structured formats suitable for analytics. This project demonstrates the implementation of an end-to-end file ingestion pipeline that processes data from object storage into a relational data warehouse structure.

The pipeline ingests files stored in MinIO object storage (Landing zone), processes them through multiple data layers (Raw, Staging, and Fact), and loads the final transformed data into PostgreSQL fact table for analytical use. Workflow orchestration is handled using Apache Airflow, ensuring monitoring, and execution of each stage of the pipeline.


## Problem Statement
Organizations often receive large volumes of data in unstructured or semi-structured files. Without a structured ingestion pipeline, it becomes difficult to reliably process, transform, and load this data into analytical databases.

Key challenges include:
Efficiently ingesting files from object storage.
Tracking which files have been processed.
Handling transformations between raw data and analytical models.
Ensuring reliable and repeatable pipeline execution.


## Skills and Concepts demonstrated
The following are some of the skills and concept demonstrated during the execution of this project:
* End-to-End Data Pipeline Development
* Object Storage Integration
* Data Validation & File Processing
* Layered Data Architecture
* Workflow Orchestration
* Data Transformation & Modeling
* Data Lineage & Tracking
* Logging & Observability
* Modular Pipeline Design 

## Tech Stack
* Python
* Apache Airflow
* PostgreSQL
* MinIO
* Docker

## Data Pipeline Workflow
There are 4 pipeline stages:
* File Processor – Ingests files from MinIO object storage and performs validation checks, including filtering files using regex patterns, removing duplicates, counting records, assigning chunks, and identifying valid files for downstream processing.

* Raw Layer – Stores ingested files in their original format to ensure traceability, auditability, and the ability to reprocess data if needed.

* Staging Layer – Performs initial data processing and structuring, preparing datasets for transformation and ensuring they conform to the expected schema.

* Fact Layer – Applies business transformations and loads the processed data into PostgreSQL fact table, making it ready for analytical queries and reporting.


Using Apache Airflow for orchestration, the pipeline ensures each stage runs in the correct order, enabling a scalable and maintainable approach to file-based data ingestion.


## Orchestration



















