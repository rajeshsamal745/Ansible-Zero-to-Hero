# 🚀 Enterprise Java Order Management Deployment on AWS Using Ansible

[![Ansible](https://img.shields.io/badge/Ansible-2.21-red?logo=ansible)](https://www.ansible.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20S3%20%7C%20CloudWatch-orange?logo=amazonaws)](https://aws.amazon.com/)
[![Java](https://img.shields.io/badge/Java-17-blue?logo=openjdk)](https://openjdk.org/)
[![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-green?logo=nginx)](https://nginx.org/)
[![Linux](https://img.shields.io/badge/Linux-Ubuntu-orange?logo=ubuntu)](https://ubuntu.com/)
[![Shell](https://img.shields.io/badge/Shell-Bash-lightgrey?logo=gnu-bash)](https://www.gnu.org/software/bash/)
[![Git](https://img.shields.io/badge/Git-GitHub-black?logo=git)](https://git-scm.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> Production-oriented AWS DevOps project demonstrating reusable Ansible
> roles, AWS EC2 dynamic inventory, automated Java application deployment,
> versioned artifacts, rolling releases, application health checks,
> monitoring, failure recovery, and version-based rollback.

---

# 📌 Table of Contents

- [🎯 Project Objective](#-project-objective)
- [⭐ Recruiter Snapshot](#-recruiter-snapshot)
- [🏗️ Architecture](#️-architecture)
- [🔄 Dynamic Inventory Architecture](#-dynamic-inventory-architecture)
- [🧩 Ansible Role Architecture](#-ansible-role-architecture)
- [☁️ AWS Services Used](#️-aws-services-used)
- [📦 Application Artifact Strategy](#-application-artifact-strategy)
- [🔄 Deployment Flow](#-deployment-flow)
- [🚦 Rolling Deployment](#-rolling-deployment)
- [❤️ Application Health Check](#️-application-health-check)
- [🔙 Rollback Strategy](#-rollback-strategy)
- [🧪 Failure Simulation](#-failure-simulation)
- [🛠️ Troubleshooting](#️-troubleshooting)
- [🔐 Security](#-security)
- [🔒 Ansible Vault](#-ansible-vault)
- [🌎 Environment Strategy](#-environment-strategy)
- [📁 Project Structure](#-project-structure)
- [🧪 Validation and Testing](#-validation-and-testing)
- [🔁 Idempotency](#-idempotency)
- [🔔 Ansible Handlers](#-ansible-handlers)
- [🏷️ Ansible Tags](#️-ansible-tags)
- [🔗 Role Dependencies](#-role-dependencies)
- [♻️ Role Reusability](#️-role-reusability)
- [📊 Monitoring](#-monitoring)
- [🧠 DevOps Concepts Demonstrated](#-devops-concepts-demonstrated)
- [💼 What I Built](#-what-i-built)
- [💻 Technology Stack](#-technology-stack)
- [📸 Screenshots](#-screenshots)
- [🚀 Quick Start](#-quick-start)
- [🔮 Future Enhancements](#-future-enhancements)
- [⚠️ Security Notice](#️-security-notice)
- [📝 Recommended .gitignore](#-recommended-gitignore)
- [📜 License](#-license)
- [👨‍💻 Author](#-author)

---

# 🎯 Project Objective

The objective of this project is to automate the deployment and lifecycle
management of a Java-based Order Management application running on Ubuntu
EC2 instances in AWS.

The project demonstrates how Ansible can be used as a reusable automation
framework for:

- Server provisioning
- Configuration management
- Security hardening
- Java installation
- Nginx configuration
- Application deployment
- Versioned artifact management
- AWS dynamic inventory
- Rolling deployments
- Application health validation
- Failure handling
- Recovery
- Rollback
- Monitoring
- Secret management
- Environment-specific configuration

The project follows a production-oriented DevOps approach rather than
treating Ansible as only a server installation tool.

---

# ⭐ Recruiter Snapshot

| Category | Details |
|---|---|
| **Role** | AWS DevOps Engineer |
| **Cloud Platform** | AWS |
| **Automation** | Ansible |
| **Application** | Java / Spring Boot |
| **Operating System** | Ubuntu Linux |
| **Web Server** | Nginx |
| **Artifact Repository** | Amazon S3 |
| **Monitoring** | Amazon CloudWatch |
| **Inventory** | AWS EC2 Dynamic Inventory |
| **Deployment Strategy** | Rolling Deployment |
| **Recovery** | Version-Based Rollback |
| **Configuration Management** | Reusable Ansible Roles |
| **Secrets Management** | Ansible Vault |
| **Security** | IAM + SSH Hardening |
| **Validation** | Health Checks + Check Mode |
| **Quality** | Ansible Lint |
| **Version Control** | Git / GitHub |

### Key Project Highlights

```text
AWS EC2
   ↓
Dynamic Inventory
   ↓
Reusable Ansible Roles
   ↓
Java 17
   ↓
Nginx
   ↓
S3 Versioned Artifact
   ↓
Rolling Deployment
   ↓
Health Check
   ↓
CloudWatch Monitoring
   ↓
Failure Recovery

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

    CW --> AWS
   ↓
Rollback
