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
Nginx Reverse Proxy
   ↓
S3 Versioned Artifact
   ↓
Rolling Deployment
   ↓
Application Health Check
   ↓
CloudWatch Monitoring
   ↓
Failure Recovery
   ↓
Version-Based Rollback
```

---

# 🏗️ Architecture

The solution consists of two Ubuntu EC2 application servers running the
Java Order Management application.

Nginx acts as the reverse proxy and forwards external HTTP traffic to the
Java application running internally on port `8080`.

```text
                         ┌──────────────────────┐
                         │      Developer       │
                         │     / DevOps Team    │
                         └──────────┬───────────┘
                                    │
                                    │ Ansible
                                    ▼
                    ┌──────────────────────────────┐
                    │       Ansible Control Node    │
                    │                              │
                    │  Roles                       │
                    │  ├── common                  │
                    │  ├── security                │
                    │  ├── java                    │
                    │  ├── nginx                   │
                    │  ├── application             │
                    │  └── monitoring              │
                    └──────────────┬───────────────┘
                                   │
                     AWS EC2 Dynamic Inventory
                                   │
                 ┌─────────────────┴─────────────────┐
                 │                                   │
                 ▼                                   ▼
        ┌─────────────────┐                 ┌─────────────────┐
        │     App 01      │                 │     App 02      │
        │  Ubuntu EC2     │                 │  Ubuntu EC2     │
        │                 │                 │                 │
        │ Nginx :80       │                 │ Nginx :80       │
        │      │          │                 │      │          │
        │      ▼          │                 │      ▼          │
        │ Java App :8080  │                 │ Java App :8080  │
        │                 │                 │                 │
        │ Systemd         │                 │ Systemd         │
        └────────┬────────┘                 └────────┬────────┘
                 │                                   │
                 └─────────────────┬─────────────────┘
                                   │
                                   ▼
                         ┌──────────────────┐
                         │   Amazon S3      │
                         │                  │
                         │ Versioned JAR    │
                         └──────────────────┘

                                   │
                                   ▼

                         ┌──────────────────┐
                         │ Amazon CloudWatch│
                         │                  │
                         │ CPU / Memory     │
                         │ Disk / Logs      │
                         └──────────────────┘
```

### Request Flow

```text
Client
  │
  │ HTTP :80
  ▼
Nginx
  │
  │ Reverse Proxy
  ▼
Java Spring Boot Application
  │
  │ :8080
  ▼
Order Management Service
```

---

# 🔄 Dynamic Inventory Architecture

Instead of manually maintaining server IP addresses, this project uses
AWS EC2 dynamic inventory.

EC2 instances are identified using AWS tags.

### EC2 Tags

```text
Environment = dev
Application = order-service
Role        = app
```

These tags are used by the Ansible AWS EC2 inventory plugin to automatically
discover the correct application servers.

### Dynamic Inventory Flow

```text
AWS EC2 Instances
       │
       │ EC2 Tags
       ▼
┌───────────────────────────┐
│ Environment = dev         │
│ Application = order-service│
│ Role = app                │
└─────────────┬─────────────┘
              │
              ▼
    AWS EC2 Dynamic Inventory
              │
              ▼
        app_servers
              │
        ┌─────┴─────┐
        ▼           ▼
      app01       app02
```

### Dynamic Inventory Configuration

Example:

```yaml
---
plugin: amazon.aws.aws_ec2

regions:
  - ap-south-1

filters:
  instance-state-name: running

groups:
  app_servers: >
    tags.Environment == 'dev'
    and tags.Application == 'order-service'
    and tags.Role == 'app'

keyed_groups:
  - key: tags.Environment
    prefix: environment
    separator: "_"

  - key: tags.Application
    prefix: application
    separator: "_"

  - key: tags.Role
    prefix: role
    separator: "_"

hostnames:
  - ip-address

compose:
  ansible_host: public_ip_address
```

### Verify Dynamic Inventory

```bash
ansible-inventory \
  -i inventory/aws/dev.aws_ec2.yml \
  --graph
```

List discovered hosts:

```bash
ansible-inventory \
  -i inventory/aws/dev.aws_ec2.yml \
  --list
```

Test connectivity:

```bash
ansible \
  -i inventory/aws/dev.aws_ec2.yml \
  app_servers \
  -m ping
