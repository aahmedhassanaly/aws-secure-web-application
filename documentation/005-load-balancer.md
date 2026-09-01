# Task 5 — Application Load Balancer

## Objective

Deploy an Application Load Balancer (ALB) to distribute HTTP traffic across multiple EC2 web servers.

## Architecture

The application architecture was extended to use an internet-facing Application Load Balancer.

The ALB receives HTTP requests from the internet and forwards them to multiple EC2 web servers.

The EC2 web servers communicate with the private Aurora MySQL database.

## EC2 Web Servers

Two EC2 instances were deployed as web servers:

- `secure-web-server`
- `web-server-2`
- <img width="1910" height="759" alt="image" src="https://github.com/user-attachments/assets/87126686-f4a3-4256-bfa1-4b1cd0dc2280" />


Both instances run Apache HTTP Server and listen on TCP port `80`.

Each server was tested locally to verify that the web server was running correctly.

## Application Load Balancer Configuration

The following Application Load Balancer was created:

- Name: `secure-web-alb`
- Type: Application Load Balancer
- Scheme: Internet-facing
- Listener: HTTP on port `80`
- Region: Canada Central (`ca-central-1`)
- <img width="1919" height="781" alt="image" src="https://github.com/user-attachments/assets/1f99b6cb-1694-4784-b3b6-4d5ae4daaa16" />

- 

The ALB uses public subnets in two Availability Zones:

- `ca-central-1a`
- `ca-central-1b`

Using multiple Availability Zones provides better availability for the load-balanced application.
<img width="1901" height="830" alt="image" src="https://github.com/user-attachments/assets/e05b7174-04f0-4e19-b88c-002efa7ddec7" />


## Security Groups

A dedicated Security Group was created for the Application Load Balancer:

`alb-sg`

Inbound traffic:

- HTTP
- TCP port `80`
- Source: `0.0.0.0/0`

Outbound traffic:

- All traffic allowed

The EC2 instances use:

`web-server-sg`

HTTP traffic from the ALB Security Group is allowed on TCP port `80`.

This separates the security controls for the load balancer from the application servers.

## Target Group

A target group named:

`fg`

was created for the EC2 web servers.

Configuration:

- Target type: Instances
- Protocol: HTTP
- Port: `80`
- Health check protocol: HTTP
- Health check path: `/`
- Health check port: Traffic port

The following EC2 instances were registered:

- `secure-web-server`
- `web-server-2`

The targets became healthy after correcting the Security Group configuration.
<img width="1917" height="715" alt="image" src="https://github.com/user-attachments/assets/2088d10f-8213-45e3-a8fa-9ae769b0b581" />


## Listener Configuration

The ALB listener is configured to listen for HTTP traffic on port `80`.

The default listener action forwards incoming requests to the target group:

`fg`

The ALB then distributes requests between the registered healthy EC2 instances.

## Troubleshooting

During testing, the ALB initially failed to respond to HTTP requests and the connection timed out.

The following components were investigated:

- DNS resolution
- Public subnet configuration
- Route table
- Internet Gateway
- Network ACL
- ALB listener
- Target group
- EC2 web servers
- Security Groups

The EC2 web servers were confirmed to be running Apache and listening on port `80`.

The target group health checks were also investigated.

### Root Cause

The ALB was initially associated with the EC2 Security Group:

`web-server-sg`

instead of the dedicated ALB Security Group:

`alb-sg`

As a result, the ALB did not have the correct inbound HTTP access configuration.

### Fix

The dedicated `alb-sg` Security Group was attached to the Application Load Balancer.

The Security Group allows inbound HTTP traffic on TCP port `80` from the internet.

After applying the change, the ALB became reachable and HTTP requests were successfully forwarded to the EC2 web servers.

## Verification

The ALB was tested using its DNS name.

The HTTP request successfully reached the application through the Application Load Balancer.

The target group showed both EC2 instances as healthy.

This verified:

- ALB connectivity
- HTTP listener configuration
- Security Group configuration
- Target group configuration
- EC2 web server availability
- Health checks

## Result

The application now uses an internet-facing Application Load Balancer to distribute HTTP traffic across multiple EC2 web servers.

The ALB performs health checks and forwards traffic to healthy targets.

The application servers remain separated from the public-facing load balancer through Security Group rules.

## Lessons Learned

This task demonstrated the importance of separating Security Groups according to network roles.

The ALB requires its own Security Group for internet-facing traffic, while the EC2 servers should only accept application traffic from the appropriate source.

The troubleshooting process also demonstrated that a timeout does not necessarily mean that the web server is down. Network routing, Security Groups, Network ACLs, listeners, and target health must be checked systematically.

The troubleshooting approach used was:

**Problem → Evidence → Hypothesis → Test → Fix → Verify**
