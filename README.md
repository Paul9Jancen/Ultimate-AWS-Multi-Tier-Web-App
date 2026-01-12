AWS Multi-Tier Web Application - Ultimate Project

A production-ready, highly available multi-tier web application deployed on AWS, demonstrating secure architecture, load balancing, and operational best practices.

Architecture (VPC)

UltimateVPC - Custom VPC with multi-AZ deployment

VPC CIDR: 10.0.0.0/16

Region: us-east-1 (N. Virginia)

Availability Zones: 2 (us-east-1a, us-east-1b)

Subnet Architecture:
Public Subnets (Internet-facing):

Public-Subnet-1: 10.0.1.0/24 (us-east-1a)

Public-Subnet-2: 10.0.2.0/24 (us-east-1b)

Private Subnets (Application tier):

Private-Subnet-1: 10.0.3.0/24 (us-east-1a)

Private-Subnet-2: 10.0.4.0/24 (us-east-1b)

Security (Private EC2 + Security Groups)

Private Instance Architecture
WebServer-1:

Instance ID: i-013f08763dc6bd478

Instance Type: t3.micro

Private IP: 10.0.3.46

No Public IP - Isolated in private subnet

Placement: Private-Subnet-1 (us-east-1a)

Security Groups:

Instances in private subnets with no direct internet access

Security groups configured for traffic from ALB only

Inbound traffic restricted to port 80 from ALB security group

SSM access enabled via IAM role (no SSH keys required)

Connectivity (NAT Gateway + Route Tables)

NAT Gateway:

Ultimate-NAT

NAT Gateway ID: nat-034dcdbedd517e456

Type: Public

Elastic IP: 52.1.226 (allocated)

Placement: Public-Subnet-1

Purpose: Enables private instances to access internet for updates

Route Tables:

Public-RT (rtb-051a3ff998da6a3e9): Associated with Public-Subnet-1, Public-Subnet-2

Routes: 10.0.0.0/16 → local, 0.0.0.0/0 → Internet Gateway

Private-RT (rtb-03ef8b89416c65332): Associated with Private-Subnet-1, Private-Subnet-2

Routes: 10.0.0.0/16 → local, 0.0.0.0/0 → nat-034dcdbedd517e456 (NAT Gateway)

Entry Point (Application Load Balancer)

Ultimate-ALB:

Type: Application Load Balancer

Scheme: internet-facing

DNS Name: Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com

Load Balancer ARN: arn:aws:elasticloadbalancing:us-east-1:657429748140:loadbalancer/app/Ultimate-ALB/b493e5c78ef2798d

Status: Active

Availability Zones: us-east-1a, us-east-1b

Protocol: HTTP, Port: 80

VPC: vpc-08fbb8b6ed3d6c5df (UltimateVPC)

Health Check (Target Group = Healthy)

WebServer-TG:

Target Group ARN: arn:aws:elasticloadbalancing:us-east-1:657429748140:targetgroup/WebServer-TG/851758a0c3cddf65

Target Type: Instance

Protocol: HTTP:80, Protocol Version: HTTP1

Health Check Configuration:

Path: /

Port: Traffic port (80)

Protocol: HTTP

Healthy threshold: 5 consecutive successes

Unhealthy threshold: 2 consecutive failures

Timeout: 5 seconds

Interval: 30 seconds

Success codes: 200

Target Status:

Total targets: 1

Healthy: 1

Unhealthy: 0

Registered Target: i-013f08763dc6bd478 (WebServer-1)

Health Status: Available and passing all health checks

Live Application (Browser Access)

Live Website URL:
http://Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com

Application Stack:

Web Server: Apache HTTP Server (httpd)

PHP Version: 8.4.14

OS: Amazon Linux 2023

Architecture: x86_64

Server API: FPM/FastCGI

Application Status:

Website accessible via ALB DNS name

PHP info page loads successfully

Application serving 18,655+ requests

Response time: 0.072 requests/sec

Bytes served: 5.2KB/sec

Operational Access (AWS Systems Manager - SSM)

IAM Role Configuration:

EC2-SSM-Role, Policy: AmazonSSMManagedInstanceCore

Purpose: Enables Session Manager access without SSH

Attached to: WebServer-1

Session Manager Benefits:

Browser-based shell access

No SSH keys required

No bastion host needed

No public IP required

Full audit trail in CloudWatch Logs

Centralized access management via IAM

Connection Method:

EC2 Console → WebServer-1 → Connect → Session Manager tab → Connect

Browser-based terminal opens instantly

Fix Proof (Apache httpd Running)

Service Status Verification:

sudo systemctl status httpd


Output Confirmation:

httpd.service - The Apache HTTP Server

Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)

Active: active (running) since Fri 2026-01-09 07:28:06 UTC; 2 days ago

Main PID: 21915 (httpd)

Status: "Total requests: 18655; Idle/Busy workers 100/0; Requests/sec: 0.072; Bytes served/sec: 5.2KB/sec"

Key Evidence:

Status: active (running)

Enabled: Starts automatically on boot

Port: Listening on port 80

Uptime: Running for 2+ days

Requests Served: 18,655 total requests

Workers: 100 idle workers ready to serve traffic

Configuration: Server configured, listening on port 80

Troubleshooting Steps Taken:

Problem: 502 Bad Gateway - Target group showed unhealthy

Root Cause: Apache was never installed (user data script failed)

Solution:

Created IAM role with SSM access

Connected via Session Manager

Installed Apache and PHP: sudo dnf install -y httpd php

Started service: sudo systemctl start httpd

Enabled auto-start: sudo systemctl enable httpd

Created PHP info page: echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/index.php

Result: Target became healthy, application is now live

Project Highlights

High Availability: Multi-AZ deployment across 2 availability zones

Security: Private instances with no public IPs

Scalability: Load balancer ready for multiple targets

Reliability: Health checks ensure only healthy targets receive traffic

Operational Excellence: SSM Session Manager for secure access

Cost Optimization: NAT Gateway in single AZ, t3.micro instances

Architecture Diagram Summary

Internet → IGW → Ultimate-ALB (Public Subnets)
  ↓
Target Group (WebServer-TG)
  ↓
WebServer-1 (Private Subnet)
  ↓
NAT Gateway ← Private Route Table
  ↓
Internet (for outbound only)

Deployment Verified

All components tested and verified operational:

VPC and subnets created across 2 AZs

Internet Gateway attached

NAT Gateway operational in public subnet

Route tables configured correctly

Security groups properly configured

EC2 instance running in private subnet

IAM role attached for SSM access

Apache HTTP Server installed and running

Application Load Balancer distributing traffic

Target Group health checks passing

Live application accessible via browser

Session Manager access confirmed

Project Status: FULLY OPERATIONAL
