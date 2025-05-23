# V Profile Project - Automated Setup of Local Multi Tier App

## 01. Introduction

This section introduces how to **automatically provision the entire application stack** using Vagrant and Bash scripts. Unlike the previous project where all components were set up manually, this setup automates everything — saving time and ensuring consistency.

---

### 🚀 Objective

Automate the provisioning of the following services:

- ✅ Nginx (Reverse Proxy)
- ✅ Tomcat (Java Application Server)
- ✅ RabbitMQ (Message Broker)
- ✅ Memcached (In-memory Cache)
- ✅ MySQL (Relational Database)
- ✅ View Profile Application (Java-based Web App)

All of this will be done with a **single command**:

```bash
vagrant up
```

---

### ⚙️ How It Works

This setup builds on the same architecture from the previous lesson, with a key difference:

🧠 Instead of manually installing and configuring services, we now use Bash scripts and Vagrant provisioning to do it automatically.

| Component         | Description                                    |
| ----------------- | ---------------------------------------------- |
| `Vagrantfile`     | Defines the virtual machines and networking    |
| Bash Scripts      | Installed in each VM to automate service setup |
| Shell Provisioner | Triggers the bash scripts during `vagrant up`  |

---

### 🧳 What Gets Provisioned Automatically?

When you run `vagrant up`, the following happens automatically:

1. All required virtual machines are created.
2. Each VM executes its associated bash script.
3. Bash scripts perform:
   - System updates (apt update && apt upgrade)
   - Installation and configuration of:
     - Nginx (reverse proxy setup)
     - Tomcat (deployment of Java web app)
     - RabbitMQ, Memcached, and MySQL
   - App deployment and database initialization

---

### 🎯 Purpose of This Automation

- Avoid manual errors.
- Speed up environment setup.
- Help simulate real-world DevOps workflows.
- Prepare for CI/CD and infrastructure-as-code (IaC) tools (e.g., Ansible, Terraform).

---

---

---
