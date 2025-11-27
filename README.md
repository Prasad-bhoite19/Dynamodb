# 📘 DynamoDB Project README

Created by Prasad

## 🧭 Overview

Amazon DynamoDB is a fully managed NoSQL database service that delivers single‑digit millisecond performance at any scale. This README provides a complete professional guide for using DynamoDB in real‑world cloud & DevOps environments.

## 📂 Table of Contents

- Introduction
- Architecture Diagram
- Key Concepts
- DynamoDB Components
- Data Modeling
- Provisioned vs On‑Demand Capacity
- Indexing (GSI, LSI)
- Streams & Event-Driven Patterns
- Security & IAM
- Backup & Restore
- DynamoDB Accelerator (DAX)
- Pricing Breakdown
- AWS Console Guide
- AWS CLI Guide
- CloudFormation Template
- CDK Sample Code
- CRUD Operations Examples
- Best Practices
- Performance Optimization
 - Monitoring & Observability
- Troubleshooting Guide
- Real‑World Use Cases
- Common Interview Questions
- Future Enhancements
- Author

## 📝 Introduction

Amazon DynamoDB is a key‑value and document database known for being:

- Fully managed
- Serverless
- Highly available
- Highly scalable
- Cost‑effective

## 🏗️ Architecture Diagram
```
[ Client Apps ]
       |
       v
  API Gateway / Lambda ----> DynamoDB Table
       |
       v
    Streams ---> Lambda Consumers ---> S3 / Redshift / OpenSearch
```
## 🔑 Key Concepts

- Table – Collection of items
- Item – Group of attributes (like a row)
- Attribute – Individual data element (column)
- Partition Key – Determines item distribution
- Sort Key – Determines sort order within partitions
- GSI/LSI – Secondary indexes for queries

## 🧩 Components

- DynamoDB Tables
- Global Secondary Indexes
- Local Secondary Indexes
- Streams
- DAX (Caching)
- TTL (Time to Live)
- Encryption (KMS)
- Automatic Scaling

## 🏛️ Data Modeling Strategy

- Use single‑table design
- Model access patterns first
- Prefer denormalization
- Use composite keys
- Store related items in the same partition
  
Example partition key strategy:

`USER#12345
ORDER#98765`

## ⚙️ Provisioned vs On‑Demand

- Provisioned Mode
- Good for predictable workloads -- Autoscaling available
- On‑Demand Mode
- Pay per request
- Best for unpredictable workloads
  
## 🔍 Indexing

- GSI (Global Secondary Index)
- Different partition/sort key
- Used to support additional query patterns
- LSI (Local Secondary Index)
- Same partition key, different sort key

## 🔄 DynamoDB Streams

- Enables building event‑driven architectures.

Use cases:

- Change data capture
- Trigger Lambda functions
- Auditing
- Replication

## 🔐 Security & IAM

- Enable KMS encryption
- Use least‑privilege IAM policies
- Use VPC Endpoints for private traffic
  
Example policy:
```
{
  "Effect": "Allow",
  "Action": ["dynamodb:PutItem", "dynamodb:GetItem"],
  "Resource": "arn:aws:dynamodb:us-east-1:123456789012:table/MyTable"
}
```
## 💾 Backup & Restore

- Point‑in‑Time Recovery
- On‑demand backups
- Export to S3

## ⚡ DAX (DynamoDB Accelerator)

Benefits:

- Caching layer
- Microsecond latency
- Great for read‑heavy applications

## 💰 Pricing Breakdown

Pricing components:

- Read/write request units
- Storage cost
- GSI cost
- Streams cost
- Data transfer cost

## 🖥️ AWS Console Guide

- Go to DynamoDB Console
- Create a new table
- Define partition & sort keys
- Enable PITR, TTL, encryption
- Optionally create GSI/LSI

## 🧪 AWS CLI Guide

Create a table via CLI:
```
aws dynamodb create-table \
  --table-name Users \
  --attribute-definitions \
      AttributeName=UserId,AttributeType=S \
  --key-schema \
      AttributeName=UserId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```
Describe a table:

```aws dynamodb describe-table --table-name Users```

## 🧱 CloudFormation Template
```
Resources:
  UsersTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: Users
      AttributeDefinitions:
        - AttributeName: UserId
          AttributeType: S
      KeySchema:
        - AttributeName: UserId
          KeyType: HASH
      BillingMode: PAY_PER_REQUEST
```
## 🧰 CDK Sample Code (TypeScript)
```
import * as dynamodb from 'aws-cdk-lib/aws-dynamodb';


const table = new dynamodb.Table(this, 'Users', {
  tableName: 'Users',
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  partitionKey: { name: 'UserId', type: dynamodb.AttributeType.STRING },
});
```
## 🔧 CRUD Operations (Node.js)
Put Item
```
await client.send(new PutItemCommand({
  TableName: 'Users',
  Item: marshall({ UserId: '101', Name: 'Prasad' })
}));
```
Get Item
```
await client.send(new GetItemCommand({
  TableName: 'Users',
  Key: marshall({ UserId: '101' })
}));
```
## 🏆 Best Practices

- Avoid hot partitions
- Use sparse indexes
- Prefer queries over scans
- Use DAX for caching heavy reads
- Store large blobs in S3, not DynamoDB

## 🚀 Performance Optimization

- Use write‑sharding
- Batch operations
- Tune WCU/RCU
- Use adaptive capacity

## 📡 Monitoring & Observability

CloudWatch metrics

DynamoDB Contributor Insights

X-Ray tracing

CloudTrail logs

🛠️ Troubleshooting Guide
Issue	Cause	Fix
Throttling	Exceeded RCUs/WCUs	Increase capacity / optimize data model
Hot keys	Same partition key heavily used	Add random suffixes
Slow queries	Using Scan	Use Query with index
🌍 Real‑World Use Cases

Session management

E‑commerce carts

Social media timelines

IoT telemetry storage

Gaming leaderboards

❓ Common Interview Questions

Difference between GSI and LSI?

What is DynamoDB TTL?

How does adaptive capacity work?

What is a hot partition?

Explain single‑table design.

🔮 Future Enhancements

Add Terraform module

Add TypeScript SDK examples

Add Serverless Framework templates

Add multi‑region replication example

👤 Author

Prasad – Cloud & DevOps Engineer

⭐ If you like this template, consider reusing it for your AWS projects!
