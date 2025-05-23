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

- Screenshot:

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

## 03. MySQL Setup

This section walks you through the setup and configuration of **MySQL (MariaDB)** for the VProfile project. This is a critical step, as all services in the application depend on a properly configured and initialized database.

---

### 📦 Prerequisites

Before starting:

- Ensure all VMs are up and running.
- If you took a long break, run `vagrant reload` in the appropriate folder to restart the VMs:

  ```bash
  vagrant reload
  ```

Make sure you are inside the correct directory:
`vprofile-project/vagrant/Manual_provisioning_WinMacIntel`

---

### 🚪 Step 1: SSH into the DB VM

Log into the database VM:

```bash
vagrant ssh db01
```

Switch to the root user:

```bash
sudo -i
```

---

### 🔄 Step 2: System Update

Update the system to get the latest patches:

```bash
dnf update -y
```

---

### 📥 Step 3: Install Required Packages

Install EPEL repository, Git, and MariaDB Server:

```bash
dnf install epel-release -y
dnf install git mariadb-server -y
```

---

### ▶️ Step 4: Start and Enable MariaDB

Start and enable the database service:

```bash
systemctl start mariadb
systemctl enable mariadb
systemctl status mariadb
```

- Screenshot:

![alt text](<images/04.Start and Enable MariaDB.png>)

---

### 🔐 Step 5: Secure the Database

Run the security script to configure root access and remove vulnerabilities:

```bash
mysql_secure_installation
```

Answer the questions as follows:

- Current root password? → Press Enter (no password yet)

- Switch to unix_socket authentication? → Enter (default Yes)

- Change root password? → Yes
  Use: admin123

- Remove anonymous users? → Yes

- Disallow root login remotely? → Yes

- Remove test database? → Yes

- Reload privilege tables now? → Yes

---

### 💾 Step 6: Login to MySQL

Login with the root user:

```bash
mysql -u root -p
```

Enter password: `admin123`

---

### 🏗️ Step 7: Create Database and User

Inside the MySQL prompt:

```bash
CREATE DATABASE accounts;

GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'localhost' IDENTIFIED BY 'admin123';

GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'%' IDENTIFIED BY 'admin123';

FLUSH PRIVILEGES;

EXIT;
```

- Screenshot:

![alt text](<images/05.Login to MySQL , Create Database and User.png>)

---

### 🧬 Step 8: Load DB Schema (SQL Dump)

Clone the source code:

```bash
cd /tmp

git clone -b local https://github.com/hkhcoder/vprofile-project.git

cd vprofile-project
```

Navigate to SQL file location:

```bash
cd src/main/resources
```

Run SQL dump into the database:

```bash
mysql -u root -p accounts < db_backup.sql
```

Password: `admin123`

---

### 🔍 Step 9: Verify Database

Login to MySQL again:

```bash
mysql -u root -p accounts
```

List the tables:

```bash
SHOW TABLES;
```

You should see a list of tables populated from the `db_backup.sql`.

To check available databases:

```bash
SHOW DATABASES;
```

- Screenshot:

![alt text](<images/06.Verify Database.png>)

---

### 🔥 (Optional) Firewall Settings

⚠️ Skipped for now — These commands relate to firewall configuration and will be explained later with proper networking context.

---

### ✅ Summary

✔️ Updated OS and installed MariaDB

✔️ Secured root user and database

✔️ Created database accounts and user admin

✔️ Loaded schema using provided SQL dump

Your MySQL setup is now complete! 🎉

---

---

---

## 04. Memcached Setup

This document provides step-by-step instructions to set up the Memcached service for the VProfile project. Memcached is used here as a caching layer to improve performance.

### 🖥️ Target Environment

- VM Name: `mc01`
- OS: CentOS 8 (or similar)
- Tool: Vagrant
- User: `vagrant` (switch to root during setup)

---

### 🔧 Setup Steps

1. Connect to Memcached VM

