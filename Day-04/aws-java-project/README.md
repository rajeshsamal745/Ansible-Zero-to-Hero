# 🚀 Enterprise Java Order Management: AWS & Ansible Automation

[![Ansible](https://img.shields.io/badge/Ansible-2.15%2B-EE0000?logo=ansible&logoColor=white)](https://www.ansible.com/)
[![AWS](https://img.shields.io/badge/AWS-EC2%20%7C%20S3%20%7C%20IAM%20%7C%20CloudWatch-FF9900?logo=amazonaws&logoColor=white)](https://aws.amazon.com/)
[![Java](https://img.shields.io/badge/Java-OpenJDK%2017-ED8B00?logo=openjdk&logoColor=white)](https://openjdk.org/)
[![Nginx](https://img.shields.io/badge/Nginx-Reverse%20Proxy-009639?logo=nginx&logoColor=white)](https://www.nginx.com/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04%20LTS-E95420?logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

> **Production-oriented DevOps project** demonstrating reusable Ansible roles, automated Java application deployment, zero-downtime rolling releases, infrastructure drift correction, and secure secret management on AWS.

---

## 📖 Executive Summary
This repository contains the end-to-end automation framework for deploying and managing a Java-based Order Management application on AWS. Instead of simple scripting, this project utilizes **modular Ansible Roles** to enforce infrastructure as code (IaC) principles, ensuring idempotency, security hardening, and high availability. 

It bridges the gap between basic configuration management and enterprise SRE practices by implementing **rolling deployments, automated health-check gatekeeping, AWS IAM least-privilege access, and Ansible Vault for secrets.**

---

## 🏗️ Architecture & Deployment Flow

### Infrastructure Topology
<img width="420" height="308" alt="image" src="https://github.com/user-attachments/assets/4e78f726-a25a-4b87-9bc2-5d6b53f8a827" />