```

### Why Dynamic Inventory?

Dynamic inventory removes the dependency on manually maintained IP addresses.

When EC2 instances are replaced or new instances are added with the
correct tags, Ansible can automatically discover them.

---

# 🧩 Ansible Role Architecture

The project follows an Ansible role-based architecture.

Each role is responsible for a specific infrastructure or application
concern.

```text
roles/
│
├── common/
│   └── Base OS configuration
│
├── security/
│   └── Server hardening
│
├── java/
│   └── Java 17 installation
│
├── nginx/
│   └── Nginx reverse proxy
│
├── application/
│   └── Java application deployment
│
└── monitoring/
    └── CloudWatch Agent
```

### Role Responsibilities

| Role | Responsibility |
|---|---|
| `common` | Base packages, user/group, directories, timezone |
| `security` | Security configuration and hardening |
| `java` | Install and configure Java 17 |
| `nginx` | Install and configure Nginx |
| `application` | Download, configure and deploy application |
| `monitoring` | Configure CloudWatch monitoring |

### Execution Order

```text
common
   ↓
security
   ↓
java
   ↓
nginx
   ↓
application
   ↓
monitoring
```

This separation makes the automation easier to maintain, test and reuse.

---

# ☁️ AWS Services Used

| AWS Service | Purpose |
|---|---|
| **Amazon EC2** | Hosts the Java application |
| **Amazon S3** | Stores versioned application JAR artifacts |
| **Amazon CloudWatch** | Monitoring and log collection |
| **AWS IAM** | Access control and permissions |
| **Amazon VPC** | Network infrastructure |
| **Security Groups** | Network-level access control |

### EC2

Two Ubuntu EC2 instances are used as application servers.

```text
EC2 App 01
EC2 App 02
```

### S3

S3 stores versioned application artifacts.

Example:

```text
s3://<bucket-name>/
└── order-service/
    ├── 1.0/
    │   └── order-service.jar
    ├── 1.1/
    │   └── order-service.jar
    └── 1.2/
        └── order-service.jar
```

### CloudWatch

CloudWatch is used for centralized monitoring of the application
infrastructure and logs.

---

# 📦 Application Artifact Strategy

The application artifact is stored in Amazon S3 instead of being copied
directly from the Ansible control node.

Example:

```text
S3 Bucket
   │
   └── order-service/
       │
       └── 1.0/
           │
           └── order-service.jar
```

The application version is configurable through Ansible variables.

Example:

```yaml
app_version: "1.0"
```

The artifact key follows the pattern:

```text
order-service/{{ app_version }}/order-service.jar
```

### Deployment Model

```text
Developer
    │
    ▼
Build Java Application
    │
    ▼
Create JAR
    │
    ▼
Upload to S3
    │
    ▼
Ansible
    │
    ▼
Download versioned JAR
    │
    ▼
EC2 Application Server
    │
    ▼
Systemd
    │
    ▼
Java Application
```

### Benefits

- Version-specific deployments
- Repeatable deployments
- Easy rollback
- Centralized artifact storage
- No application binary stored inside the Ansible role
- Clear separation between automation and application artifact

---

# 🔄 Deployment Flow

The complete deployment process is:

```text
1. Developer uploads application artifact to S3
                    ↓
2. Ansible discovers EC2 instances
                    ↓
3. Common configuration
                    ↓
4. Security configuration
                    ↓
5. Java 17 installation
                    ↓
6. Nginx configuration
                    ↓
7. Application artifact downloaded from S3
                    ↓
8. Application configuration generated
                    ↓
9. Systemd service configured
                    ↓
10. Application started
                    ↓
11. Health check executed
                    ↓
12. Monitoring configured
```

### Main Playbook

Example:

```yaml
---
- name: Deploy ABC Order Service
  hosts: app_servers
  become: true

  roles:
    - common
    - security
    - java
    - nginx
    - application
    - monitoring
```

---

# 🚦 Rolling Deployment

The application is deployed using a rolling deployment strategy.

The deployment uses:

```yaml
serial: 1
```

This means only one application server is updated at a time.

### Rolling Deployment Flow

```text
Initial State

App01 → Version 1.0 → Healthy
App02 → Version 1.0 → Healthy


              │
              ▼

Deploy App01

App01 → Version 1.1 → Health Check


              │
              ▼

App01 Healthy


              │
              ▼

Deploy App02

App02 → Version 1.1 → Health Check


              │
              ▼

Final State

App01 → Version 1.1 → Healthy
App02 → Version 1.1 → Healthy
```

### Deployment Configuration

```yaml
---
- name: Rolling Application Deployment
  hosts: app_servers
  become: true

  serial: 1

  roles:
    - application

  post_tasks:

    - name: Wait for application health
      ansible.builtin.uri:
        url: "http://127.0.0.1:{{ app_port }}/health"
        status_code: 200
      register: health_check

      retries: 10
      delay: 5

      until: health_check.status == 200
