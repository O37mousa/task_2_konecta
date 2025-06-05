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

![image](https://github.com/user-attachments/assets/e4aa5543-b295-4917-b879-c5dfe09091ed)



🔗 Connect to Frontend Machine
`ssh -i ansible-key.pem ubuntu@13.62.52.225`



![image](https://github.com/user-attachments/assets/de57b50f-783c-46bd-9fbb-da2aeb021d67)





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


## ✅ Step 6: On Ansible (control) machine, Run Playbooks
Run docker playbook using: 
`ansible-playbook -i inventory.ini playbooks/docker.yml`

![image](https://github.com/user-attachments/assets/2476bd47-42a5-41d7-b803-9873594c1984)

Run frontend playbook using: 
`ansible-playbook -i inventory.ini playbooks/frontend.yml`

![image](https://github.com/user-attachments/assets/ac080591-c3f0-4f0d-b151-f0d187fb0fb7)




## ✅ Step 6: Validate Running Services
Once playbooks are run, verify that:

✅ Docker container is running on the Docker machine

![image](https://github.com/user-attachments/assets/df4ca2c2-1e2d-4873-b5ee-0e422d4ebd87)
