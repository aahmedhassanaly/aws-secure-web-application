# AWS Secure Web Application

## Project Overview

This project demonstrates the design and implementation of a secure web application infrastructure on AWS.

The project simulates a small company hosting a web application in AWS and focuses on:

* Cloud networking
* Compute
* Database services
* Storage
* Identity and access management
* Security
* Monitoring
* Cost control
* Troubleshooting

## Architecture

![AWS Architecture](architecture/architecture.png)

### AWS Services

* Amazon VPC
* Amazon EC2
* Amazon RDS
* Amazon S3
* Amazon CloudFront
* AWS IAM
* Amazon CloudWatch
* AWS Budgets

## Architecture Overview

The infrastructure will be designed using a VPC with separate public and private subnets.

* EC2 will be used as the application server.
* RDS will provide the managed database.
* S3 will store static website content.
* CloudFront will provide content delivery.
* IAM will control access to AWS resources.
* Security Groups will control network access.
* CloudWatch will provide monitoring and alarms.
* AWS Budgets will be used for cost monitoring.

## Security

The project will follow basic AWS security best practices, including:

* MFA
* Least privilege
* Security Groups
* Private subnet for the database
* Encryption
* Controlled network access

## Monitoring

Amazon CloudWatch will be used to monitor infrastructure resources and create alarms for important metrics.

## Cost Control

AWS Budgets will be configured to monitor estimated AWS spending and provide alerts when defined thresholds are reached.

## Troubleshooting

The project will include practical troubleshooting scenarios involving:

* EC2 connectivity
* Security Groups
* Network routing
* RDS connectivity
* S3 access
* CloudFront access

## Project Status

🚧 In Progress

## Learning Objectives

By completing this project, I aim to demonstrate practical understanding of AWS infrastructure rather than only theoretical knowledge.

## Cleanup

All AWS resources will be reviewed and removed after testing to avoid unnecessary charges.