```bash
vagrant ssh mc01
sudo -i  # Switch to root
```

2. Update System Packages (Optional)

```bash
dnf update -y
```

⚠️ This step is optional for this lab project **(takes long time)** but recommended in real-world setups to apply latest patches.

3. Install Required Packages
   Install EPEL repository and Memcached service:

```bash
dnf install epel-release -y
dnf install memcached -y
```

4. Enable and Start Memcached

```bash
systemctl start memcached
systemctl enable memcached
```

5. Configure Memcached for Remote Access
   By default, Memcached listens only to `127.0.0.1` (localhost). To allow connections from external services (e.g., Tomcat), change it to `0.0.0.0`.

Run the following command to modify the config:

```bash
sed -i 's/127.0.0.1/0.0.0.0/g' /etc/sysconfig/memcached
```

Verify change (optional):

```bash
cat /etc/sysconfig/memcached
```

- Screenshot:

![alt text](<images/07.Configure Memcached for Remote Access.png>)

6. Restart Memcached Service

```bash
systemctl restart memcached
```

7. Manually Start Memcached Daemon (Optional)
   Run Memcached manually with default port (11211: TCP port / 11111: UDP port):

```bash
memcached -p 11211 -U 11111 -u memcached -d
```

🔁 `-d` runs it as a background process (daemon).

---

### 🔥 Notes

- **Port 11211:** Memcached default port
- Ensure the configuration allows remote connections if your app runs on a different VM.
- Firewall settings are not mandatory for this lab. They will be discussed in detail in networking sections.

---

---

---

## 05. RabbitMQ Setup

This guide explains how to install and configure the RabbitMQ message queuing service for the VProfile project. RabbitMQ will be used to handle asynchronous communication between services.

---

### 🖥️ Target Environment

- VM Name: `rmq01`
- OS: CentOS 8 (or similar)
- Tool: Vagrant
- User: `vagrant` (switch to root during setup)

---

### 🔧 Setup Steps

1. Connect to RabbitMQ VM

```bash
vagrant ssh rmq01
sudo -i  # Switch to root
```

2. Update System Packages (Optional)

```bash
dnf update -y
```

⚠️ This step is optional but recommended for real-world usage.

3. Set EPEL Repository & Install Required Tools

```bash
dnf install epel-release -y

dnf install wget -y
```

4. Add RabbitMQ Repository

```bash
dnf -y install centos-release-rabbitmq-38
```

📝 This command adds the RabbitMQ YUM repository to your system.

5. Install RabbitMQ Server

```bash
dnf --enablerepo=centos-rabbitmq-38 -y install rabbitmq-server
```

6. Start and Enable RabbitMQ Service

```bash
systemctl enable --now rabbitmq-server
```

✅ This command both enables and starts the RabbitMQ service.

---

### 🛠️ Configuration Steps

7. Create RabbitMQ Configuration File

```bash
sudo sh -c 'echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config'
```

🧠 This allows remote connections by disabling default loopback restrictions.

8. Restart RabbitMQ to Apply Config

```bash
systemctl restart rabbitmq-server
```

---

### 👤 RabbitMQ User Management

9. Add a New User

```bash
rabbitmqctl add_user test test
```

10. Assign Administrator Tag to the User

```bash
rabbitmqctl set_user_tags test administrator
```

11. Set User Permissions

```bash
rabbitmqctl set_permissions -p / test ".*" ".*" ".*"

systemctl restart rabbitmq-server   #restart the service after changes
```

---

### 📊 Final Verification

12. Check RabbitMQ Service Status

```bash
systemctl status rabbitmq-server
```

✅ Status should show `active (running)`. Use `q` to quit the status view.

---

---

---

## 06. App Setup (Tomcat & Vprofile Deployment)

This document explains how to set up the Tomcat application server and deploy the `vprofile` application using Maven. This is part of a multi-tier architecture project and assumes the supporting services like MySQL, Memcached, and RabbitMQ are already configured.

---

### 📦 Prerequisites

