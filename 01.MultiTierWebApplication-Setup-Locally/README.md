# V Profile Project - Local Multi-Tier Web Application Setup

## 01. Introduction

Welcome to the **V Profile Project** – a foundational DevOps project aimed at setting up a **multi-tier web application stack** in a local environment. This project serves as the baseline for many upcoming hands-on DevOps initiatives, including containerization, Kubernetes deployments, CI/CD implementations, and infrastructure automation.

The project simulates a **real-world web application** stack typically found in production environments, and allows developers and DevOps engineers to conduct R&D locally without relying on shared or cloud environments. It promotes best practices in automation and repeatability using tools like **Vagrant**, **VirtualBox**, and **Bash scripting**.

---

### Project Architecture

## ![alt text](<images/00.Project Architecture.png>)

### Objectives

- Learn how to set up a complete application stack locally using automation.
- Gain hands-on experience with common services used in enterprise environments.
- Build a reusable local lab for testing, experimentation, and learning.
- Prepare a baseline project for future projects involving Docker, Kubernetes, Ansible, Jenkins, and more.

---

### Tools & Technologies

- **Oracle VM VirtualBox**: Hypervisor to run virtual machines locally.
- **Vagrant**: Tool to automate VM creation and provisioning.
- **Git Bash**: Command-line interface for executing scripts and Git commands.
- **Text Editors/IDEs**: Visual Studio Code, Sublime Text, Notepad++, etc.
- **Shell Scripts**: For service setup and automation.

---

### Application Stack Overview

This project simulates a **social networking web application** developed in **Java**, hosted on an **Apache Tomcat** server, and accessible via a **load-balanced** setup using **Nginx**.

The stack includes the following services:

- **Nginx** – Web server and load balancer.
- **Apache Tomcat** – Java application server.
- **RabbitMQ** – Message broker (dummy setup for practice).
- **Memcached** – In-memory caching layer.
- **MySQL** – Relational database for storing user data.

Each service runs on its **own virtual machine**, and the entire environment is orchestrated using **Vagrant**.

---

### Project Flow Summary

1. **Set up tools** (VirtualBox, Vagrant, Git Bash).
2. **Clone the repository** containing the source code and Vagrantfile.
3. **Use Vagrant** to provision all required virtual machines.
4. **Install and configure** each service (MySQL, Memcached, RabbitMQ, Tomcat, Nginx).
5. **Deploy the Java web application**.
6. **Access the application** through a browser using the IP of the Nginx load balancer.

---

### Why This Project Matters

- Establishes a repeatable and automated environment for testing and learning.
- Builds confidence before making changes in real or production servers.
- Enables you to simulate complex application environments locally.
- Serves as a base for containerization, Kubernetes deployments, and CI/CD pipelines.

---

---

---

## 02. VM Setup

This guide outlines the steps to set up virtual machines using **Vagrant** for the **V Profile Project**. This setup allows you to simulate a complete multi-tier application stack on your local machine for testing, learning, and DevOps experimentation.

---

### 🧰 Prerequisites

Make sure the following tools are installed on your system:

- Oracle VM VirtualBox
- Vagrant
- Git
- A terminal (Mac/Linux) or Git Bash (Windows)

---

### 📦 Clone the Repository

1. Open [GitHub Repo](https://github.com/hkhcoder/vprofile-project) in your browser.
2. Copy the HTTPS URL.
3. Open VS Code (or any other IDE/Terminal of your choice).
4. Clone the repository:

   ```bash
   git clone https://github.com/hkhcoder/vprofile-project.git
   ```

5. Navigate to the cloned folder.

---

### 🌿 Switch to the Correct Git Branch

The source code has multiple branches. Make sure to switch to the `local` branch:

- In VS Code, click on the bottom-left branch name (e.g., `main`).

- Select `origin/local`.

You should now see the `local` branch checked out.

---

### 📁 Navigate to the Correct Folder

Choose the folder based on your OS architecture:

- Windows, Mac (Intel), Linux:

```bash
cd vprofile-project/vagrant/Manual_provisioning_WinMacIntel
```

- Mac (M1/M2/M3):

```bash
cd vprofile-project/vagrant/Manual_provisioning_MacOSM1
```

---

### 🔌 Install Required Vagrant Plugin

Run this command to install the plugin:

```bash
vagrant plugin install vagrant-hostmanager
```

This plugin auto-manages the host-to-IP mapping in the /etc/hosts file on your system.

---

### 🧹 (Optional) Clean Up Existing VMs

Check for any running VMs:

```bash
vagrant global-status
```

If any VM is active, destroy them:

```bash
cd <project-path>
vagrant destroy
```

Clean stale records:

```bash
vagrant global-status --prune
```

---

### 🚀 Start the VMs

Run the following command inside the correct folder:

```bash
vagrant up
```

- GitBash Screenshot:
  ![alt text](<images/01.vagrant up screenshot.png>)

- You can check from the Oracle VirtualBox interface:
  ![alt text](<images/02.Oracle VirtualBox.png>)

This will bring up five virtual machines:

- db01 – MySQL
- mc01 – Memcached
- rmq01 – RabbitMQ
- app01 – Tomcat (Java App)
- web01 – Nginx (Load Balancer)

💡 Note: First-time startup may take several minutes.

---

### 🧾 Validate Setup

After vagrant up, verify the following:

1. SSH into a VM:

```bash
vagrant ssh web01
```

2. Switch to Root User:

```bash
cat /etc/hosts
```

3. Check the host file:

```bash
cat /etc/hosts
```

You should see hostname-to-IP mappings like:

```bash
192.168.56.10   db01
192.168.56.11   mc01
192.168.56.12   rmq01
192.168.56.13   app01
192.168.56.14   web01
```

4. Ping services:

```bash
ping db01 -c 4
ping mc01 -c 4
```

- Screenshot:
  ![alt text](<images/03.vagrant ssh web01.png>)

---

### 🔄 Rebooting / Reloading VMs

If any VM fails to start or throws a timeout:

```bash
vagrant reload <vm-name>      # Reload specific VM
vagrant reload                # Reload all VMs
```

---

### 📐 VM Architecture

Each service is deployed on a separate VM:

```bash
[ Nginx (web01) ]
        ↓
[ Tomcat (app01) ]
        ↓
[ RabbitMQ (rmq01) ]
        ↓
[ Memcached (mc01) ]
        ↓
[ MySQL (db01) ]
```

---

### ⚠️ Order of Provisioning

It is recommended to configure services in the following order:

MySQL (db01)

Memcached (mc01)

RabbitMQ (rmq01)

Tomcat (app01)

Nginx (web01)

And shut them down in reverse order.

---

### 🧪 Optional Exercise

Log in to each VM using:

```bash
vagrant ssh <vm-name>
```

And try pinging the other services to verify host resolution:

```bash
ping db01 -c 4
ping mc01 -c 4
```

---

---

---
