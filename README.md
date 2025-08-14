# 3-Tier Web Application on AWS with Terraform

> **Note:** This project uses Terraform to deploy a scalable and highly available 3-tier application architecture on AWS.  

<img width="1470" height="686" alt="image-1" src="https://github.com/user-attachments/assets/20ff8ef4-795b-44b5-9725-8e47024e5abe" />

## 🧩 Solution Overview

This project implements a scalable and resilient **3-tier web application architecture** on AWS using **Terraform** Infrastructure as Code (IaC). It is designed for workloads that need separation of concerns (frontend/backend/database), scalability, high availability, and automated provisioning.

This solution automatically provisions networking, compute, storage, monitoring, and CDN resources in a modular fashion. The infrastructure follows best practices and supports the following capabilities:

- Frontend and backend tiers run on EC2 instances in Auto Scaling Groups (ASGs).
- Compute resources are split across multiple Availability Zones to improve fault tolerance.
- Amazon RDS is used to host a managed, persistent database layer.
- Amazon CloudFront is used to serve static assets and frontend content globally, reducing latency.
- AWS CloudWatch is used to monitor CPU metrics and trigger alarms.
- Security Groups provide strict access control between components.
- Scripts (UserData) allow automatic installation of web server software (e.g., NGINX, Apache, etc.).

This solution can serve as a production-ready base infrastructure for hosting any modern web application, such as an e-commerce platform, content management system (CMS), or microservice-based backend API.

---

## 📚 Table of Content

- [Solution Overview](#solution-overview)  
- [Architecture Diagram](#architecture-diagram)  
- [Terraform Modules](#terraform-modules)  
- [Component Breakdown](#component-breakdown)  
- [Customizing the Solution](#customizing-the-solution)  
- [Prerequisites for Customization](#prerequisites-for-customization)  
  1. [Clone the Repository](#1-clone-the-repository)  
  2. [Initialize and Validate](#2-initialize-and-validate)  
  3. [Build & Deploy](#3-build--deploy)  
- [Operational Metrics](#operational-metrics)  
- [External Contributors](#external-contributors)  
- [License](#license)

---
## 🖼 Architecture Diagram

The 3-Tier architecture consists of the following components:

1. **Frontend Tier**
   - Deployed in a public subnet.
   - Uses Auto Scaling Group with EC2 instances.
   - UserData installs a web server and optionally connects to the backend API.
   - Content is accelerated globally via Amazon CloudFront.

2. **Backend Tier**
   - Deployed in a private subnet.
   - Backend EC2 instances handle business logic and connect securely to the database.
   - ASG ensures dynamic scaling based on load.

3. **Database Tier**
   - Amazon RDS is provisioned in a private subnet.
   - Accessible only via backend Security Group.

4. **Networking**
   - VPC with 2 or more Availability Zones.
   - Public and private subnets separated via route tables.
   - Internet Gateway for frontend traffic, and NAT Gateway for backend EC2 outbound internet.

5. **Monitoring and Scaling**
   - CloudWatch monitors CPU, triggers alarms, and can be extended with custom metrics.
   - Auto Scaling policies respond to traffic spikes.

6. **Security**
   - Each tier has its own Security Group with least-privilege access.
   - SSH access is managed using a key pair created via Terraform.

The overall infrastructure is highly modular, making it easy to extend with services like AWS WAF, ALB, or S3 static hosting.

This Terraform-based project deploys a production-ready **3-tier architecture** designed for web applications. It separates application concerns across:
- **Frontend Layer**: Serves static and dynamic content to end users.
- **Backend Layer**: Handles business logic and API endpoints.
- **Database Layer**: Manages persistent storage using RDS.

Key Features:
- Highly Available across multiple Availability Zones
- Automatically Scalable using AWS Auto Scaling Groups
- Modular Design using Terraform modules for reusability
- Monitoring with CloudWatch and log collection
- Fast Global Content Delivery via CloudFront

---



## 🧱 Terraform Modules

The project is structured using isolated modules to separate concerns and ease maintenance.

| Module      | Purpose                                      |
|-------------|----------------------------------------------|
| `vpc`       | Defines custom VPC, public/private subnets   |
| `SecGroups` | Sets up tiered security groups               |
| `key`       | Handles SSH key pair for EC2 access          |
| `FrontEnd`  | Provisions EC2 for frontend and UserData     |
| `BackEnd`   | Deploys backend EC2 services                 |
| `ASG`       | Auto Scaling for front and backend tiers     |
| `DB`        | RDS database provisioning                    |
| `CloudWatch`| Alarm and metrics for monitoring             |
| `CloudFront`| CDN for frontend acceleration                |

---

## 🧩 Component Breakdown

### VPC & Networking
- Custom VPC with public and private subnets across 2 AZs
- Internet Gateway and NAT Gateway for routing

### Auto Scaling Groups
- Separate ASGs for frontend and backend
- Configured with Launch Templates and health checks

### EC2 & User Data
- Scripts in `UserData` directories configure the web servers
- Includes installation of web servers, app code, and dependencies

### Database Layer
- RDS MySQL/PostgreSQL deployed in private subnet
- Accessible only from backend security group

### CloudWatch & Monitoring
- Alarms for CPU usage and instance health
- Logging available for further analysis

### CloudFront CDN
- Distributes frontend content globally with low latency

---

## ⚙️ Customizing the Solution

You can easily tailor this infrastructure to suit different applications by:
- Updating variable values in `variables.tf`
- Modifying `UserData` scripts to install your stack (Node, Python, PHP, etc.)
- Adding new modules for S3 buckets, Lambda, etc.

---

## 🛠 Prerequisites for Customization

### 1. Clone the Repository

```bash
git clone https://github.com/sharara99/Deploying-Scalable-3-Tier-Architecture-on-AWS.git
cd Deploying-Scalable-3-Tier-Architecture-on-AWS
```

### 2. Initialize and Validate

```bash
terraform init
terraform validate
```

### 3. Build & Deploy

```bash
terraform plan -out=tfplan
terraform apply tfplan
```

---

## 📈 Operational Metrics

Monitored through AWS CloudWatch:
- EC2 Instance CPU, Memory, Disk
- Auto Scaling Event logs
- RDS performance insights (optional)
- CloudFront access logs (optional)

---

## 🤝 External Contributors

Contributions are welcome! If you want to enhance or fix something:
- Fork the repository
- Commit your changes
- Open a Pull Request with a description

Make sure to run:
```bash
terraform fmt
terraform validate
```

---
