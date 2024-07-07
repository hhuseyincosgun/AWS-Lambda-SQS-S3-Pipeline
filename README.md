# AWS Lambda, SQS, and S3 Workflow

![image](https://github.com/hhuseyincosgun/AWS-Lambda-SQS-S3-Pipeline/assets/21257660/e184a6cd-59d6-4b7a-a751-5345e00ea4d2)


This repository contains the code and configuration for an AWS workflow using Lambda, SQS, and S3. The workflow is designed to process messages and store the output as CSV files in an S3 bucket.

The URL is being used to fetch financial data for a specific stock or symbol from the [Mynet Finans](https://finans.mynet.com/borsa/hisseler/) website. Here's a detailed explanation of why and how it is used:

![image](https://github.com/hhuseyincosgun/AWS-Lambda-SQS-and-S3-Workflow/assets/21257660/87c15acb-0c08-438e-916c-0ed847294ad1)

```bash
body = json.loads(event['Records'][0]['body'])
symbol = body[0]
urlpart = body[1]

url = f'https://finans.mynet.com/borsa/hisseler/{urlpart}'
```
### Workflow Steps

1. **Step 1: Lambda Function (Step_1)**
   - **Role:** Initiates the process by sending messages to an SQS queue.
   - **Permissions:** 
     - `AmazonSQSFullAccess`
   - **SQS URL:** 
     - `https://sqs.eu-north-1.amazonaws.com/891377159635/step1`

2. **Step 2: SQS Queue (step1)**
   - **Role:** Acts as a buffer, holding messages sent by Step_1 and ensuring they are delivered to Step_3.
   - **Trigger:** Configured to trigger the Lambda function in Step 3.

3. **Step 3: Lambda Function (Step_3)**
   - **Role:** Processes messages from the SQS queue, performing tasks like extracting, transforming, and loading data into an S3 bucket.
   - **Trigger Configuration:** 
     - Maximum concurrency: 2
   - **Permissions:** 
     - `AmazonSQSFullAccess`
     - `AmazonS3FullAccess`

4. **Amazon S3 Bucket**
   - **Role:** Stores the processed data as CSV files.
   - **Files:** Example files include `A1CAP.csv`, `ACSEL.csv`, `ADEL.csv`, etc.
   - **Data Structure:** Each CSV file contains columns such as `date`, `high`, `low`, `close`, and `volume`.

## Getting Started

### Prerequisites

- AWS account with necessary permissions
- AWS CLI configured
- Python 3.x installed