Make sure the following packages are installed and services are already configured:

- Java JDK 17
- Git & wget
- Apache Tomcat 10
- Apache Maven
- Vagrant VM with hostname: `app01`
- Database service (MySQL), RabbitMQ, and Memcached properly set up

---

### 🔧 Step-by-Step Setup

1. Connect to Tomcat VM & Update the System

```bash
vagrant ssh app01

sudo -i

dnf update -y
```

2. Install Dependencies

```bash
dnf install epel-release -y

dnf install java-17-openjdk java-17-openjdk-devel -y

dnf install git wget unzip -y
```

3. Download and Install Tomcat

```bash
cd /tmp

wget https://archive.apache.org/dist/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10.1.26.tar.gz

tar -xvzf apache-tomcat-10.1.26.tar.gz

useradd --home-dir /usr/local/tomcat --shell /sbin/nologin tomcat

cp -r apache-tomcat-10.1.26/* /usr/local/tomcat

chown tomcat.tomcat -R /usr/local/tomcat/
```

4. Configure Tomcat as a System Service
   Create the file: `vi /etc/systemd/system/tomcat.service`

```bash
[Unit]
Description=Tomcat
After=network.target

[Service]
User=tomcat
Group=tomcat
WorkingDirectory=/usr/local/tomcat
Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/var/tomcat/%i/run/tomcat.pid
Environment=CATALINA_HOME=/usr/local/tomcat
Environment=CATALINE_BASE=/usr/local/tomcat
ExecStart=/usr/local/tomcat/bin/catalina.sh run
ExecStop=/usr/local/tomcat/bin/shutdown.sh
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target

```

Reload systemd and start Tomcat:

```bash
systemctl daemon-reload

systemctl start tomcat

systemctl enable tomcat

systemctl status tomcat
```

---

### 🛠 Build and Deploy vProfile Application

1. Download and Install Maven

```bash
cd /tmp

wget https://archive.apache.org/dist/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.zip

unzip apache-maven-3.9.9-bin.zip

cp -r apache-maven-3.9.9 /usr/local/maven3.9
```

Set fake memory for Maven (VM workaround):

```bash
export MAVEN_OPTS="-Xmx512m"
```

2. Clone the Source Code

```bash
git clone -b local https://github.com/hkhcoder/vprofile-project.git

cd vprofile-project
```

⚠ Make sure the file `src/main/resources/application.properties` contains the correct service connection details for:

- MySQL (hostname: `db01`, port: `3306`)
- Memcached (hostname: `mc01`, port: `11211`)
- RabbitMQ (hostname: `mq01`, port: `5672`, user: `test`, pass: `test`)

3. Build the Application

```bash
/usr/local/maven3.9/bin/mvn install
```

4. Deploy the Artifact

```bash
rm -rf /usr/local/tomcat/webapps/ROOT

cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war

chown tomcat: /usr/local/tomcat/webapps/ROOT.war
```

Tomcat will automatically extract the `.war` file and deploy it.

Restart Tomcat:

```bash
systemctl restart tomcat
```

---

### ✅ Validation

- Confirm that Tomcat is running:

```bash
systemctl status tomcat
```

---

---

---

## 07. Nginx Setup

This guide explains how to set up **Nginx** as a reverse proxy and load balancer for forwarding HTTP requests to a backend service (e.g., Tomcat). This setup is part of a Vagrant-based multi-VM architecture, where **web01** is the Nginx server.

### 🔧 Objective

To configure Nginx on `web01` Ubuntu machine to:

- Listen for HTTP requests on port `80`
- Forward requests to a backend server (`app01`) running on port `8080`

---

### 🧱 System Overview

- **Nginx Server (web01)**: Acts as a reverse proxy/load balancer.
- **Backend App Server (app01)**: Hosts the actual application on port `8080`.

---

### ⚙️ Step-by-Step Instructions

1. Connect to the Nginx Server

```bash
vagrant ssh web01
sudo -i   # Switch to root user
```