```

### Why `serial: 1`?

If one server is being upgraded while another server remains available,
the application can continue serving traffic through the healthy server.

This reduces deployment risk compared with updating every server
simultaneously.

---

# ❤️ Application Health Check

Application health validation is performed after deployment.

The application exposes:

```text
http://127.0.0.1:8080/health
```

Ansible validates that the endpoint returns:

```text
HTTP 200
```

### Health Check Logic

```text
Deploy Application
       │
       ▼
Start / Restart Service
       │
       ▼
Wait for Application
       │
       ▼
GET /health
       │
       ├── HTTP 200 ──► Deployment Successful
       │
       └── Failure ───► Retry
                          │
                          ▼
                       10 Attempts
```

### Retry Configuration

```yaml
retries: 10
delay: 5
```

This allows the Java application time to start before Ansible considers
the deployment unsuccessful.

### Manual Health Check

```bash
curl http://127.0.0.1:8080/health
```

Through Nginx:

```bash
curl http://127.0.0.1/health
```

---

# 🔙 Rollback Strategy

The application deployment uses version-based rollback.

For example:

```text
Current Version:
1.1

Previous Stable Version:
1.0
```

Rollback changes the application version back to:

```yaml
app_version: "1.0"
```

### Rollback Playbook

```yaml
---
- name: Rollback ABC Order Service
  hosts: app_servers
  become: true

  serial: 1

  vars:
    app_version: "1.0"

  roles:
    - application
```

### Run Rollback With Dynamic Inventory

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/rollback.yml
```

### Rollback Flow

```text
Production Version
       │
       ▼
   Version 1.1
       │
       │ Deployment Problem
       ▼
Rollback Triggered
       │
       ▼
   Version 1.0
       │
       ▼
Application Restart
       │
       ▼
Health Check
       │
       ▼
Rollback Validation
```

### Important Design Principle

The rollback playbook itself does not depend on static or dynamic inventory.

The playbook targets:

```yaml
hosts: app_servers
```

The inventory determines where `app_servers` comes from.

Dynamic inventory:

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/rollback.yml
```

Static inventory:

```bash
ansible-playbook \
  -i inventory/dev/hosts.ini \
  playbooks/rollback.yml
```

---

# 🧪 Failure Simulation

A deliberate application failure was introduced to validate the recovery
and troubleshooting process.

### Failure Scenario

The application JAR on one application server was moved temporarily.

Example:

```bash
sudo mv \
  /opt/abc-order-service/releases/1.0/order-service.jar \
  /opt/abc-order-service/releases/1.0/order-service.jar.bak
```

The application service was then stopped and restarted.

```bash
sudo systemctl stop abc-order-service
sudo systemctl start abc-order-service
```

The service entered a failure/restart loop because the JAR was unavailable.

### Expected Error

```text
Unable to access jarfile
```

### Troubleshooting

Check service status:

```bash
sudo systemctl status abc-order-service
```

View logs:

```bash
sudo journalctl -u abc-order-service -n 100 --no-pager
```

Check application directory:

```bash
ls -lah /opt/abc-order-service/
```

Check release directory:

```bash
ls -lah /opt/abc-order-service/releases/
```

### Recovery

Restore the artifact:

```bash
sudo mv \
  /opt/abc-order-service/releases/1.0/order-service.jar.bak \
  /opt/abc-order-service/releases/1.0/order-service.jar
```

Restart:

```bash
sudo systemctl restart abc-order-service
```

Validate:

```bash
sudo systemctl status abc-order-service
```

Health check:

```bash
curl http://127.0.0.1:8080/health
```

### Automated Recovery

Because the application artifact is stored in S3, the Ansible application
role can also re-download the required version during deployment.

This demonstrates failure simulation, diagnosis and recovery rather than
only testing the happy path.

---

# 🛠️ Troubleshooting

## Check EC2 Connectivity

```bash
ansible \
  -i inventory/aws/dev.aws_ec2.yml \
  app_servers \
  -m ping
```

## Check Inventory

```bash
ansible-inventory \
  -i inventory/aws/dev.aws_ec2.yml \
  --graph
```

## List Inventory Variables

```bash
ansible-inventory \
  -i inventory/aws/dev.aws_ec2.yml \
  --list
```

## Check Service Status

```bash
sudo systemctl status abc-order-service
```

## Restart Application

```bash
sudo systemctl restart abc-order-service
```

## View Application Logs

```bash
sudo journalctl \
  -u abc-order-service \
  -n 100 \
  --no-pager
