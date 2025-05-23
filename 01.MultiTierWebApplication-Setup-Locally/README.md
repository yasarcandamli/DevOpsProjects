# V Profile Project - Local Multi-Tier Web Application Setup

## 01. Introduction

Welcome to the **V Profile Project** – a foundational DevOps project aimed at setting up a **multi-tier web application stack** in a local environment. This project serves as the baseline for many upcoming hands-on DevOps initiatives, including containerization, Kubernetes deployments, CI/CD implementations, and infrastructure automation.

The project simulates a **real-world web application** stack typically found in production environments, and allows developers and DevOps engineers to conduct R&D locally without relying on shared or cloud environments. It promotes best practices in automation and repeatability using tools like **Vagrant**, **VirtualBox**, and **Bash scripting**.

---

### Project Architecture

![alt text](<Project Architecture.png>)

---

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