2. Update and Upgrade System Packages

```bash
apt update && apt upgrade -y
```

`apt update`: Checks for available updates.

`apt upgrade`: Applies the updates.

3. Install Nginx

```bash
apt install nginx -y
```

4. Configure Nginx

📁 File Location

Create the config file in:

```bash
vi /etc/nginx/sites-available/vproapp
```

📝 Sample Configuration

```bash
upstream vproapp {
    server app01:8080;
}

server {
    listen 80;

    location / {
        proxy_pass http://vproapp;
    }
}
```

5. Enable the Configuration

- Remove the default config:

```bash
rm /etc/nginx/sites-enabled/default
```

- Create a symbolic link:

```bash
ln -s /etc/nginx/sites-available/vproapp /etc/nginx/sites-enabled/vproapp
```

6. Restart Nginx

```bash
systemctl restart nginx
```

Check status:

```bash
systemctl status nginx
```

If there are errors, review the config file.

---

### 📂 Directory Reference

| Directory                     | Purpose                                       |
| ----------------------------- | --------------------------------------------- |
| `/etc/nginx/sites-available/` | Contains all virtual host configuration files |
| `/etc/nginx/sites-enabled/`   | Contains symlinks to enabled virtual hosts    |

---

### ✅ Final Notes

Once the setup is complete:

- Access your application via the IP of `web01` on port `80`.
- Nginx will automatically forward it to the backend service on port `8080`.

---

---

---

## 08. Validate

This document explains how to **validate the complete application stack** after setting up the Nginx reverse proxy and deploying the application. The goal is to ensure that all integrated services — Nginx, Tomcat (backend), PostgreSQL (database), Memcached (cache), and RabbitMQ (messaging) — are working correctly.

---

## ✅ Validation Goals

- Ensure **Nginx** is forwarding requests correctly.
- Verify that the **Tomcat** application is up and serving content.
- Confirm **Database** connection by logging in with stored credentials.
- Test **RabbitMQ** integration.
- Test **Memcached** for caching mechanism.

---

### 🌐 Step 1: Access the Application via Browser

Open your browser and visit:
`http://192.168.56.11`

- `192.168.56.11` is the IP address of the `web01` (Nginx) server.
- HTTP uses port `80` by default, so it can be omitted in the URL.
- This request is forwarded by Nginx to the Tomcat server on port `8080`.

If the application loads, it confirms:

- Nginx is configured correctly.
- Tomcat is running.
- Nginx is successfully forwarding traffic.

- Screenshot:

![alt text](<images/08.Access the Application.png>)

### 🔐 Step 2: Log In to the Application

Use the following credentials:

- **Username**: `admin_vp`
- **Password**: `admin_vp`

These credentials are stored in the PostgreSQL database, loaded via DB schema and dump.

✅ If login is successful, the database connection is verified.

- Screenshot:

![alt text](<images/09.Login to the Application.png>)

### 📦 Step 3: Validate RabbitMQ Integration

Click the **"RabbitMQ"** button (available only to the `admin_vp` user).

- If the message _"Queues and connection established successfully"_ appears, RabbitMQ is working correctly.
- If you see an error or no response, troubleshoot the RabbitMQ setup.

- Screenshot:

![alt text](<images/10. Validate RabbitMQ.png>)

---

### 💾 Step 4: Validate Memcached

Click the **"All Users"** button:

- This triggers a database call and lists all users.
- Select any user (e.g., `Aron`).
- First time: Data is fetched from the database and stored in Memcached.
- Second time: Data is fetched quickly from cache (no DB hit).

💡 This demonstrates how the cache improves response time for repeated data access.

- Screenshots:

![alt text](<images/11.1 Validate Memcached.png>)

![alt text](<images/11.2 Validate Memcached.png>)

---

### 🧹 Step 5: (Optional) Destroy the Setup

After validation, you can clean up the environment:

```bash
vagrant destroy --force
```
