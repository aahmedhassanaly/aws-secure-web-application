# AWS Secure Web Application

**Portfolio Priority: #6 — Cloud Infrastructure**

A practical AWS infrastructure project demonstrating a secure web-application architecture with networking, compute, database, storage, IAM, monitoring, backup, cost control, and troubleshooting.

## Architecture

<img width="1293" height="852" alt="AWS secure web application architecture" src="https://github.com/user-attachments/assets/429d925b-ea7d-4b49-a793-918c2019ce49" />

```text
Internet
   |
   v
Application Load Balancer
   |
   +--------+--------+
   |                 |
  EC2               EC2
   |                 |
   +--------+--------+
            |
            v
      Private Aurora
        MySQL DB

S3 (Private)
CloudWatch
IAM
Budgets
```

## AWS Services

- Amazon VPC
- Amazon EC2
- Application Load Balancer
- Amazon Aurora MySQL
- Amazon S3
- AWS IAM
- Amazon CloudWatch
- AWS Budgets

## Network Design

- VPC: `10.0.0.0/16`
- Region: Canada Central (`ca-central-1`)
- Internet Gateway for public connectivity
- NAT Gateway for private-subnet outbound access
- Two public subnets
- Two private subnets
- Database kept private

### Public Subnets

| Subnet | CIDR | AZ |
|---|---|---|
| public-subnet-1 | 10.0.1.0/24 | ca-central-1a |
| public-subnet-2 | 10.0.5.0/24 | ca-central-1b |

### Private Subnets

| Subnet | CIDR | AZ |
|---|---|---|
| private-subnet-1 | 10.0.2.0/24 | ca-central-1a |
| private-subnet-2 | 10.0.3.0/24 | ca-central-1b |

## Security Model

```text
Internet
   |
  ALB
   |
  EC2
   |
 Aurora MySQL
```

Security Groups enforce the traffic path:

- ALB: HTTP 80 from the internet
- EC2: HTTP 80 only from `alb-sg`
- EC2 SSH: TCP 22 only from the administrator's public IP
- Aurora: TCP 3306 only from `web-server-sg`

Additional controls:

- Root account MFA
- IAM user and groups for normal administration
- Least-privilege approach
- S3 Block Public Access
- S3 encryption
- S3 versioning
- Private database access

## Monitoring & Backup

CloudWatch CPU monitoring was configured with:

- Metric: `CPUUtilization`
- Period: 5 minutes
- Threshold: >70%
- Evaluation: 1 datapoint

Aurora automated backups were verified with:

- Backup retention: 1 day
- Encryption: enabled
- Completed snapshot status: `available`

## Verification & Troubleshooting

The project verified:

- DNS resolution
- Network connectivity
- Security Group behavior
- TCP/3306 database connectivity
- ALB listener and target health
- HTTP forwarding to the EC2 web servers

A real ALB connectivity problem was investigated using:

**Problem → Evidence → Hypothesis → Test → Fix → Verify**

### Root Cause

The ALB initially used `web-server-sg` instead of the dedicated `alb-sg`, preventing expected inbound HTTP traffic.

### Fix

The dedicated `alb-sg` was attached to the ALB. The ALB then became reachable and forwarded traffic to healthy EC2 targets.

## Tasks

| # | Task | Status |
|---:|---|:---:|
| 001 | [IAM & MFA](documentation/001-iam-mfa.md) | ✅ |
| 002 | [VPC Networking](documentation/002-vpc.md) | ✅ |
| 003 | [Secure Web Server](documentation/003-secure-web-server.md) | ✅ |
| 004 | [Aurora MySQL Database](documentation/004-RDS-database.md) | ✅ |
| 005 | [Application Load Balancer](documentation/005-load-balancer.md) | ✅ |
| 006 | [Monitoring & Backup](documentation/006-monitoring-backup.md) | ✅ |
| 007 | [Final Security & Cost Review](documentation/007-final-security-cost-review.md) | ✅ |

Detailed implementation notes: [Documentation](documentation/).

## Cost Control

The lab reviewed potentially chargeable resources including Aurora, EC2, ALB, NAT Gateway, and CloudWatch.

The Aurora cluster used two `db.r7g.large` instances and was the main cost consideration. Resources were intended to be removed after final verification to avoid unnecessary charges.

## Project Status

**Implemented and tested — final resource cleanup is required after verification.**