```

## Follow Application Logs

```bash
sudo journalctl \
  -u abc-order-service \
  -f
```

## Check Listening Port

```bash
sudo ss -lntp | grep 8080
```

## Test Local Application

```bash
curl http://127.0.0.1:8080/health
```

## Test Nginx

```bash
curl http://127.0.0.1/health
```

## Check Nginx

```bash
sudo systemctl status nginx
```

## Validate Nginx Configuration

```bash
sudo nginx -t
```

## Check Disk

```bash
df -h
```

## Check Memory

```bash
free -h
```

## Check System Load

```bash
uptime
```

## Check Processes

```bash
ps aux | grep java
```

---

# 🔐 Security

Security is considered at multiple layers.

### EC2 Security

Security Groups control network access to the EC2 instances.

Typical application access:

```text
Internet
   │
   ▼
TCP 80
   │
   ▼
Nginx
   │
   ▼
TCP 8080
   │
   ▼
Java Application
```

The Java application is intended to run behind Nginx rather than being
directly exposed to the internet.

### SSH

Ansible connects to the Ubuntu EC2 servers using SSH.

Example:

```text
Ansible Control Node
        │
        │ SSH
        ▼
Ubuntu EC2
```

### IAM

AWS IAM controls access to AWS resources.

Dynamic inventory requires permission to discover EC2 instances.

Example permissions include:

```text
ec2:DescribeInstances
ec2:DescribeTags
```

S3 access should be limited to the required artifact bucket and paths.

### Security Principles

- Least privilege IAM
- SSH key-based authentication
- Application not directly exposed on port 8080
- Nginx as reverse proxy
- Secrets stored using Ansible Vault
- No credentials committed to Git
- Environment-specific configuration
- Controlled AWS access

---

# 🔒 Ansible Vault

Sensitive environment-specific configuration is protected using
Ansible Vault.

Example file:

```text
group_vars/prod/vault.yml
```

Example content before encryption:

```yaml
db_username: "order_service"
db_password: "CHANGE_ME"
api_key: "CHANGE_ME"
```

Encrypt the file:

```bash
ansible-vault encrypt group_vars/prod/vault.yml
```

View encrypted content:

```bash
ansible-vault view group_vars/prod/vault.yml
```

Edit:

```bash
ansible-vault edit group_vars/prod/vault.yml
```

Run a playbook with Vault:

```bash
ansible-playbook \
  -i inventory/prod/hosts.ini \
  playbooks/common-test.yml \
  --ask-vault-pass
```

### Vault Design

```text
Git Repository
      │
      ├── group_vars/
      │
      │   └── prod/
      │       └── vault.yml
      │
      └── Encrypted Secrets
```

Sensitive values should never be stored in plain text inside Git.

---

# 🌎 Environment Strategy

The project supports environment-specific configuration.

Current environments:

```text
DEV
QA
PROD
```

Example structure:

```text
group_vars/
├── all.yml
├── dev.yml
├── qa.yml
└── prod.yml
```

### DEV

```yaml
deployment_environment: dev
app_version: "1.0"
```

### QA

```yaml
deployment_environment: qa
app_version: "1.0"
```

### PROD

```yaml
deployment_environment: prod
app_version: "1.0"
```

### Environment Flow

```text
                Ansible Roles
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
       DEV          QA           PROD
        │            │            │
     dev.yml       qa.yml      prod.yml
        │            │            │
        ▼            ▼            ▼
    Environment-specific variables
```

The same roles can therefore be reused across environments while
environment-specific variables are maintained separately.

---

# 📁 Project Structure

Recommended project structure:

```text
enterprise-java-order-management/
│
├── ansible.cfg
├── README.md
├── requirements.yml
├── .gitignore
│
├── inventory/
│   │
│   ├── dev/
│   │   └── hosts.ini
│   │
│   ├── qa/
│   │   └── hosts.ini
│   │
│   ├── prod/
│   │   └── hosts.ini
│   │
│   └── aws/
│       └── dev.aws_ec2.yml
│
├── group_vars/
│   │
│   ├── all.yml
│   ├── dev.yml
│   ├── qa.yml
│   ├── prod.yml
│   │
│   └── prod/
│       └── vault.yml
│
├── playbooks/
│   │
│   ├── common-test.yml
│   ├── deploy.yml
│   └── rollback.yml
│
├── roles/
│   │
│   ├── common/
│   │   ├── defaults/
│   │   ├── handlers/
│   │   ├── tasks/
│   │   ├── templates/
│   │   └── vars/
│   │
│   ├── security/
│   │
│   ├── java/
│   │
│   ├── nginx/
│   │
│   ├── application/
│   │
│   └── monitoring/
│
└── screenshots/
```

---

# 🧪 Validation and Testing

The project includes multiple levels of validation.

## 1. Inventory Validation

```bash
ansible-inventory \
  -i inventory/aws/dev.aws_ec2.yml \
  --graph
