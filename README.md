# AWS Secure Web Application

## Project Overview

This project demonstrates the design and implementation of a secure
web application infrastructure on AWS.

The project simulates a small company hosting a web application in AWS
and focuses on:

- Cloud networking
- Compute
- Database services
- Storage
- Identity and access management
- Security
- Monitoring
- Backup
- Cost control
- Troubleshooting

## Architecture
<img width="1293" height="852" alt="image" src="https://github.com/user-attachments/assets/429d925b-ea7d-4b49-a793-918c2019ce49" />


The architecture consists of:

- Internet-facing Application Load Balancer
- Two EC2 web servers
- Private Aurora MySQL database
- VPC with public and private subnets
- Security Groups controlling traffic between components
- Private S3 bucket for static assets
- CloudWatch monitoring and alarms
- Automated Aurora database backups

## AWS Services

- Amazon VPC
- Amazon EC2
- Elastic Load Balancing (Application Load Balancer)
- Amazon Aurora MySQL
- Amazon S3
- AWS IAM
- Amazon CloudWatch
- AWS Budgets

## Network Architecture

A dedicated VPC was created for the application infrastructure.

### VPC

- VPC CIDR: `10.0.0.0/16`
- Region: Canada Central (`ca-central-1`)
- Internet Gateway configured for public connectivity
- NAT Gateway configured for private subnet outbound access

### Public Subnets

Public subnets are used for internet-facing resources such as the
Application Load Balancer and EC2 web servers.

- `public-subnet-1`
  - CIDR: `10.0.1.0/24`
  - Availability Zone: `ca-central-1a`

- `public-subnet-2`
  - CIDR: `10.0.5.0/24`
  - Availability Zone: `ca-central-1b`

### Private Subnets

Private subnets are used for internal resources and database services.

- `private-subnet-1`
  - CIDR: `10.0.2.0/24`
  - Availability Zone: `ca-central-1a`

- `private-subnet-2`
  - CIDR: `10.0.3.0/24`
  - Availability Zone: `ca-central-1b`

The database is not publicly accessible.

## Compute

Two EC2 instances were deployed as web servers:

- `secure-web-server`
- `web-server-2`

Both instances run Apache HTTP Server and listen on TCP port `80`.

The servers are registered as targets behind the Application Load
Balancer.

## Application Load Balancer

An internet-facing Application Load Balancer was deployed:

- Name: `secure-web-alb`
- Listener: HTTP `80`
- Region: Canada Central (`ca-central-1`)
- Scheme: Internet-facing

The ALB distributes incoming HTTP requests between the two EC2
web servers.

### Target Group

Target group:

`fg`

Configuration:

- Target type: Instances
- Protocol: HTTP
- Port: `80`
- Health check path: `/`

Both EC2 instances became healthy after correcting the Security Group
configuration.

## Database

A private Aurora MySQL database was deployed.

Configuration:

- Engine: Aurora MySQL
- Deployment: 2 instances
- Region: Canada Central (`ca-central-1`)
- Public access: Disabled
- Port: `3306`

The database uses private subnets in multiple Availability Zones.

### Database Security

The database uses a dedicated Security Group:

`rds-secure-web-sg`

Inbound MySQL/Aurora traffic is allowed on TCP port `3306` only from:

`web-server-sg`

This prevents direct database access from the public internet.

## Storage

An Amazon S3 bucket was created to store static web assets.

The bucket contains:

- `index.html`
- `style.css`
- `app.js`

The bucket remains private.

Security configuration includes:

- Block Public Access enabled
- ACLs disabled
- Bucket owner enforced
- Server-side encryption enabled
- Versioning enabled

The S3 bucket was not made publicly accessible.

## IAM and Security

AWS IAM was used for normal administrative access.

Security controls implemented include:

- Root account MFA
- IAM user for normal access
- IAM groups
- Least-privilege approach
- Security Groups
- Private database access
- Encryption
- Restricted network access

SSH access to the EC2 instances was restricted to the administrator's
current public IP address instead of allowing access from:

`0.0.0.0/0`

## Security Groups

### ALB Security Group

`alb-sg`

Inbound:

- HTTP `80`
- Source: `0.0.0.0/0`

### EC2 Security Group

`web-server-sg`

