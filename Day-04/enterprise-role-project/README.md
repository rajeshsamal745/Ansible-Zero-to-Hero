📌 Project Overview
This project automates the deployment and management of Apache HTTP Server on two Ubuntu EC2 instances (web01 and web02) using Ansible. It goes beyond a simple installation by implementing:

✅ Idempotent role – run the playbook multiple times without unexpected changes.
✅ Handler‑driven service restarts – Apache is restarted only when configuration changes.
✅ Failure recovery – automatically recreates deleted files (e.g., index.html).
✅ Validation tasks – verify service status, config syntax, and HTTP responses.
✅ Rolling updates with serial: 1 to limit blast radius in production.

This repository serves as a portfolio piece demonstrating infrastructure as code, configuration management best practices, and DevOps engineering skills.
🏗️ Architecture
+-------------------+          +-------------------+
|  Control Host     |          |   AWS EC2         |
|  (WSL / Linux)    |          |   Ubuntu 22.04    |
|  Ansible 2.9+     |  SSH     |   web01 (public)  |
|                   |--------->|   Apache          |
|  Inventory        |          |                   |
|  playbooks/       |          +-------------------+
|  roles/           |
|                   |          +-------------------+
|                   |  SSH     |   AWS EC2         |
|                   |--------->|   Ubuntu 22.04    |
|                   |          |   web02 (public)  |
|                   |          |   Apache          |
+-------------------+          +-------------------+
Control Host: WSL (Ubuntu 22.04) with Ansible installed.
Managed Nodes: Two EC2 t2.micro instances running Ubuntu 22.04.
Security Groups: Allow SSH (22) and HTTP (80) from anywhere (demo only).
SSH Key‑based authentication using ~/.ssh/id_rsa.

📂 Project Structure
text
enterprise-role-project/
├── roles/
│   └── apache/                       # The Apache role (modular, reusable)
│       ├── defaults/
│       │   └── main.yml              # Default variables (port, admin email)
│       ├── handlers/
│       │   └── main.yml              # Handler: restart apache
│       ├── tasks/
│       │   ├── main.yml              # Main tasks (install, config, deploy)
│       │   └── validate.yml          # Post‑deployment validation
│       ├── templates/
│       │   ├── apache.conf.j2        # VirtualHost template
│       │   └── index.html.j2         # Website page template
│       ├── vars/
│       │   └── main.yml              # Distribution‑specific variables
│       └── meta/
│           └── main.yml              # Role metadata
├── inventory                         # Static inventory (web01, web02)
├── site.yml                          # Main playbook (serial=1, validation)
└── README.md                         # This file

⚙️ Prerequisites
AWS Account – to launch EC2 instances.
Ansible installed on the control host (version 2.9+).
SSH key pair (.pem or id_rsa) already uploaded to AWS.
Basic knowledge of Linux, SSH, and the command line.

🔧 Setup Instructions
1. Launch Two EC2 Instances on AWS
AMI: Ubuntu 22.04 LTS
Instance type: t2.micro (free tier eligible)
Security Group rules:
SSH (22) from your IP (or 0.0.0.0/0 for testing)
HTTP (80) from 0.0.0.0/0
Key pair: Select your existing key pair.
Tags: Name = web01 and web02 (launch separately or rename after).
Note the public IPv4 addresses of both instances.

2. Configure the Control Host
Update the inventory file inventory with the actual IPs:

ini
[webservers]
web01 ansible_host=<web01-public-ip>
web02 ansible_host=<web02-public-ip>
Test connectivity:

bash
ansible -i inventory webservers -m ping -u ubuntu --private-key ~/.ssh/ansible-key
3. Clone or Create the Project
bash
git clone https://github.com/rajeshsamal745/enterprise-role-project.git
cd enterprise-role-project
🚀 Deploying Apache with Ansible
Run the playbook:

bash
ansible-playbook -i inventory site.yml -u ubuntu --private-key ~/.ssh/ansible-key
Expected output – first run:

text
PLAY RECAP
web01 : ok=7  changed=5  unreachable=0  failed=0
web02 : ok=7  changed=5  unreachable=0  failed=0
After deployment, open http://<web01-ip> in your browser – you should see a custom HTML page displaying the server’s hostname.

