# Task 7 — Final Security and Cost Review

## Objective

Perform a final security and cost review of the AWS infrastructure
before completing the lab.

The review focused on Security Groups, database access, S3 security,
IAM access, and potentially chargeable AWS resources.

## Security Group Review

The Application Load Balancer uses a dedicated Security Group:

`alb-sg`

Inbound HTTP traffic is allowed on:

- TCP port `80`
- Source: `0.0.0.0/0`

The EC2 instances use:

`web-server-sg`

HTTP traffic from the ALB Security Group is allowed on TCP port `80`.

The temporary self-reference HTTP rule that was used during
troubleshooting was removed after the target health issue was resolved.

## SSH Security

SSH access to the EC2 instances was initially open to:

`0.0.0.0/0`

During the final security review, the SSH source was restricted to
the administrator's current public IP address.

This reduces unnecessary exposure of the SSH service to the internet.

## Database Security Review

The Aurora MySQL database is configured with:

- Public access: Disabled
- Port: `3306`
- Dedicated Security Group: `rds-secure-web-sg`

Inbound database access is allowed only from:

`web-server-sg`

The database is deployed in private subnets and is not directly
exposed to the internet.

## S3 Security Review

The S3 bucket containing the static web content remains private.

Security configuration includes:

- Block Public Access: Enabled
- ACLs: Disabled
- Bucket owner enforced
- Server-side encryption enabled

The bucket was not made publicly accessible.

## IAM Security Review

The AWS account was configured to use an IAM user for normal
administrative access instead of the root account.

Root account MFA was enabled.

IAM permissions were reviewed to avoid unnecessary access.

The IAM user and group were retained for the project and documentation.

## Cost Review

The following resources were identified as potentially generating
AWS charges during the lab:

- Aurora MySQL cluster with two instances
- EC2 instances
- Application Load Balancer
- NAT Gateway
- CloudWatch resources

The Aurora MySQL cluster uses two `db.r7g.large` instances and was
identified as the most significant cost consideration for this lab.

The NAT Gateway was also identified as an important resource to
remove during cleanup.

The lab resources will be removed after the final verification to
avoid unnecessary ongoing charges.

## Final Security Status

The final security review confirmed:

- ALB uses a dedicated Security Group
- EC2 HTTP access is restricted to the ALB Security Group
- SSH access is restricted to the administrator's IP address
- RDS is private
- RDS access is restricted to the EC2 Security Group
- S3 remains private
- Root MFA is enabled
- IAM is used for normal access

## Cleanup Plan

After final verification, the following resources should be removed
if they are no longer required:

1. Aurora MySQL cluster and instances
2. Application Load Balancer
3. Target Group
4. EC2 instances
5. NAT Gateway
6. Unused Elastic IP addresses
7. CloudWatch alarms
8. S3 bucket and objects, if no longer required
9. Remaining VPC resources after dependent resources are removed

Resources should be deleted in dependency order to avoid deletion
errors and unnecessary charges.

## Result

The final security review confirmed that the deployed infrastructure
was reviewed for security exposure and unnecessary access.

The main security controls were verified across:

- IAM
- VPC networking
- Security Groups
- EC2
- Aurora MySQL
- S3
- Application Load Balancer
- CloudWatch
- Database backups

The infrastructure is ready for final cleanup after completing
the remaining verification steps.

## Lessons Learned

Security should be reviewed after infrastructure deployment rather
than assuming that the initial configuration is secure.

A secure architecture requires separation between:

- Internet-facing components
- Application servers
- Database services

Security Groups should reflect these layers and allow only the
traffic required between them.

Cost control is also part of infrastructure management. Resources
such as database instances, NAT Gateways, load balancers, and EC2
instances should be removed when they are no longer required.

The troubleshooting and security review process followed:

**Problem → Evidence → Hypothesis → Test → Fix → Verify**
