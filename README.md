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

# Table of Contents

1. [Data Pipeline Architecture](#data-pipeline-architecture)
2. [Project Structure](#project-structure)

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

### 1. Reddit API Integration
- Objective: Use Reddit's API to extract data, authenticated with a Client ID and Secret Key.
- Process: Set up a Reddit application to obtain API credentials, then use praw to fetch data from specified subreddits.
![redditAPI](https://github.com/user-attachments/assets/412cf7cb-7994-433c-8080-20f2b915f9a3)

### 2. Apache Airflow Setup
- Objective: Orchestrate the ETL process.
- Process: Install required packages, configure Airflow settings, Configure connections to PostgreSQL (used as metadata DB), and define DAGs for task scheduling.
![Screenshot 2025-04-24 at 5 13 12 PM](https://github.com/user-attachments/assets/8a1fef13-b5a2-4365-abd3-1b460e58b64f)

### 3. Data Storage in AWS S3
- Objective: Clean and stage Reddit data.
- Process: Clean raw dict data using pandas. Convert it into CSV or Parquet. Upload processed files to Amazon S3.
<img width="1502" alt="Screenshot 2025-04-24 at 5 35 22 PM" src="https://github.com/user-attachments/assets/d2058460-9fd7-4c19-bfb9-ac72d0442e52" />

### 4. AWS Glue Integration
- Objective: Catalog and transform data.
- Process:
  - Set up a Crawler to scan S3 and populate AWS Glue Data Catalog.
  - Create and run a Glue Job to perform any additional transformation (e.g., merging columns, casting types).
<img width="1211" alt="Screenshot 2025-04-24 at 10 56 36 PM" src="https://github.com/user-attachments/assets/ebf5606d-8e56-4774-a876-33526a9d78a6" />

### 5. Querying with Amazon Athena
- Objective: Run SQL queries on the processed data in S3.
- Process:
  - Define an Athena table using the Glue Catalog.
  - Use SQL queries to filter, aggregate, or join data for insights.
<img width="1461" alt="Screenshot 2025-04-24 at 11 06 39 PM" src="https://github.com/user-attachments/assets/4c88fa32-cb09-4774-82a2-c861d2f37fed" />

### 6. Amazon Redshift Integration
- Objective: Load clean data into a centralized data warehouse.
- Process:
  - Set up a Redshift cluster and schema.
  - Use the COPY command to pull data from S3 into Redshift.
  - Validated that the data loaded correctly using SQL queries.
<img width="1419" alt="Screenshot 2025-04-24 at 11 11 23 PM" src="https://github.com/user-attachments/assets/d3b69c5d-538e-4250-b11a-a224197b410b" />

# Data Source Overview

Reddit is one of the largest online discussion platforms, hosting a wide variety of communities (subreddits) on topics ranging from technology to entertainment to professional discussions. Its official API provides developers with access to a massive stream of user-generated content in near real-time, making it a valuable resource for research, trend analysis, sentiment tracking, and portfolio projects.

This project primarily utilizes Reddit's Posts API via the praw (Python Reddit API Wrapper) library. It extracts metadata from posts in targeted subreddits (e.g., r/dataengineering) such as:

- Post titles and content
- User engagement metrics (score, comments)
- Author details
- Timestamps
- Post flags (NSFW, spoiler, stickied, edited)

These fields can be used to build pipelines for text analysis, dashboarding, alert systems, and more.

To use the Reddit API, developers must register an application via Reddit's app console and authenticate with a Client ID, Secret, and User Agent. Unlike some government APIs, Reddit’s API enforces rate limits and uses OAuth2 for access control, making proper session handling essential.

<img width="462" alt="Screenshot 2025-04-24 at 11 23 25 PM" src="https://github.com/user-attachments/assets/d15e22bd-c638-4297-b95f-360c933435ad" />

- id: Unique identifier for each post
- title: Title of the Reddit post
- score: Upvote count
- num_comments: Total number of comments
- author: Username of the post creator
- created_utc: Timestamp when the post was created (UTC)
- URL: Direct URL to the Reddit post or external content
- over_18: Whether the post is marked NSFW
- ESS_updated: Flags indicating:
  - edited – Whether the post has been edited
  - spoiler – If the post is marked as a spoiler
  - stickied – If the post is pinned (stickied) by a moderator

Final table example:
![Screenshot 2025-04-24 at 11 33 14 PM](https://github.com/user-attachments/assets/7a8e1929-9e72-4ccf-8d4a-a83f6f054c4b)





