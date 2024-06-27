# Fetch_Assesement
# Fetch Rewards Data Engineering Take Home

## Project Overview

This project reads JSON data from an AWS SQS Queue, processes the data to mask PII fields, and writes the processed data to a Postgres database. The environment is set up using Docker and Docker Compose for local testing.

## Prerequisites

- Docker
- Docker Compose
- Python 3.7+
- pip (Python package installer)
- awscli-local
- psql (Postgres command-line tool)

## Setup Instructions

1. Clone the repository:
    ```sh
    git clone <your-repo-url>
    cd <your-repo-directory>
    ```

2. Run Docker Compose:
    ```sh
    docker-compose up -d
    ```

3. Install Python Dependencies:
    ```sh
    pip install boto3 psycopg2-binary awscli-local
    ```

4. Test Local Access:
    - Read a message from the queue using `awslocal`:
    ```sh
    awslocal sqs receive-message --queue-url http://localhost:4566/000000000000/login-queue
    ```
    - Connect to the Postgres database and verify the table is created:
    ```sh
    psql -d postgres -U postgres -p 5432 -h localhost -W
    postgres=# select * from user_logins;
    ```

5. Run the ETL Script:
    ```sh
    python etl.py
    ```

## Thought Process

1. Reading Messages from SQS:
    - Use `boto3` with `awscli-local` to interact with the local SQS instance.

2. Data Structures:
    - Use Python dictionaries to handle JSON data.

3. Masking PII Data:
    - Use a hashing function (e.g., SHA256) to mask `device_id` and `ip` while ensuring duplicate values produce the same masked output.

4. Writing to Postgres:
    - Use `psycopg2` to connect to the Postgres database and insert data.

5. Running the Application:
    - The application is designed to run locally for testing purposes but can be adapted for production with proper deployment strategies.

## Next Steps

- Production Deployment:
    - Use AWS services like ECS or EKS for container orchestration.
    - Implement monitoring and logging with AWS CloudWatch.
    - Use IAM roles and security groups for access control.

- Scaling:
    - Implement auto-scaling for handling a growing dataset.
    - Use RDS for Postgres to manage database scaling and backups.

- PII Recovery:
    - Store the original PII data in a secure, encrypted storage if recovery is necessary.
    - Implement access controls to ensure only authorized personnel can decrypt and access PII.

- Assumptions:
    - The input JSON structure is consistent.
    - The local development setup mimics the production environment.