```

## 2. Connectivity Test

```bash
ansible \
  -i inventory/aws/dev.aws_ec2.yml \
  app_servers \
  -m ping
```

## 3. Syntax Check

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml \
  --syntax-check
```

## 4. Ansible Lint

```bash
ansible-lint
```

## 5. Check Mode

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml \
  --check
```

## 6. Deployment

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml
```

## 7. Application Verification

```bash
curl http://127.0.0.1:8080/health
```

## 8. Nginx Verification

```bash
curl http://127.0.0.1/health
```

## 9. Service Verification

```bash
sudo systemctl status abc-order-service
```

## 10. Idempotency Test

Run the same playbook again:

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml
```

The second execution should report fewer or no changes when the system
is already in the desired state.

---

# 🔁 Idempotency

Idempotency is one of the core principles demonstrated by this project.

Ansible tasks are designed so that repeated execution produces the same
desired state instead of repeatedly modifying the server.

### Example

First execution:

```text
changed: 4
ok: 27
```

Second execution:

```text
changed: 0
ok: 31
```

### Why Idempotency Matters

Without idempotency:

```text
Run 1 → Change
Run 2 → Change
Run 3 → Change
Run 4 → Change
```

With idempotency:

```text
Run 1 → Configure
Run 2 → Already Correct
Run 3 → Already Correct
Run 4 → Already Correct
```

This makes automation safe to execute repeatedly.

---

# 🔔 Ansible Handlers

Handlers are used when a configuration change requires a service reload
or restart.

### Application Example

If the application configuration changes:

```text
Configuration Changed
        │
        ▼
Handler Triggered
        │
        ▼
Restart Application
```

### Nginx Example

When the Nginx configuration changes:

```text
Nginx Configuration Changed
        │
        ▼
Handler Triggered
        │
        ▼
Reload Nginx
```

Example:

```yaml
notify:
  - Restart application
```

And the handler:

```yaml
- name: Restart application
  ansible.builtin.systemd:
    name: abc-order-service
    state: restarted
    daemon_reload: true
```

### Benefits

- Prevent unnecessary restarts
- Restart only when configuration changes
- Keep tasks clean
- Improve idempotency
- Reduce service disruption

---

# 🏷️ Ansible Tags

Tags allow selected tasks to be executed without running the entire
playbook.

Example tags used in the Java role:

```yaml
tags:
  - java
  - install
```

Configuration:

```yaml
tags:
  - java
  - configure
```

Validation:

```yaml
tags:
  - java
  - validate
```

### List Tags

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml \
  --list-tags
```

### Run Only Java Tasks

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml \
  --tags java
```

### Why Tags?

Tags are useful for:

- Debugging
- Partial deployments
- Targeted configuration
- Faster development
- Operational troubleshooting

---

# 🔗 Role Dependencies

Roles are intentionally separated by responsibility.

```text
common
   │
   ▼
security
   │
   ▼
java
   │
   ▼
nginx
   │
   ▼
application
   │
   ▼
monitoring
```

### Dependency Logic

The application role assumes:

```text
common → Base directories/users
security → Secure server
java → Java installed
nginx → Reverse proxy available
application → Java application deployed
monitoring → Monitoring configured
```

This allows each role to have a clear responsibility.

---

# ♻️ Role Reusability

The project uses reusable Ansible roles instead of placing every task in
one large playbook.

For example:

```text
java role
```

can be reused for another Java application.

Similarly:

```text
nginx role
```

can be reused for another application requiring Nginx as a reverse proxy.

The application-specific values are controlled through variables.

Example:

```yaml
app_name: abc-order-service
app_port: 8080
app_version: "1.0"
app_root: /opt/abc-order-service
```

### Benefits

- Reduced duplication
- Easier maintenance
- Consistent configuration
- Environment portability
- Faster onboarding
- Better separation of concerns

---

# 📊 Monitoring

The project includes AWS CloudWatch monitoring through the monitoring role.

### Monitoring Architecture

```text
                EC2 Instances
                     │
       ┌─────────────┼─────────────┐
       │             │             │
       ▼             ▼             ▼
      CPU          Memory         Disk
       │             │             │
       └─────────────┼─────────────┘
                     │
                     ▼
              CloudWatch Agent
                     │
                     ▼
              Amazon CloudWatch
