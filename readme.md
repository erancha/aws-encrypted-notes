# EncNotes Web App

## 1. Overview

EncNotes is a serverless web application built with AWS, React, and WebSockets. The application allows authenticated users to add, edit, delete and list notes through a web-based interface, isolated with a different encryption key per user.  
The application is available online at https://d8z8oboi5znfz.cloudfront.net.

## 2. Architecture Components

![Architecture diagram](https://lucid.app/publicSegments/view/51fb6369-9f6c-4b20-8c75-bc90b2b16a49/image.jpeg)

### 2.1 Frontend

- **React**-based Single Page Application (SPA)
- Hosted on AWS **S3**
- Accessed via AWS **CloudFront** for global content delivery

### 2.2 Backend

- AWS **Lambda** functions (Node.js) for serverless compute
- AWS **API Gateway** for **REST**ful and **WebSocket** APIs
- AWS **DynamoDB** for serverless database storage
- AWS Key Management Service (**KMS**) for encryption key management
- AWS **Elasticache Redis** for caching users data keys and connected devices
- AWS **SQS** for decoupling of notes management from notifications to connected devices

### 2.3 Authentication & Authorization

- AWS **Cognito** for user authentication and authorization, with **Google** federated authentication

## 3. Key Features

- **List, Add, edit and delete** encrypted notes
- **Secure storage** of encrypted notes, with a different data encryption key per user (i.e. a user can never view notes of other users)
- **Notifications** about modifications between devices of a user (meaningful for accounts shared between different users)

## 4. Data Flow

1. User interacts with the React-based frontend hosted on CloudFront from S3
2. Frontend makes API calls to AWS API Gateway
3. API Gateway authenticates users with Cognito and triggers appropriate Lambda functions
4. Lambda functions interact with:
   - DynamoDB to store/retrieve encrypted notes and encrypted DEKs
   - KMS to generate DEKs
   - Elasticache to manage plaintext DEKs and connection ids of other connected devices of the current user
   - SQS to send notifications to the connected devices thru WebSocket API gateway

## 5. Security Considerations

- All data in transit is encrypted using HTTPS
- Notes are encrypted at rest in DynamoDB using KMS
- User authentication is handled by AWS Cognito
- Lambda functions and Elasticache deployed into a private subnet
- Least privilege principle applied to IAM roles

## 6. Deployment

- AWS SAM (Serverless Application Model) is used for deployment
- CloudFormation templates define the infrastructure as code
- Single command deployment using `sam build` and `sam deploy`
- The application is available online at https://d8z8oboi5znfz.cloudfront.net.

## 7. Scalability & Performance

- Serverless architecture allows automatic scaling
- CloudFront ensures low-latency content delivery
- DynamoDB provides consistent performance at any scale

## 8. Monitoring & Logging

- AWS CloudWatch for monitoring and logging

## 9. Cost Optimization

- Pay-per-use model with serverless components
- Elasticache and VPC resources incur hourly costs
