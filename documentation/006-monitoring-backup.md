# Task 6 — Monitoring and Backup

## Objective

Monitor the EC2 web server using Amazon CloudWatch and verify
database backup configuration for the Aurora MySQL database.

## CloudWatch Monitoring

A CloudWatch alarm was created to monitor CPU utilization of the
primary EC2 web server.

EC2 instance:

`secure-web-server`

Metric:

`CPUUtilization`

Configuration:

- Statistic: Average
- Period: 5 minutes
- Threshold: Greater than 70%
- Evaluation: 1 datapoint within 5 minutes

Alarm name:

`secure-web-server-high-cpu`

The alarm is configured to send notifications through the
CloudWatch alarms notification topic when the alarm enters the
`In alarm` state.

<img width="1902" height="758" alt="image" src="https://github.com/user-attachments/assets/2a298737-60e5-4cdf-8310-a420c13f107b" />

## Backup Configuration

Automated backups were verified for the Aurora MySQL database.

Database:

`aws-secure-web-db`

Configuration:

- Database engine: Aurora MySQL
- Backup retention period: 1 day
- Backup window: 06:49–07:19
- Encryption: Enabled

The automated backup system provides a restorable time range for
the database.

## Backup Verification

The automated backups section was inspected and a completed
system snapshot was available.

Snapshot status:

`available`

Progress:

`Completed`

This verified that the database backup mechanism was functioning.

<img width="1894" height="754" alt="image" src="https://github.com/user-attachments/assets/d426c7d4-b2d9-4385-be14-c657b9536109" />

## Result

CloudWatch monitoring and database backup configuration were
successfully implemented and verified.

The EC2 server is monitored for high CPU utilization, while the
Aurora MySQL database has automated backup and restore capability.



Monitoring allows infrastructure problems to be detected before
**Problem → Evidence → Hypothesis → Test → Fix → Verify**