```

Application and Nginx logs can also be integrated into CloudWatch for
centralized observability.

### Monitoring Areas

```text
EC2 CPU
EC2 Memory
Disk Utilization
Application Logs
Nginx Logs
Application Health
```

### Monitoring Objective

The monitoring role provides a foundation for observing application
servers without manually logging into each EC2 instance.

---

# 🧠 DevOps Concepts Demonstrated

This project demonstrates several real-world DevOps practices.

### Infrastructure Automation

```text
Manual Configuration
        ↓
Ansible Automation
```

### Configuration Management

```text
Desired State
     ↓
Ansible
     ↓
EC2 Servers
```

### Dynamic Infrastructure

```text
AWS EC2
   ↓
Tags
   ↓
Dynamic Inventory
   ↓
Ansible
```

### Artifact Management

```text
Build Artifact
     ↓
Amazon S3
     ↓
Versioned Deployment
```

### Continuous Delivery Concepts

```text
Artifact
   ↓
Deploy
   ↓
Health Check
   ↓
Validate
   ↓
Release
```

### Safe Deployment

```text
serial: 1
```

ensures that application instances are updated one at a time.

### Failure Recovery

```text
Failure
   ↓
Troubleshooting
   ↓
Recovery
   ↓
Health Validation
```

### Rollback

```text
Version 1.1
     ↓
Deployment Failure
     ↓
Version 1.0
```

### Security

```text
IAM
+
Security Groups
+
SSH
+
Ansible Vault
```

### Automation Quality

```text
Syntax Check
      +
Ansible Lint
      +
Check Mode
      +
Idempotency
```

---

# 💼 What I Built

I built an enterprise-style Java application deployment platform using AWS
and Ansible.

The platform automates the complete lifecycle of a Java Order Management
application.

### Infrastructure

- Ubuntu EC2 application servers
- AWS Security Groups
- IAM permissions
- Dynamic EC2 inventory

### Configuration Management

- Reusable Ansible roles
- Environment-specific variables
- Idempotent configuration
- Ansible handlers
- Ansible tags

### Application Platform

- Java 17
- Systemd service
- Nginx reverse proxy
- Application configuration
- Application health endpoint

### Deployment

- S3-based artifact management
- Versioned application releases
- Rolling deployments
- Post-deployment health checks

### Reliability

- Failure simulation
- Troubleshooting
- Automated recovery
- Version-based rollback

### Security

- SSH-based access
- IAM permissions
- Security Groups
- Ansible Vault

### Observability

- CloudWatch Agent
- EC2 monitoring
- Application logs
- Nginx logs

---

# 💻 Technology Stack

| Technology | Usage |
|---|---|
| **AWS EC2** | Application servers |
| **AWS S3** | Application artifact repository |
| **AWS CloudWatch** | Monitoring and logging |
| **AWS IAM** | Access control |
| **AWS VPC** | Networking |
| **Ansible** | Automation |
| **Ansible Vault** | Secrets management |
| **Ansible Lint** | Automation quality |
| **Ubuntu Linux** | Server operating system |
| **Java 17** | Application runtime |
| **Spring Boot** | Java application framework |
| **Nginx** | Reverse proxy |
| **Systemd** | Application service management |
| **Bash** | Linux automation and troubleshooting |
| **Git** | Version control |
| **GitHub** | Source code repository |

---

# 📸 Screenshots

Screenshots can be added to this section to demonstrate the actual
implementation.

Recommended screenshots:

### AWS EC2 Instances

```text
screenshots/aws-ec2-instances.png
```

### EC2 Tags

```text
screenshots/ec2-tags.png
```

### Dynamic Inventory

```text
screenshots/dynamic-inventory.png
```

### Ansible Deployment

```text
screenshots/ansible-deployment.png
```

### Rolling Deployment

```text
screenshots/rolling-deployment.png
```

### Application Health Check

```text
screenshots/application-health.png
```

### CloudWatch

```text
screenshots/cloudwatch.png
```

### Failure Recovery

```text
screenshots/failure-recovery.png
```

### Rollback

```text
screenshots/rollback.png
```

When screenshots are added to the repository, they can be displayed using:

```markdown
![AWS EC2 Instances](screenshots/aws-ec2-instances.png)
```

---

# 🚀 Quick Start

## 1. Clone Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd enterprise-java-order-management
```

## 2. Install Ansible

Ubuntu example:

```bash
sudo apt update
sudo apt install -y ansible
```

Verify:

```bash
ansible --version
```

## 3. Install Required Collections

Example:

```bash
ansible-galaxy collection install amazon.aws
```

