# Reddit-Data-Engineering-Project 

As active internet users, we engage with online information daily, and in domains like retail, marketing, and promotion analytics, it is crucial to stay closely connected to customer feedback, opinions, and interests. Reddit, being one of the largest and most dynamic social platforms, serves as a rich source of consumer insights. For CPG brands and retailers, monitoring Reddit discussions can reveal valuable, real-time signals that help guide data-driven business decisions.

In addition to traditional analytics, this project also explores the use of Reddit data in downstream applications such as Discord bot integration. By leveraging Reddit’s API, the pipeline fetches, processes, and stores data for features like subreddit monitoring, sentiment analysis, keyword alerts, and content aggregation. These capabilities enable real-time use cases—such as surfacing top posts, tracking community sentiment, or highlighting trending discussions within Discord—making Reddit data more accessible, interactive, and actionable for various user communities.


# Overview
This project implements a scalable, end-to-end data pipeline designed to extract, transform, and load (ETL) Reddit data into an Amazon Redshift data warehouse for advanced analytics and querying. The pipeline leverages a suite of modern AWS services and orchestrates each stage using Apache Airflow.

The architecture is designed with the following key components:

  - Extraction: Reddit data is fetched via the Reddit API using scheduled Airflow DAGs. This includes posts, comments, and metadata from specified subreddits.
  
  - Storage: Raw CSV data is ingested and stored securely in Amazon S3 for durability and future reference.
  
  - Transformation: AWS Glue and Amazon Athena are used to clean, structure, and normalize the data, converting it into a query-friendly format.
  
  - Loading: Transformed data is loaded into Amazon Redshift, where it becomes available for downstream analytics (SQL), dashboarding, and reporting use cases.

This pipeline supports both historical and near-real-time data ingestion, enabling use cases such as sentiment analysis, trend tracking, marketing intelligence, and even integration with applications like Discord bots for real-time content delivery.

# Data Pipeline Architecture
![Screenshot 2025-04-24 at 3 49 05 PM](https://github.com/user-attachments/assets/995e4fc6-5aa6-4639-9434-2afde4a02602)

  - Reddit API: Serves as the primary data source, providing access to posts, comments, and subreddit activity.
  - Apache Airflow & Celery: Handle the orchestration of the data pipeline, including scheduling and distributed task execution.
  - PostgreSQL: Used for intermediate data storage and managing workflow metadata.
  - Amazon S3: Acts as the landing zone for raw, unprocessed data.
  - AWS Glue: Powers the data catalog and executes ETL scripts to prepare data for analysis.
  - Amazon Athena: Enables SQL-based querying and transformation of data directly on S3.
  - Amazon Redshift: Serves as the centralized data warehouse for scalable analytics.

The entire workflow is orchestrated by Apache Airflow and Celery, enabling smooth coordination between pipeline components and ensuring dependable task execution. The system is containerized using Docker to support consistent, portable, and scalable deployments.

# Project Structure
    Reddit-Data-Engineering-Project/
    ├── config/                        # Configuration files for Airflow and other services
    │   └── config.conf.example        # Airflow configuration settings
    ├── dags/                          # Directed Acyclic Graphs for Airflow workflows
    │   └── reddit_dag.py              # Main ETL DAG for Reddit data processing
    ├── data/                          # Directory for storing raw and processed data
    │   ├── input/                     # Raw data files from Reddit API
    │   └── output/                    # Processed data ready for analysis
    ├── etls/                          # ETL scripts for data transformation
    │   ├── reddit_etl.py              # Script to extract data from Reddit API
    │   └── aws_etl.py                 # Script to transform data using AWS Glue
    ├── pipelines/                     # Pipeline definitions and orchestration logic
    │   ├── reddit_pipeline.py         # Main Reddit pipeline script
    │   └── aws_s3_pipeline.py         # Pipeline for handling data movement into S3
    ├── utils/                         # Utility scripts and helper functions
    │   └── Constants.py               # AWS-related helper functions
    ├── Dockerfile                     # Dockerfile for containerizing the application
    ├── docker-compose.yml             # Docker Compose configuration for multi-container setup
    ├── requirements.txt               # Python dependencies for the project
    └── README.md                      # Project documentation and setup instructions

1. Reddit API Integration
- Objective: Use Reddit's API to extract data, authenticated with a Client ID and Secret Key.
- Process: Set up a Reddit application to obtain API credentials, then use praw to fetch data from specified subreddits.
![redditAPI](https://github.com/user-attachments/assets/412cf7cb-7994-433c-8080-20f2b915f9a3)

2. Apache Airflow Setup
reddit_dag.py
- Objective: Orchestrate the ETL process.
- Process: Install required packages, configure Airflow settings, Configure connections to PostgreSQL (used as metadata DB), and define DAGs for task scheduling.
![Screenshot 2025-04-24 at 5 13 12 PM](https://github.com/user-attachments/assets/8a1fef13-b5a2-4365-abd3-1b460e58b64f)

3. Data Storage in AWS S3
- Objective: Clean and stage Reddit data.
- Process: Clean raw dict data using pandas. Convert it into CSV or Parquet. Upload processed files to Amazon S3.
<img width="1502" alt="Screenshot 2025-04-24 at 5 35 22 PM" src="https://github.com/user-attachments/assets/d2058460-9fd7-4c19-bfb9-ac72d0442e52" />

4. AWS Glue Integration
- Objective: Catalog and transform data.
- Process:
  - Set up a Crawler to scan S3 and populate AWS Glue Data Catalog.
  - Create and run a Glue Job to perform any additional transformation (e.g., merging columns, casting types).
<img width="1500" alt="Screenshot 2025-04-24 at 10 55 41 PM" src="https://github.com/user-attachments/assets/9a2e2805-86a9-4fe6-bc4a-08eb6c352f8f" />