🔁 Idempotency Test
Run the same command again. You should see changed=0 for all tasks, proving that the role is idempotent – it only makes changes when the system drifts from the desired state.

🧪 Key Features Demonstrated
✅ Idempotent Configuration
The role uses state: present, template with diff, and service with state: started – all are idempotent by design.
✅ Handler‑Driven Restarts
When the Apache configuration template changes, a notify triggers the handler, which restarts Apache only if necessary.
A second run with no configuration changes does not restart Apache.
✅ Recovery from Accidental Deletion
Simulate a failure: sudo rm /var/www/html/index.html on one node.
Rerunning the playbook recreates the file, restoring the service without manual intervention.
✅ Rolling Updates (serial: 1)
The playbook uses serial: 1 to update one server at a time.
Production‑grade – if one node fails, the other remains unaffected, minimising downtime.
✅ Automated Validation
After each host deployment, the validate.yml tasks run to:
Verify that the apache2 service is running.
Run apache2ctl configtest to ensure syntax is valid.
Send an HTTP request to localhost to confirm a 200 OK response.
This ensures the deployment is fully functional before moving to the next host.
✅ Failure Handling & Recovery
We deliberately introduce an invalid directive in apache.conf.j2 to see the playbook fail.
After fixing the template, the playbook succeeds – demonstrating how to handle configuration errors in a controlled manner.

📝 How to Run the Validation Separately
If you want to validate only, you can create a dedicated validation playbook:

yaml
- hosts: webservers
  become: true
  tasks:
    - import_tasks: roles/apache/tasks/validate.yml
🛠️ Customization
Change the Apache port by updating apache_port in roles/apache/defaults/main.yml.

Modify the web page content in roles/apache/templates/index.html.j2.
Add more hosts to the inventory file to scale out.

📊 Sample Execution Log
text
PLAY [Configure web servers] ***************************************************

TASK [Gathering Facts] *********************************************************
ok: [web01]
ok: [web02]

TASK [apache : Install Apache] *************************************************
ok: [web01]
ok: [web02]

TASK [apache : Enable and start Apache] ****************************************
ok: [web01]
ok: [web02]

TASK [apache : Copy Apache virtual host configuration] *************************
changed: [web01]
changed: [web02]

TASK [apache : Copy index.html] ************************************************
changed: [web01]
changed: [web02]

RUNNING HANDLER [apache : restart apache] **************************************
changed: [web01]
changed: [web02]

TASK [Gathering service facts] *************************************************
ok: [web01]
ok: [web02]

TASK [Verify Apache service is running] ****************************************
ok: [web01] => {
    "changed": false,
    "msg": "Apache service is running"
}
ok: [web02] => {
    "changed": false,
    "msg": "Apache service is running"
}

TASK [Verify Apache configuration is valid] ************************************
ok: [web01]
ok: [web02]

TASK [Verify HTTP response (200 OK)] *******************************************
ok: [web01]
ok: [web02]

PLAY RECAP *********************************************************************
web01 : ok=9  changed=2  unreachable=0  failed=0
web02 : ok=9  changed=2  unreachable=0  failed=0

🧠 Lessons Learned
Ansible roles keep your playbook clean and reusable.
Handlers are essential for efficient service management.
Serial execution reduces risk during deployments.
Validation tasks build confidence in automation.
Idempotency is not automatic – it requires careful task design.

🚧 Troubleshooting
SSH connection refused – check security group and key pair.
validate.yml not found – ensure you have the correct directory structure and path.
Apache fails to restart – run apache2ctl configtest manually to see the error.
HTTP 403/404 – verify the DocumentRoot and file permissions.

🔮 Future Improvements
Add dynamic inventory using the AWS EC2 plugin.
Integrate with Terraform to provision EC2 instances automatically.
Add monitoring (e.g., Prometheus exporter) via Ansible.
Implement canary deployments using load balancer logic.

📬 Connect
GitHub: https://github.com/rajeshsamal745
Email:rajeshsamal745@gmail.com


