AWS Multi-Tier Web Application – Ultimate Project

Project Type: Production-Ready Multi-Tier Web Application
AWS Region: us-east-1 (N. Virginia)
Status: Fully Operational

1. Project Overview

This project demonstrates the deployment of a highly available, secure, and scalable multi-tier web application on AWS. It showcases best practices in cloud architecture, load balancing, and operational excellence.

Key Features:

Multi-AZ deployment for high availability

Private EC2 instances with no public IPs for security

Application Load Balancer (ALB) for scalability

NAT Gateway for outbound internet access from private subnets

Operational access via AWS Systems Manager (SSM)

Health checks ensuring reliable service delivery

2. Architecture
VPC and Subnets

VPC Name: UltimateVPC

CIDR: 10.0.0.0/16

Availability Zones: 2 (us-east-1a, us-east-1b)

Subnets:

Subnet Type	Name	CIDR	AZ
Public	Public-Subnet-1	10.0.1.0/24	us-east-1a
Public	Public-Subnet-2	10.0.2.0/24	us-east-1b
Private	Private-Subnet-1	10.0.3.0/24	us-east-1a
Private	Private-Subnet-2	10.0.4.0/24	us-east-1b
Security

WebServer-1 (Private EC2)

Instance Type: t3.micro

Private IP: 10.0.3.46

No Public IP (isolated in private subnet)

Security Groups allow traffic only from ALB

SSM access enabled (no SSH keys required)

Connectivity

NAT Gateway: Ultimate-NAT (Public Subnet-1)

Route Tables:

Public RT: 0.0.0.0/0 → Internet Gateway

Private RT: 0.0.0.0/0 → NAT Gateway

Load Balancing

Application Load Balancer (Ultimate-ALB)

Scheme: Internet-facing

Protocol: HTTP (Port 80)

DNS: Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com

Target Group: WebServer-TG (WebServer-1)

Health Check Configuration:

Protocol: HTTP

Path: /

Interval: 30 seconds

Healthy threshold: 5

Unhealthy threshold: 2

Target Status:

Healthy: 1 / 1

Unhealthy: 0

3. Application Stack

Web Server: Apache HTTP Server (httpd)

PHP Version: 8.4.14

OS: Amazon Linux 2023

Architecture: x86_64

Server API: FPM/FastCGI

Performance Metrics:

Requests Served: 18,655+

Response Time: 0.072 requests/sec

Bytes Served: 5.2 KB/sec

Website URL:
http://Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com

4. Operational Access

AWS Systems Manager (SSM):

IAM Role: EC2-SSM-Role with AmazonSSMManagedInstanceCore

Enables browser-based shell access without SSH or bastion host

Full audit logging in CloudWatch

Connection via EC2 Console → Session Manager

5. Troubleshooting & Fixes

Issue: Target group showed unhealthy (502 Bad Gateway)
Root Cause: Apache was not installed (user data script failed)
Resolution:

Created IAM role with SSM access

Connected via Session Manager

Installed Apache and PHP: sudo dnf install -y httpd php

Started and enabled Apache service

Created PHP info page to verify deployment

Result: Target became healthy, application now live.

6. Project Highlights

High Availability: Multi-AZ deployment across 2 availability zones

Security: Private EC2 instances with no public IPs

Scalability: Load balancer ready for multiple targets

Reliability: Health checks ensure only healthy targets receive traffic

Operational Excellence: SSM Session Manager for secure access

Cost Optimization: NAT Gateway in single AZ, t3.micro instances

7. Architecture Diagram
Internet → IGW → Ultimate-ALB (Public Subnets) 
           ↓
      Target Group (WebServer-TG)
           ↓
      WebServer-1 (Private Subnet)
           ↓
      NAT Gateway ← Private Route Table
           ↓
      Internet (outbound only)

8. Deployment Verification

VPC and subnets deployed across 2 AZs

Internet Gateway attached

NAT Gateway operational

Route tables and security groups configured correctly

EC2 instance running in private subnet

Apache HTTP Server installed and running

ALB distributing traffic to WebServer-TG

Health checks passing

Live application accessible

Session Manager access confirmed

Project Status: Fully Operational
