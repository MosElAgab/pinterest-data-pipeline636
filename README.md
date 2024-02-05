# Pinterest Data Pipeline

## Table of Content
1. [Project Description](#project-description)
    - [Overview](#overview)
    - [Project Architecture](#project-architecture)
    - [Project Aim](#project-aim)
    - [What I learned](#what-i-learned)
1. [Installation Instruction](#installation-instruction)
    - [Configuration files](#configuration-files)
    - [Virtual Environment](#virtual-environment-venv)
    - [Database](#database)
1. [Usage Instruction](#usage-instruction)
    - [ETL process](#etl-process)
    - [Building a star schema](#building-a-star-schema)
    - [Query the data](#query-the-data)
    - [Run the code](#run-the-code)
    - [Testing](#testing)
1. [File Structure](#file-structure)
1. [License](#license)


## Project Description

### Overview

The Pinterest Data Pipeline project is an end to end data engineering pipeline crafted to establish a robust data management system, similar to those employed by major social media platforms like Pinterest. It offers a tangible demonstration of the effective collection of computing resources, processing tools, and storage of data within a cloud infrastructure, specifically tailored for AWS.

### Project Architecture


### Project Aim

The core objective of this project is to offer a hands-on exploration of configuring and managing a comprehensive data pipeline. It provides valuable insights into the strategies employed by large-scale applications like Pinterest to efficiently handle vast datasets, ensuring seamless processing and secure storage. The overarching goal is to construct a resilient data pipeline that empowers us to:

- **Data Emulation:** Develop a script designed to fetch data from an Amazon RDS, simulating the posting process akin to platforms like Pinterest.

- **Data Processing with Kafka:** Implement Apache Kafka to adeptly process the surge of incoming data, ensuring smooth data flow and scalability.

- **Data Storage in S3:** Employ Amazon S3 buckets for the secure storage of processed data, offering convenient access for future analysis.

- **API Integration for Data Streaming:** Create an API facilitating the streaming of data into the Kafka cluster, followed by storage in the designated S3 bucket.

- **Data Analysis in Databricks:** Establish a connection between Databricks and the S3 bucket to conduct thorough batch analysis on the stored Pinterest data.

- **Workflow Orchestration with MWAA:** Leverage Managed Workflows for Apache Airflow (MWAA) to orchestrate intricate data workflows using Directed Acyclic Graphs (DAGs), enhancing automation and monitoring capabilities in the data pipeline.

- **Real-time Data Handling with Kinesis:** Integrate AWS Kinesis Data Streams to augment the pipeline's capabilities for real-time data management.

### What I Learned:

The journey of developing and implementing this project has enriched my practical knowledge across pivotal concepts and tools within the realms of data engineering and cloud computing:

- **AWS Services:** I have gained proficiency in Amazon Web Services, mastering the intricacies of IAM, VPC, EC2, and S3. This includes adeptly configuring roles and permissions for seamless operation.

- **Apache Kafka:** From installation to configuration on an EC2 instance, I now possess a comprehensive understanding of how to leverage Kafka for real-time data streaming and processing.

- **MSK Cluster:** My exploration extended to Amazon Managed Streaming for Apache Kafka (MSK), where I proficiently created and managed Kafka clusters within the AWS ecosystem.

- **API Gateway:** I successfully conceptualized and configured an API using AWS API Gateway, enhancing my ability to facilitate smooth data streaming.

- **Kafka REST Proxy:** The setup of a Kafka REST Proxy for streamlined communication with the Kafka cluster, coupled with insights into IAM authentication during API interactions, has fortified my grasp on secure data handling.

- **Apache Spark and Databricks Integration:** I delved into the creation of clusters, mounting S3 buckets, assuming IAM roles, and executing data cleaning and querying operations for analysis within Databricks.

- **Apache Airflow (MWAA):** My proficiency now extends to creating and managing an Airflow environment, orchestrating workflows using DAGs, and seamlessly integrating with Databricks for streamlined workload automation. This experience significantly deepened my understanding of workflow management in cloud environments.

- **AWS Kinesis Data Streams:** Acquiring skills in real-time data streaming and management using AWS Kinesis has added a dynamic dimension to my toolkit, broadening my capabilities in handling data in motion.