Install Python dependencies required for AWS dynamic inventory:

```bash
python3 -m venv .venv
source .venv/bin/activate

pip install boto3 botocore
```

## 4. Configure AWS Credentials

Verify AWS authentication:

```bash
aws sts get-caller-identity
```

The AWS identity must have permission to discover the required EC2
instances.

## 5. Configure EC2 Tags

Example:

```text
Environment = dev
Application = order-service
Role        = app
```

## 6. Verify Dynamic Inventory

```bash
ansible-inventory \
  -i inventory/aws/dev.aws_ec2.yml \
  --graph
```

## 7. Test Connectivity

```bash
ansible \
  -i inventory/aws/dev.aws_ec2.yml \
  app_servers \
  -m ping
```

## 8. Syntax Check

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml \
  --syntax-check
```

## 9. Run Ansible Lint

```bash
ansible-lint
```

## 10. Run Check Mode

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml \
  --check
```

## 11. Deploy

```bash
ansible-playbook \
  -i inventory/aws/dev.aws_ec2.yml \
  playbooks/common-test.yml
```

## 12. Verify

```bash
ansible \
  -i inventory/aws/dev.aws_ec2.yml \
  app_servers \
  -m shell \
  -a "systemctl status abc-order-service --no-pager"
```

Health check:

```bash
curl http://<SERVER-IP>/health
```

---

# 🔮 Future Enhancements

The following improvements can be added as the project evolves.

## CI/CD Pipeline

Integrate GitHub Actions or another CI/CD platform.

```text
Git Push
   ↓
CI Pipeline
   ↓
Build
   ↓
Test
   ↓
Ansible Lint
   ↓
Artifact
   ↓
S3
   ↓
Deployment
```

## Terraform

Provision AWS infrastructure using Terraform.

```text
Terraform
   ↓
VPC
   ↓
Security Groups
   ↓
EC2
   ↓
S3
   ↓
IAM
```

## Application Load Balancer

Introduce an AWS Application Load Balancer in front of the EC2 instances.

```text
Internet
   ↓
ALB
   ↓
┌─────────────┐
│             │
▼             ▼
App01        App02
```

## Auto Scaling Group

Move from manually managed EC2 instances to an Auto Scaling Group.

## Blue-Green Deployment

Introduce parallel environments:

```text
Blue  → Current Version
Green → New Version
```

Traffic can then be switched after successful validation.

## Automated Testing

Add:

- Unit testing
- Integration testing
- API testing
- Molecule testing for Ansible roles

## Centralized Secrets Management

Integrate:

- AWS Secrets Manager
- AWS Systems Manager Parameter Store

## Advanced Observability

Add:

- CloudWatch alarms
- Application metrics
- Alerting
- Dashboards
- Log aggregation

---

# ⚠️ Security Notice

Do not commit sensitive information to GitHub.

Never commit:

```text
AWS Access Keys
AWS Secret Keys
Passwords
Private SSH Keys
API Keys
Database Credentials
Vault Passwords
```

Use:

```text
Ansible Vault
AWS IAM
AWS Secrets Manager
Environment Variables
GitHub Secrets
```

### Recommended Practice

Before pushing the repository:

```bash
git status
```

Review:

```bash
git diff
```

Check for accidentally exposed secrets.

---

# 📝 Recommended .gitignore

Use a `.gitignore` similar to:

```gitignore
# Python virtual environments
.venv/
venv/
env/

# Python cache
__pycache__/
*.pyc

# Ansible retry files
*.retry

# Ansible Vault password files
.vault_password
vault_password.txt

# SSH keys
*.pem
*.key
id_rsa
id_rsa.pub

# AWS credentials
.aws/

# Environment files
.env
.env.*

# IDE files
.vscode/
.idea/

# OS files
.DS_Store
Thumbs.db

# Logs
*.log

# Temporary files
*.tmp
*.bak
```

---

# 📜 License

This project is licensed under the MIT License.

See the [LICENSE](LICENSE) file for details.

---

# 👨‍💻 Author

## AWS DevOps Engineer

**Rajesh Kumar Samal**

### Core Skills

```text
AWS
Ansible
Linux
Docker
Git
GitHub
Bash
Java
Spring Boot
Nginx
Amazon S3
CloudWatch
IAM
CI/CD
Infrastructure Automation
Configuration Management
```

---

# 🏆 Project Highlights

