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


