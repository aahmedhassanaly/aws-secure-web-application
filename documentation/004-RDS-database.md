# Task 4 — RDS Database

## Objective

Deploy a private managed database and securely connect it to the EC2
application server.

## Database Configuration

- Database engine: Aurora MySQL
- Deployment: 2 instances
- Region: Canada Central (`ca-central-1`)
- VPC: `secure-web-vpc`
- Public access: Disabled
- Port: `3306`
- <img width="1919" height="813" alt="image" src="https://github.com/user-attachments/assets/cebc7e4d-c742-4609-b6ec-1b1efdec2d4a" />


## Network Configuration

A DB subnet group was created using private subnets in multiple
Availability Zones:

- `private-subnet-1` — `ca-central-1a`
- `private-subnet-2` — `ca-central-1b`

This keeps the database private and provides subnet coverage across
multiple Availability Zones.
<img width="1900" height="834" alt="image" src="https://github.com/user-attachments/assets/4afd1579-a4a6-44b0-b872-cae1bc18de45" />


## Security Group

A dedicated Security Group was created:

`rds-secure-web-sg`

Inbound MySQL/Aurora traffic is allowed on TCP port `3306` only
from the EC2 Security Group:

`web-server-sg`

The database is not exposed directly to the internet.

## Connectivity Testing

Connectivity was first tested from the EC2 instance to the database
endpoint on TCP port `3306`.

The connection was successful.

The MariaDB/MySQL client was then installed on the EC2 instance and
used to authenticate to the database successfully.

This verified:

- DNS resolution
- Network connectivity
- Security Group configuration
- TCP port 3306 access
- Database authentication

## Result

The EC2 instance can securely communicate with the private
Aurora MySQL database without exposing the database to the public
internet.
<img width="1919" height="856" alt="image" src="https://github.com/user-attachments/assets/61e4aac6-579e-458d-9a55-4b4307805f66" />


## Note

Aurora MySQL was used for this lab instead of standard RDS MySQL.
The Aurora cluster was configured with two instances.
