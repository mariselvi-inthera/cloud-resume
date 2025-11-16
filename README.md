# **Cloud Resume Challenge — Full Stack AWS Project**

*Serverless Resume Website with Visitor Counter, CI/CD, and AWS Infrastructure*

---

## **📌 Overview**

The **Cloud Resume Challenge** is an end-to-end cloud engineering project that demonstrates practical skills with **AWS serverless services, CI/CD automation, and full-stack cloud architecture**.

This implementation includes:

* Static resume website hosted on **Amazon S3**
* Global HTTPS delivery with **CloudFront CDN**
* Serverless backend: **API Gateway → Lambda → DynamoDB**
* Persistent visitor counter displayed on frontend
* **GitHub Actions CI/CD pipeline** to auto-deploy changes
* Professional documentation & production-ready architecture

---

## **🚀 Architecture Diagram**

```
 User
  |
  |--> CloudFront (HTTPS)
          |
          +--> S3 (Static Website)
          
  |
  |--> API Gateway (GET /counter)
          |
          +--> Lambda (Python)
                    |
                    +--> DynamoDB (visitor_count)
```

---

## **🧰 Tech Stack**

### **Frontend**

* HTML, CSS, JavaScript
* Hosted on **Amazon S3**
* Delivered globally via **CloudFront**

### **Backend**

* **AWS Lambda (Python)**
* **Amazon DynamoDB**
* **API Gateway REST API**

### **DevOps / CI/CD**

* **GitHub Actions**
* Automatic deployment to S3
* CloudFront cache invalidation

---

## **📁 Folder Structure**

```
cloud-resume/
│
├── frontend/
│     ├── index.html
│     ├── styles.css
│     ├── script.js
│     ├── images/
│     └── assets/
│
├── backend/
│     ├── lambda_function.py
│     └── requirements.txt
│
├── .github/
│     └── workflows/
│           └── main.yml
│
└── README.md
```

---

## **⚙️ AWS Resources Created**

### **Frontend**

* S3 bucket: `cloud-resume-static-site`
* CloudFront distribution with HTTPS
* Origin Access Control (OAC)

### **Backend**

* DynamoDB table: `visitor_count`
* Lambda function: `resume-counter-lambda`
* API Gateway REST API

### **IAM**

* Policies for:

  * Lambda execution
  * DynamoDB access
  * GitHub Actions deploy

---

## **✔️ Visitor Counter API**

### DynamoDB Table

| id     | count |
| ------ | ----- |
| resume | 0     |

### Lambda (Python)

```python
import boto3
import json

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('visitor_count')

def lambda_handler(event, context):
    response = table.update_item(
        Key={'id': 'resume'},
        UpdateExpression='ADD #count :inc',
        ExpressionAttributeNames={'#count': 'count'},
        ExpressionAttributeValues={':inc': 1},
        ReturnValues="UPDATED_NEW"
    )

    return {
        'statusCode': 200,
        'headers': {"Access-Control-Allow-Origin": "*"},
        'body': json.dumps({'count': response['Attributes']['count']})
    }
```

---

## **📡 Frontend Integration**

`script.js`:

```javascript
fetch("https://YOUR_API.execute-api.us-east-1.amazonaws.com/prod/resume")
  .then(res => res.json())
  .then(data => {
      document.getElementById("counter").innerText = data.count;
  });
```

---

## **🔄 CI/CD — GitHub Actions**

`.github/workflows/main.yml`

```yaml
name: Deploy Resume to S3

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3

      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Sync files to S3
        run: aws s3 sync frontend/ s3://cloud-resume-static-site --delete
```

Store these secrets in repo settings:

* `AWS_ACCESS_KEY_ID`
* `AWS_SECRET_ACCESS_KEY`

---

## **🧪 How to Test**

### 1. Open CloudFront URL

```
https://xxxxxxx.cloudfront.net
```

### 2. Check visitor count updates

Refresh the page → number should increase.

### 3. Push frontend changes

GitHub Actions auto-deploys to S3.

---

## **📦 Deployment Steps (Summary)**

1. Create S3 bucket → upload frontend
2. Create CloudFront distribution
3. Create DynamoDB table
4. Create Lambda function (Python)
5. Create API Gateway → connect Lambda
6. Integrate frontend with API
7. Configure GitHub Actions for CI/CD
8. Deploy → Test → Document

---

## **📜 Features Completed**

✔ Static resume website
✔ Global HTTPS delivery
✔ Serverless API for visitor count
✔ DynamoDB persistent storage
✔ Lambda-based backend logic
✔ Clean folder structure
✔ CI/CD pipeline
✔ Fully automated deployment
✔ Developer documentation

---

## **📌 Lessons Learned**

* Working with AWS IAM & permissions
* Configuring S3 for static hosting
* Integrating Lambda + DynamoDB
* Enabling CORS correctly
* Designing serverless backend
* Setting up CI/CD pipelines
* Writing production-focused documentation

---

## **🤝 Contributions**

Feel free to fork, improve, or reuse this project.

---
