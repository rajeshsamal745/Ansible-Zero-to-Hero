# 🚀 Enterprise Java Order Management Deployment on AWS using Ansible

[![Ansible](https://img.shields.io/badge/Ansible-2.21-red?logo=ansible)](https://www.ansible.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20S3%20%7C%20CloudWatch-orange?logo=amazonaws)](https://aws.amazon.com/)
[![Java](https://img.shields.io/badge/Java-17-blue?logo=openjdk)](https://openjdk.org/)
[![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-green?logo=nginx)](https://nginx.org/)
[![Linux](https://img.shields.io/badge/Linux-Ubuntu-orange?logo=ubuntu)](https://ubuntu.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> Production-oriented AWS DevOps project demonstrating reusable Ansible roles,
> dynamic AWS inventory, automated Java application deployment, rolling releases,
> health checks, monitoring, security hardening, and rollback capabilities.

---

## 📌 Project Overview

This project automates the deployment and lifecycle management of a Java-based
Order Management application running on Ubuntu EC2 instances in AWS.

The infrastructure is managed using **Ansible Roles** with a focus on:

- Infrastructure automation
- Configuration management
- Application deployment
- Environment-specific configuration
- AWS dynamic inventory
- Rolling deployments
- Application health validation
- Failure handling
- Rollback
- CloudWatch monitoring
- Ansible Vault
- Idempotency
- Reusable infrastructure code

The project is designed to demonstrate how a traditional Ansible deployment
can evolve into a more production-oriented AWS deployment architecture.

---

# 🏗️ Architecture

```mermaid
flowchart TB

    Developer[Developer / DevOps Engineer]

    GitHub[GitHub Repository]

    Ansible[Ansible Control Node]

    AWS[AWS]

    EC2[EC2 Instances]

    App01[App01]
    App02[App02]

    Nginx[Nginx]
    Java[Java 17]
    App[Spring Boot Order Service]
    CW[CloudWatch Agent]

    S3[S3 Artifact Repository]

    Developer --> GitHub
    GitHub --> Ansible

    Ansible --> AWS

    AWS --> EC2

    EC2 --> App01
    EC2 --> App02

    App01 --> Nginx
    App01 --> Java
    App01 --> App
    App01 --> CW

    App02 --> Nginx
    App02 --> Java
    App02 --> App
    App02 --> CW

    S3 --> App01
    S3 --> App02


🔄 Dynamic Inventory Architecture

The project supports AWS EC2 dynamic inventory using EC2 tags.

AWS EC2
   │
   ├── Environment=dev
   ├── Application=order-service
   └── Role=app
          │
          ▼
   AWS Dynamic Inventory
          │
          ▼
     app_servers
          │
     ┌────┴────┐
     ▼         ▼
   App01      App02

This removes the need to manually maintain EC2 IP addresses in the inventory.

🧩 Ansible Role Architecture

The project is organized into reusable Ansible roles:

roles/
├── common/
├── security/
├── java/
├── nginx/
├── application/
└── monitoring/
Common

Responsible for:

System preparation
Common packages
Application user/group
Application directories
Timezone configuration
Security

Responsible for:

SSH configuration
Server hardening
Java

Responsible for:

Java 17 installation
JAVA_HOME configuration
Java validation
Nginx

Responsible for:

Nginx installation
Reverse proxy configuration
Application routing
Configuration validation
Application

Responsible for:

Application artifact download
Application configuration
Systemd service
Application startup
Application restart
Health validation
Monitoring

Responsible for:

CloudWatch Agent installation
CloudWatch configuration
Monitoring service management

    CW --> AWS
