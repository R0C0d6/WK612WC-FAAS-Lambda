
---

## Serverless & Event-Driven Architecture on AWS. Function-as-a-Service (FaaS) Using AWS Lambda
![Architecture Diagram](https://i.postimg.cc/8cqg3zbF/Whats-App-Image-2025-12-17-at-11-25-08.jpg)

---

## 📌 Table of Contents

1. Introduction
2. What This Project Is About
3. What “Serverless” Really Means
4. What “Event-Driven” Really Means
5. Services Used in This Project
6. Architecture Overview
7. Prerequisites (What You Need Before Starting)
8. AWS Account Setup
9. Step 1 – Creating the Lambda Function
10. Step 2 – Writing and Deploying Lambda Code
11. Step 3 – Testing Lambda Manually
12. Step 4 – Creating API Gateway (HTTP Endpoint)
13. Step 5 – Connecting API Gateway to Lambda
14. Step 6 – Testing the API Endpoint
15. Step 7 – Creating an S3 Bucket (Event Trigger)
16. Step 8 – Connecting S3 Events to Lambda
17. Step 9 – Testing S3 Event-Driven Execution
18. Step 10 – Creating an SQS Queue
19. Step 11 – Connecting SQS to Lambda
20. Step 12 – Testing SQS Event-Driven Execution
21. Step 13 – Monitoring with CloudWatch Logs
22. Automatic Scaling Explained
23. Cost Awareness (Very Important)
24. Screenshots Required for Submission
25. Common Errors and Fixes
26. Final Deliverables Checklist
27. What You Have Learned
28. Next Improvements and Extensions

---

## 1️⃣ Introduction
This repository contains a **complete beginner-friendly guide** to building a **real, production-style serverless backend** using **Amazon Web Services (AWS)**.

We build a backend that:
* Runs **without servers**
* Reacts automatically to **events**
* Scales automatically without configuration
* Logs everything for monitoring

---

## 2️⃣ What This Project Is About
We will create:
* A **Lambda function**
* A **public API endpoint** using API Gateway
* **Event triggers** using S3 and SQS
* **Monitoring logs** using CloudWatch
All of this works together to form an **event-driven backend system**.

---

## 3️⃣ What “Serverless” Really Means (Very Simple)

### ❌ What Serverless Is NOT
* It does **not** mean there are no computers
* It does **not** mean magic

### ✅ What Serverless IS
* You do **not manage servers**
* You do **not install operating systems**
* You do **not handle scaling**
* You only write **code**

AWS handles:
* Servers
* Scaling
* Patching
* Availability
* Security infrastructure

You only care about:
```text
WHEN something happens → RUN my code
```

---

## 4️⃣ What “Event-Driven” Really Means

An **event** is simply:
> “Something happened”
Examples:
* A user clicks a button
* A file is uploaded
* A message is sent
* An API is called

In this project:
* HTTP request → Event
* File upload → Event
* Queue message → Event

Each event **automatically triggers Lambda**.

---

## 5️⃣ Services Used in This Project

| AWS Service       | Purpose                                 |
| ----------------- | --------------------------------------- |
| AWS Lambda        | Runs your code without servers          |
| API Gateway       | Exposes Lambda as an HTTP API           |
| Amazon S3         | Triggers Lambda when files are uploaded |
| Amazon SQS        | Triggers Lambda when messages arrive    |
| Amazon CloudWatch | Logs and monitors everything            |

---

## 6️⃣ Architecture Overview

### High-Level Flow

```
User → API Gateway → Lambda → CloudWatch
File Upload → S3 → Lambda → CloudWatch
Message → SQS → Lambda → CloudWatch
```

### Key Characteristics

* Serverless
* Event-driven
* Automatically scalable
* Pay-per-use
* No server management

---

## 7️⃣ Prerequisites (What You Need)

Before starting, you must have:

✅ An AWS Account
✅ Internet connection
✅ Web browser (Chrome recommended)
✅ Basic typing skills

❌ No programming experience required
❌ No server knowledge required

---

## 9️⃣ Step 1 – Creating the Lambda Function
### Open Lambda
1. In AWS Console search for **Lambda**
2. Click **Lambda**
3. Click **Create function**

### Configure Function

* Author from scratch
* Function name. e.g.:
  ```
  week6-serverless-function
  ```
* Runtime. e.g.:

  ```
  Python 3.12
  ```
* Architecture:
  ```
  x86_64
  ```

![lambda](https://i.postimg.cc/RFxBXFXN/Screenshot-2025-12-18-181429.png)

Click **Create function**

![lambda](https://i.postimg.cc/nrdNbcYf/Screenshot-2025-12-16-175351.png)

---

## 🔟 Step 2 – Writing and Deploying Lambda Code

Replace default code with:

```python
import json

def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": json.dumps({
            "message": "Hello! This Lambda function is working!",
            "event_received": event
        })
    }
```
![lambda](https://i.postimg.cc/SNJvY4fz/Screenshot-2025-12-16-175853.png)

Click **Deploy**

---

## 1️⃣1️⃣ Step 3 – Testing Lambda Manually

1. Click **Test**
2. Create new test event
3. Name it `test-event`
4. Click **Test**

Expected result:

```
Status: Succeeded
```
![lambda](https://i.postimg.cc/NFVbkykN/Screenshot-2025-12-16-180029.png)

---

## 1️⃣2️⃣ Step 4 – Creating API Gateway

1. Search **API Gateway** or within Lambda, create a trigger
2. Click **Create API**
3. Choose **HTTP API**
4. Click **Build**

![lambda](https://i.postimg.cc/J792Xg6q/Screenshot-2025-12-16-182444.png)

---

## 1️⃣3️⃣ Step 5 – Connecting API Gateway to Lambda
Set API name
Default deployment stage
![lambda](https://i.postimg.cc/4Ng1sTFz/Screenshot-2025-12-16-182512.png)

* Integration target: **Lambda**
* Lambda function:
  ```
  week6-serverless-function
  ```


Click **Create**

![lambda](https://i.postimg.cc/3NH28g1B/Screenshot-2025-12-16-182532.png)

---

## 1️⃣4️⃣ Step 6 – Testing the API Endpoint
1. Copy Invoke URL/API Endpoint
2. Paste into browser
![lambda](https://i.postimg.cc/m2gtQXPq/Screenshot-2025-12-16-182659.png)

You should see JSON response.

![lambda](https://i.postimg.cc/2y887ySS/Screenshot-2025-12-16-182711.png)

---

## 1️⃣5️⃣ Step 7 – Creating an S3 Bucket

1. Open **S3**
2. Click **Create bucket**
3. Name. e.g.:
   ```
   week6-event-bucket-unique
   ```
  ![S3](https://i.postimg.cc/6QtsvS3t/Screenshot-2025-12-16-183237.png)
  
4. Same region as Lambda
5. Click **Create bucket**
![S3](https://i.postimg.cc/8zd9NHHG/Screenshot-2025-12-16-183345.png)
---

## 1️⃣6️⃣ Step 8 – Connecting S3 Events to Lambda

Update Lambda code. e.g.:

```python
def lambda_handler(event, context):
    record = event['Records'][0]
    bucket = record['s3']['bucket']['name']
    file = record['s3']['object']['key']

    return {
        "statusCode": 200,
        "body": f"File {file} uploaded to {bucket}"
    }
```

![S3](https://i.postimg.cc/vB1dYnT3/Screenshot-2025-12-16-194454.png)


Add trigger:
* Service: S3
* Event: Object Created
![S3](https://i.postimg.cc/RhzpkJH5/Screenshot-2025-12-16-184926.png)
---



## 1️⃣7️⃣ Step 9 – Testing S3 Event Trigger

Upload any file to the bucket.
![Testing S3 trigger](https://i.postimg.cc/2ywGgcYM/Screenshot-2025-12-16-185909.png)

Lambda runs automatically. Cloudwatch logs it
![Testing S3 trigger](https://i.postimg.cc/zBqR4JfN/Screenshot-2025-12-16-195538.png)
![Testing S3 trigger](https://i.postimg.cc/MHvX8X2d/Screenshot-2025-12-16-195615.png)

---

## 1️⃣8️⃣ Step 10 – Creating an SQS Queue

1. Open **SQS**
2. Click **Create queue**
3. Type: Standard
4. Name:

   ```
   week6-event-queue
   ```
![Testing S3 trigger](https://i.postimg.cc/Pxq5mNxk/Screenshot-2025-12-16-200131.png)

Click Create
![Testing S3 trigger](https://i.postimg.cc/90RXnSZS/Screenshot-2025-12-16-200215.png)


---

## 1️⃣9️⃣ Step 11 – Connecting SQS to Lambda

1. Open Lambda
2. Add trigger
3. Select SQS
4. Choose queue
![Testing S3 trigger](https://i.postimg.cc/8zDJ7phG/Screenshot-2025-12-16-200333.png)

Click create trigger
![Testing S3 trigger](https://i.postimg.cc/sgVxVnr9/Screenshot-2025-12-16-201606.png)

---

Create a role, name it, and attach the AWSLambdaSQSQueueExecutionRole policy
![Testing S3 trigger](https://i.postimg.cc/90WK4qFY/Screenshot-2025-12-16-201737.png)


## 2️⃣0️⃣ Step 12 – Testing SQS Trigger

Send message:

```
Hello Lambda from SQS!
```

Lambda runs automatically.
![Testing S3 trigger](https://i.postimg.cc/15d7Sd6K/Screenshot-2025-12-16-203241.png)

---

## 2️⃣1️⃣ Step 13 – Monitoring with CloudWatch

1. Open **CloudWatch**
2. Go to **Logs**
3. Open log group:

   ```
   /aws/lambda/week6-serverless-function
   ```

You will see logs for:
* API calls
* S3 uploads
* SQS messages
* 
![Testing S3 trigger](https://i.postimg.cc/rmqQg2Xr/Screenshot-2025-12-16-202837.png)


---



## 2️⃣2️⃣ Automatic Scaling Explained

If:

* 1 request → 1 Lambda
* 1000 requests → 1000 Lambdas

No configuration required.

---

## 2️⃣3️⃣ Cost Awareness

AWS Lambda:

* Free tier available
* Pay only when function runs
* Stop services when finished

---

## 2️⃣5️⃣ Common Errors and Fixes

| Error                | Fix             |
| -------------------- | --------------- |
| Lambda not triggered | Check trigger   |
| API returns 500      | Check logs      |
| Wrong region         | Use same region |

---

## 2️⃣8️⃣ Next Improvements

* Add SNS notifications
* Add IAM security policies
* Add DynamoDB
* Add frontend UI
* Add error handling

---

