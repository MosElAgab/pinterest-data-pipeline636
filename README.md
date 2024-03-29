# Pinterest Data Pipeline

## Table of Content
1. [Project Description](#project-description)
    - [Overview](#overview)
    - [Project Architecture](#project-architecture)
    - [Project Aim](#project-aim)
    - [What I learned](#what-i-learned)
1. [Installation Instruction](#installation-instruction)
    - [Prerequisites](#prerequisites)
    - [AWS Resources Setup (IAM, VPC & EC2)](#aws-resources-setup-iam-vpc--ec2)
    - [MSK Cluster Configuration](#msk-cluster-configuration)
    - [Kafka Setup on Client Machine (EC2)](#kafka-setup-on-client-machine-ec2)
    - [S3](#s3)



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

## Installation Instruction

### Prerequisites
Ensure that the following prerequisites are met before proceeding with the Pinterest Data Pipeline project:

- Python 3+
- Required Python Packages: SQLAlchemy, PyMySQL
- Proficiency in Linux OS and familiarity with AWS services

To begin, clone the repository to your local machine using the following command:

```bash
git clone https://github.com/MosElAgab/pinterest-data-pipeline636.git
```

### AWS Resources Setup (IAM, VPC & EC2)
This step initialises the setup for your local environment and sets the stage for further configuration and execution of the data pipeline.

Setting up the AWS environment constitutes a fundamental step in deploying the data pipeline. In this project, the region has been designated as `us-east-1`. Follow these meticulous steps to ensure a secure and fully functional setup.

**Step 1: Identity and Access Management (IAM)**

- Create an IAM user adhering to the Principle of Least Privilege.

> Note: The IAM user's username will be denoted as `<UserID>` in line with common naming conventions observed in software development lifecycles.

- Establish an IAM Role named `<UserID>-ec2-access-role`.
    - Attach policies granting access to services such as VPC, EC2, S3, and MSK.
    - Make a careful note of this role's ARN, which will be represented as `<awsEC2RoleARN>`.

For enhanced security and streamlined resource management in AWS, it is strongly recommended to employ the IAM user created for this project instead of the root user for all subsequent operations. This practice aligns with the principle of least privilege, facilitates precise auditing, and significantly mitigates the risk of unauthorized access or inadvertent disruptions to the system.

**Step 2: VPC Setup**

- Generate a VPC and outline the subnets and security group for effective traffic control.

**Step 3: EC2 Instance Initialization**

- Craft an EC2 instance within the previously defined subnet and securely store the `.pem` file locally after creation. This file can also be retrieved from the AWS Console.

**Step 4: Configure EC2 Client for AWS IAM MSK Cluster Authentication**

- Navigate to the IAM console and, under “Roles,” select the recently created role.
- Within the **Trust Relationships** section in the `<UserID>-ec2-access-role` Role on AWS Console, click on **Edit trust policy**.
- Add a principal by selecting **IAM roles** as the Principal type and inserting `<awsEC2RoleARN>` when prompted.
- Ensure the trust policy mirrors the configuration shown below:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": [
            "ec2.amazonaws.com",
            "kafkaconnect.amazonaws.com"
        ],
        "AWS": "<awsEC2RoleARN>"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```
Note: These steps are crucial as they enable IAM authentication to the MSK cluster, ensuring a robust and secure authentication mechanism for seamless integration with AWS resources.

**Step 5: Launch EC2 Instance using SSH client**

- Ensure the EC2 key-pair is acquired and use it to launch an instance on your local machine using an SSH client while being in the directory with the key-pair `.pem` file.

> Make a note of the EC2 instance Public IPv4 DNS, denoted as `<EC2-Public-DNS>`.

### MSK Cluster Configuration

- Create an MSK Cluster on the AWS console called `pinterest-msk-cluster`. This service is used to build and run applications that leverage Apache Kafka for data processing.
- Apache Kafka Version 2.8.1 was utilized, with a total of 3 brokers and 1 broker per zone.

> Take note of the **Bootstrap servers string** and **Plaintext Apache Zookeeper connection** after creating the cluster.

**Optional:** Obtain this information by running the following commands after replacing the `ClusterArn` with the information found in the MSK Cluster console.

```bash
aws kafka describe-cluster --cluster-arn ClusterArn
aws kafka get-bootstrap-brokers --cluster-arn ClusterArn
```

### Kafka Setup on Client Machine (EC2)
**Step 1: Install Java and download Kafka**

```bash
sudo yum install java-1.8.0
```

```bash
wget https://archive.apache.org/dist/kafka/2.8.1/kafka_2.12-2.8.1.tgz
tar -xzf kafka_2.12-2.8.1.tgz
```

**Step 2: Install the IAM MSK authentication package**

```bash
wget https://github.com/aws/aws-msk-iam-auth/releases/download/v1.1.5/aws-msk-iam-auth-1.1.5-all.jar
```

**Step 3: Configure environment variable in `bash.rc`**

Add the following line in the `bash.rc` file and apply the changes to the current session by running: `source ~/.bashrc`

```bash
export CLASSPATH=/home/ec2-user/kafka_2.12-2.8.1/libs/aws-msk-iam-auth-1.1.5-all.jar
```

**Step 4: Configure Kafka client to use AWS IAM**

Navigate to `kafka_2.12-2.8.1/bin` and create a `client.properties` file with the following information:

```bash
# Sets up TLS for encryption and SASL for authN.
security.protocol = SASL_SSL

# Identifies the SASL mechanism to use.
sasl.mechanism = AWS_MSK_IAM

# Binds SASL client implementation.
sasl.jaas.config = software.amazon.msk.auth.iam.IAMLoginModule required awsRoleArn="<awsEC2RoleARN>";

# Encapsulates constructing a SigV4 signature based on extracted credentials.
# The SASL client bound by "sasl.jaas.config" invokes this class.
sasl.client.callback.handler.class = software.amazon.msk.auth.iam.IAMClientCallbackHandler
```
> Replace `<awsEC2RoleARN>` with the appropriate string saved previously.

**Step 5: Kafka Topics Creation**

Create 3 topics where each will receive specific data:

- `<UserID>.pin` for Pinterest posts data
- `<UserID>.geo` for post geolocation data
- `<UserID>.user` for post user data

Navigate to `kafka_2.12-2.8.1/bin` and run the following command:

```bash
./kafka-topics.sh --bootstrap-server <BootstrapServerString> --command-config client.properties --create --topic <topic_name>
```
> Replace **BootstrapServerString** with the connection string previously saved and `<topic_name>` with the specified name.

**Optional:** Run the following command to have a Kafka Consumer to check incoming messages:

```bash
./kafka-console-consumer.sh --bootstrap-server <BootstrapServerString> --consumer.config client.properties --group students --topic <topic_name> --from-beginning
```