Inbound application traffic:

- HTTP `80`
- Source: `alb-sg`

SSH:

- TCP `22`
- Source: Administrator's public IP

### RDS Security Group

`rds-secure-web-sg`

Inbound:

- MySQL/Aurora `3306`
- Source: `web-server-sg`

This creates a layered security model:

Internet → ALB → EC2 → Aurora MySQL

## Monitoring

Amazon CloudWatch was used to monitor the EC2 web server.

A CPU utilization alarm was created:

- Alarm: `secure-web-server-high-cpu`
- Metric: `CPUUtilization`
- Statistic: Average
- Period: 5 minutes
- Threshold: Greater than 70%
- Evaluation: 1 datapoint within 5 minutes

The alarm is configured to send notifications through the
CloudWatch alarms notification topic.

## Database Backup

Automated backups were verified for the Aurora MySQL database.

Configuration:

- Backup retention: 1 day
- Encryption: Enabled
- Automated backup window: `06:49–07:19`

A completed system snapshot was also verified with status:

`available`

This confirmed that the database backup mechanism was functioning.

## Connectivity Testing

The infrastructure was tested from the EC2 environment.

Database connectivity was verified using the MariaDB/MySQL client.

The EC2 instance successfully connected to the private Aurora MySQL
database on TCP port `3306`.

The test verified:

- DNS resolution
- Network connectivity
- Security Group configuration
- TCP port `3306`
- Database authentication

The Application Load Balancer was also tested through its DNS name
and successfully forwarded HTTP traffic to the EC2 web servers.

## Troubleshooting

The project included real troubleshooting scenarios involving:

- EC2 connectivity
- Security Groups
- Network routing
- Network ACLs
- ALB connectivity
- Target health checks
- RDS connectivity
- S3 security
- SSH exposure

### ALB Troubleshooting

The ALB initially failed to respond to HTTP requests.

The following components were investigated:

- DNS resolution
- Route tables
- Internet Gateway
- Network ACL
- ALB listener
- Target group
- EC2 web servers
- Security Groups

The EC2 web servers were confirmed to be running Apache and listening
on port `80`.

### Root Cause

The ALB initially used:

`web-server-sg`

instead of the dedicated:

`alb-sg`

This prevented the ALB from receiving the expected inbound HTTP
traffic.

### Fix

The dedicated `alb-sg` Security Group was attached to the ALB.

After the correction, the ALB became reachable and HTTP requests were
successfully forwarded to the healthy EC2 targets.

The troubleshooting methodology used was:

**Problem → Evidence → Hypothesis → Test → Fix → Verify**

## Cost Control

Potentially chargeable resources were reviewed during the project.

Important cost considerations included:

- Aurora MySQL with two instances
- EC2 instances
- Application Load Balancer
- NAT Gateway
- CloudWatch resources

The Aurora MySQL cluster was the most significant cost consideration
for this lab because it used two `db.r7g.large` instances.

Resources will be removed after final verification to avoid unnecessary
ongoing charges.

## Final Security Review

The final review confirmed:

- Root MFA is enabled
- IAM is used for normal access
- ALB uses a dedicated Security Group
- EC2 HTTP access is restricted to the ALB Security Group
- SSH access is restricted to the administrator's IP
- RDS is private
- RDS access is restricted to the EC2 Security Group
- S3 remains private
- Database encryption is enabled
- Automated database backups are enabled

## Project Status

🚧 Final Cleanup

The infrastructure has been implemented and tested.

The remaining step is to clean up the AWS resources after final
verification to prevent unnecessary charges.

## Learning Objectives

This project demonstrates practical experience with:

- Designing AWS VPC networking
- Working with public and private subnets
- Configuring Internet and NAT Gateways
- Deploying EC2 web servers
- Configuring an Application Load Balancer
- Managing a private Aurora MySQL database
- Implementing Security Group-based access control
- Securing S3 storage
- Managing IAM access
- Monitoring infrastructure with CloudWatch
- Verifying database backups
- Troubleshooting AWS networking and connectivity
- Performing security and cost reviewsractical understanding of AWS infrastructure rather than only theoretical knowledge.

## Cleanup

All AWS resources will be reviewed and removed after testing to avoid unnecessary charges.
