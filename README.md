# minio-file-ingestion-pipeline-to-fact-table
End-to-end file ingestion pipeline from MinIo to database, including raw loading, staging, transformations, and loading into a fact table.

![](airflow%20head.png)
![](airflow%20tail.png)

## Introduction
Data platforms rely on reliable pipelines to ingest, process, and transform data from various storage systems into structured formats suitable for analytics. This project demonstrates the implementation of an end-to-end file ingestion pipeline that processes data from object storage into a relational data warehouse structure.

The pipeline ingests files stored in MinIO object storage (landing zone), processes them through multiple data layers (Raw, Staging, and Fact), and loads the final transformed data into PostgreSQL fact table for analytical use. Workflow orchestration is handled using Apache Airflow, ensuring monitoring, and execution of each stage of the pipeline.


## Problem Statement
Organizations often receive large volumes of data in unstructured or semi-structured files. Without a structured ingestion pipeline, it becomes difficult to reliably process, transform, and load this data into analytical databases.

Key challenges include:
* Efficiently ingesting files from object storage.
* Building structured datasets for analytics
* Automating ingestion pipelines


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



* MiniO
![](Screenshot%202026-03-18%20035132.png)

## Data Pipeline Workflow
There are 4 pipeline stages:
* File Processor – Ingests files from MinIO object storage and performs validation checks, including filtering files using regex patterns, removing duplicates, counting records, assigning chunks, and identifying valid files for downstream processing.

* Raw Loader – Stores ingested files in their original format to ensure traceability, auditability, and the ability to reprocess data if needed.

* Staging Loader – Performs initial data processing and structuring, preparing datasets for transformation and ensuring they conform to the expected schema.

* Fact Loader – Applies business transformations and loads the processed data into PostgreSQL fact table, making it ready for analytical queries and reporting.

* Pipeline Workflow
![](Screenshot%202026-03-18%20023011.png)


## Pipeline Components
These components ensure that data moves through the system in a controlled, traceable, and structured manner.

1. File Processor

The File Processor is responsible for :-
* Identifying and preparing files for ingestion from MinIO object storage.
* Reads files from the configured bucket and incoming directory.
* Selects only files that match the expected naming pattern (using regex pattern)
* Prevents reprocessing of files that have already been ingested.
* Counts the number of records in each file for tracking purposes.
* Groups files into chunks to support batch processing.
* Registers each file and it metadata into a tracking table in PostgreSQL.

This stage ensures that only valid files enter the ingestion pipeline.

* Tracker Table
![](Screenshot%202026-03-18%20031246.png)

2. Raw Layer
The Raw Layer stores the ingested data exactly as received, without transformation. Its purpose is to provide:
* Data traceability
* Allows data to be reprocessed if transformations change.
* Enables verification of the source data.

The raw layer acts as a single source of truth for the ingested data. Raw files are loaded directly from MinIO into PostgreSQL raw tables.

3. Staging Layer
The Staging Layer performs the first level of data transformation and normalization. At this stage:
* Data types are standardized.
* Invalid or malformed records are filtered and logged.
* Data is inserted into structured staging tables in PostgreSQL.

The staging layer prepares the data for analytical use by converting semi-structured raw data into a clean relational format.


4. Fact Layer
The Fact Layer represents the final curated dataset used for analytics and reporting. In this stage:
* Business logic is applied to produce analytical fact tables.
* Records are loaded into optimized fact tables in PostgreSQL.

The fact layer provides high-quality, analysis-ready data that can be used by BI tools, dashboards, or downstream analytics systems.

![](Screenshot%202026-03-18%20032138.png)


## Orchestration (Apache Airflow)
The pipeline follows a sequential workflow:

file_processor_task
        ↓
raw_loader_task
        ↓
staging_loader_task
        ↓
fact_loader_task


This dependency chain ensures:
* Files are validated before ingestion
* Raw data is stored before transformations
* Staging tables are populated before fact table loading
* Each step runs only if the previous step completes successfully

Airflow provides built-in monitoring features. This orchestration layer ensures the ingestion pipeline is reliable, traceable, and easy to manage in production environments.

* Metadata exchanged between tasks using XCom
![](Screenshot%202026-03-18%20023459.png)

* Custom metric logs for observability
![](Screenshot%202026-03-18%20023228.png)


## Project Summary

This project demonstrates implementation of a modular, production-style data ingestion pipeline that processes event files from MinIO and loads curated analytical data into PostgreSQL, orchestrated using Apache Airflow.

The pipeline implements a layered data architecture (Raw → Staging → Fact) to ensure data traceability, structured transformations, and reliable analytical outputs. It includes automated file validation, duplicate detection, chunk-based processing, and metadata tracking to support controlled ingestion and reproducibility.

By separating ingestion, transformation, and loading stages, the system promotes scalability, maintainability, and clear data lineage. 

Overall, the project highlights key data engineering practices, including modular pipeline design, workflow orchestration, and robust ingestion patterns for processing large volumes of data.


## Thank you for reading!
