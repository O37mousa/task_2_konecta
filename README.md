# 🚀 Automating Integration Between AWS EC2 Instances Using Ansible

This guide walks through automating the integration of three AWS EC2 instances using **Ansible**. The architecture includes:

- 🧠 **Control Machine** – Ansible control node  
- 🌐 **Frontend Machine** – Hosts an NGINX web server  
- 🐳 **Docker & Redis Machine** – Runs Docker and Redis

The goal is to automate configuration and deployment using Ansible from the control machine.

---



## ☁️ Step 1: Launch EC2 Instances on AWS

- Launch **three EC2 instances** using **Ubuntu 20.04 or 22.04 LTS**.
- While creating the **Ansible Control Machine**, download the key pair file (e.g., `ansible-key.pem`).
- Use this key to establish secure SSH connections from the control node to the other instances.

### 🔐 Alternative: Generate Your Own SSH Key

On the **control machine**, you can generate a new SSH key:

```ssh-keygen -t rsa -b 4096```




## 🔄 Step 2: Copy SSH Key to Target Machines
Ensure the control machine can SSH into the other two machines.

Replace the IPs with your actual instance IP addresses.

### Copy key to Frontend Machine
`scp -i ansible-key.pem ansible-key.pem ubuntu@13.62.52.225:~/`

### Copy key to Docker Machine
`scp -i ansible-key.pem ansible-key.pem ubuntu@13.53.35.182:~/`

Set proper permissions on the key file:
`chmod 400 ansible-key.pem`





## 🔌 Step 3: Verify SSH Connectivity
Log in from the control machine to each node to ensure SSH works:

🔗 Connect to Docker Machine
`ssh -i ansible-key.pem ubuntu@13.53.35.182`

🔗 Connect to Frontend Machine
`ssh -i ansible-key.pem ubuntu@13.62.52.225`






## 📁 Step 4: Setup Inventory and Ansible Playbooks
On the Control Machine, create an inventory file to define hosts.

Example `inventory.ini`
```
[frontend]
frontend ansible_host=13.62.52.225 ansible_user=ubuntu ansible_ssh_private_key_file=~/ansible-key.pem

[docker]
docker ansible_host=13.53.35.182 ansible_user=ubuntu ansible_ssh_private_key_file=~/ansible-key.pem
```

Create Ansible playbooks for configuring:

- NGINX on the frontend

- Docker and Redis on the backend



## ✅ Step 5: Validate Running Services
Once playbooks are run, verify that:

✅ Docker container is running on the Docker machine

