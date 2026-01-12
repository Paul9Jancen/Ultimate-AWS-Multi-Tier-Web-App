# AWS Multi-Tier Web Application – Ultimate Project

[![Status](https://img.shields.io/badge/status-Operational-brightgreen)](https://github.com/) 
[![AWS](https://img.shields.io/badge/AWS-Cloud-orange)](https://aws.amazon.com/)
[![Apache](https://img.shields.io/badge/Web_Server-Apache-blue)](https://httpd.apache.org/)
[![PHP](https://img.shields.io/badge/PHP-8.4.14-purple)](https://www.php.net/)

A **production-ready multi-tier web application** deployed on AWS, demonstrating **high availability, security, and operational best practices**.  

**Live Demo:** [http://Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com](http://Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com)

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Architecture](#architecture)
3. [Application Stack](#application-stack)
4. [Operational Access](#operational-access)
5. [Troubleshooting & Fixes](#troubleshooting--fixes)
6. [Project Highlights](#project-highlights)
7. [Architecture Diagram](#architecture-diagram)
8. [Deployment Verification](#deployment-verification)

---

## Project Overview
<details>
<summary>Click to expand</summary>

This project demonstrates the deployment of a **secure, highly available, and scalable multi-tier web application** on AWS.

**Key Features:**
- Multi-AZ deployment for high availability
- Private EC2 instances with no public IPs
- Application Load Balancer (ALB) for scalability
- NAT Gateway for outbound internet access
- Operational access via AWS Systems Manager (SSM)
- Health checks ensuring reliable service delivery

</details>

---

## Architecture
<details>
<summary>Click to expand</summary>

### VPC and Subnets
- **VPC Name:** UltimateVPC
- **CIDR:** 10.0.0.0/16
- **Availability Zones:** 2 (us-east-1a, us-east-1b)

**Subnets:**

| Subnet Type | Name | CIDR | AZ |
|------------|------|------|----|
| Public | Public-Subnet-1 | 10.0.1.0/24 | us-east-1a |
| Public | Public-Subnet-2 | 10.0.2.0/24 | us-east-1b |
| Private | Private-Subnet-1 | 10.0.3.0/24 | us-east-1a |
| Private | Private-Subnet-2 | 10.0.4.0/24 | us-east-1b |

### Security
- **WebServer-1 (Private EC2)**
  - Instance Type: t3.micro
  - Private IP: 10.0.3.46
  - No Public IP (isolated)
  - Security Groups allow traffic **only from ALB**
  - SSM access enabled (no SSH keys required)

### Connectivity
- **NAT Gateway:** Ultimate-NAT (Public Subnet-1)
- **Route Tables:**
  - Public RT: 0.0.0.0/0 → Internet Gateway
  - Private RT: 0.0.0.0/0 → NAT Gateway

### Load Balancing
- **Application Load Balancer (Ultimate-ALB)**
  - Scheme: Internet-facing
  - Protocol: HTTP (Port 80)
  - DNS: Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com
  - Target Group: WebServer-TG (WebServer-1)

**Health Check:**
- Protocol: HTTP, Path: /
- Interval: 30s, Healthy threshold: 5, Unhealthy threshold: 2
- Status: 1/1 healthy

</details>

---

## Application Stack
<details>
<summary>Click to expand</summary>

- Web Server: Apache HTTP Server (httpd)
- PHP Version: 8.4.14
- OS: Amazon Linux 2023
- Architecture: x86_64
- Server API: FPM/FastCGI

**Performance Metrics:**
- Requests Served: 18,655+
- Response Time: 0.072 requests/sec
- Bytes Served: 5.2 KB/sec

**Live Website:** [Click Here](http://Ultimate-ALB-852146662.us-east-1.elb.amazonaws.com)

</details>

---

## Operational Access
<details>
<summary>Click to expand</summary>

**AWS Systems Manager (SSM):**
- IAM Role: EC2-SSM-Role with AmazonSSMManagedInstanceCore
- Browser-based shell access without SSH
- Full audit logging in CloudWatch
- Connection: EC2 Console → Session Manager → Connect

</details>

---

## Troubleshooting & Fixes
<details>
<summary>Click to expand</summary>

**Issue:** Target group showed unhealthy (502 Bad Gateway)  
**Root Cause:** Apache was not installed (user data script failed)  
**Solution:**  
1. Created IAM role with SSM access  
2. Connected via Session Manager  
3. Installed Apache and PHP: `sudo dnf install -y httpd php`  
4. Started service: `sudo systemctl start httpd`  
5. Enabled auto-start: `sudo systemctl enable httpd`  
6. Created PHP info page: `echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/index.php`  

**Result:** Target became healthy and application is live.

</details>

---

## Project Highlights
<details>
<summary>Click to expand</summary>

- High Availability: Multi-AZ deployment  
- Security: Private instances, no public IPs  
- Scalability: ALB ready for multiple targets  
- Reliability: Health checks for traffic routing  
- Operational Excellence: SSM Session Manager  
- Cost Optimization: NAT Gateway in single AZ, t3.micro instances

</details>

---

## Architecture Diagram
<details>
<summary>Click to expand</summary>

Internet → IGW → Ultimate-ALB (Public Subnets)
↓
Target Group (WebServer-TG)
↓
WebServer-1 (Private Subnet)
↓
NAT Gateway ← Private Route Table
↓
Internet (outbound only)

markdown
Copy code

</details>

---

## Deployment Verification
<details>
<summary>Click to expand</summary>

- VPC and subnets deployed across 2 AZs  
- Internet Gateway attached  
- NAT Gateway operational  
- Route tables and security groups configured correctly  
- EC2 instance running in private subnet  
- Apache HTTP Server installed and running  
- ALB distributing traffic to WebServer-TG  
- Health checks passing  
- Live application accessible  
- Session Manager access confirmed  

**Project Status:** Fully Operational

</details>