```text
✅ AWS EC2 Infrastructure
✅ Dynamic EC2 Inventory
✅ Reusable Ansible Roles
✅ Java 17
✅ Spring Boot Application
✅ Nginx Reverse Proxy
✅ Systemd Service Management
✅ S3 Artifact Management
✅ Versioned Application Releases
✅ Rolling Deployment
✅ Application Health Checks
✅ Failure Simulation
✅ Failure Recovery
✅ Version-Based Rollback
✅ Ansible Vault
✅ IAM Security
✅ CloudWatch Monitoring
✅ Ansible Tags
✅ Ansible Handlers
✅ Idempotent Automation
✅ Ansible Lint
✅ Check Mode Validation
✅ Environment-Specific Configuration
```

---

# 🔄 End-to-End DevOps Lifecycle

The complete lifecycle implemented by this project can be represented as:

```text
                  ┌───────────────────┐
                  │   Java Developer  │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │   Java Build/JAR  │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │    Amazon S3      │
                  │ Versioned Artifact │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │     Ansible       │
                  │  Control Node     │
                  └─────────┬─────────┘
                            │
                            ▼
                  ┌───────────────────┐
                  │ Dynamic Inventory │
                  │    EC2 Tags       │
                  └─────────┬─────────┘
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
       ┌──────────────┐           ┌──────────────┐
       │    App01     │           │    App02     │
       │ Ubuntu EC2   │           │ Ubuntu EC2   │
       └──────┬───────┘           └──────┬───────┘
              │                           │
              ▼                           ▼
          Nginx :80                  Nginx :80
              │                           │
              ▼                           ▼
       Java Application           Java Application
           :8080                       :8080
              │                           │
              └─────────────┬─────────────┘
                            │
                            ▼
                    Health Validation
                            │
                            ▼
                    CloudWatch Monitoring
                            │
                            ▼
                   Failure / Recovery
                            │
                            ▼
                       Rollback
```

---

# 🎯 What This Project Demonstrates in an Interview

This project can be explained as a complete DevOps automation solution
rather than simply an Ansible installation project.

### Infrastructure

I worked with AWS EC2 instances and configured them as application servers.

### Dynamic Inventory

Instead of hardcoding IP addresses, I used AWS EC2 tags and the Ansible
AWS EC2 dynamic inventory plugin to discover application servers.

### Configuration Management

I created reusable Ansible roles for common configuration, security,
Java, Nginx, application deployment and monitoring.

### Application Deployment

The Java application artifact is stored in Amazon S3 and downloaded by
Ansible during deployment.

### Deployment Strategy

The application is deployed using a rolling strategy with:

```yaml
serial: 1
```

This ensures that only one application server is updated at a time.

### Validation

After deployment, Ansible validates the application's health endpoint.

```text
/health
```

The deployment succeeds only after the application returns HTTP 200.

### Failure Handling

I intentionally simulated an application failure by making the JAR
unavailable and then investigated the failure using systemd and journal
logs.

### Recovery

The application artifact can be restored from S3 and the application
service restarted.

### Rollback

The deployment supports version-based rollback by changing the
application version variable.

### Security

Secrets are protected using Ansible Vault and AWS access is controlled
using IAM permissions.

### Monitoring

CloudWatch is used to provide monitoring and centralized visibility into
the infrastructure and application environment.

---

# 📌 Summary

This project demonstrates a production-oriented approach to deploying and
managing a Java application on AWS using Ansible.

The solution combines:

```text
AWS
+
EC2
+
S3
+
CloudWatch
+
IAM
+
Ansible
+
Dynamic Inventory
+
Reusable Roles
+
Java 17
+
Nginx
+
Systemd
+
Rolling Deployment
+
Health Checks
+
Failure Recovery
+
Rollback
+
Ansible Vault
+
Idempotency
```

The key objective is to demonstrate how infrastructure automation,
configuration management, application deployment, validation, monitoring
and recovery can be combined into a single reusable DevOps workflow.

```text
                    ┌─────────────────────┐
                    │       AWS EC2       │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Dynamic Inventory   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Ansible Roles       │
                    └──────────┬──────────┘
                               │
             ┌─────────────────┼─────────────────┐
             │                 │                 │
             ▼                 ▼                 ▼
          Java 17            Nginx          Monitoring
             │                 │                 │
             └─────────────────┼─────────────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Java Application    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Health Check        │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Rolling Deployment  │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Failure Recovery    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Version Rollback    │
                    └─────────────────────┘
```

---

## 🚀 End of Project

> **Enterprise Java Order Management Deployment on AWS Using Ansible**
>
> AWS EC2 • Dynamic Inventory • Ansible Roles • Java 17 • Nginx • S3 •
> CloudWatch • Rolling Deployment • Health Checks • Failure Recovery •
> Rollback • Ansible Vault • Idempotent Automation
