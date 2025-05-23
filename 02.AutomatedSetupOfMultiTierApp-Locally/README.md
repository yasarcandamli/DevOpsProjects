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

This setup builds on the same architecture from the previous project, with a key difference:

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

## 02. Code

In this section, we take a detailed look at the **automated provisioning code** used to bring up the full stack environment using `Vagrant` and `Bash` scripts.

This setup is a direct continuation of the previous project and is designed to run the entire stack with a single command:

```bash
vagrant up
```

---

### 📁 Folder Structure

Inside the repository, navigate to:

```bash
vagrant/
└── Automated-provisioning_MACOSM1/
└── Automated-provisioning_WinMacIntel/
```

- 🪟 Use `windows-intel/` if you are on Windows or macOS with an Intel chip.

- 🍏 Use `macos-arm/` if you are using a Mac with M1 or M2 chips.

Choose the correct folder for your OS to avoid compatibility issues.

---

### ⚙️ Vagrantfile

The `Vagrantfile` used here is almost identical to the one from manual provisioning, with one major difference:

✅ Each VM now has a `shell script` assigned that will be executed during provisioning.

Example:

```bash
config.vm.provision "shell", path: "mysql/mysql.sh"
```

⏱ Note: Shell scripts are in the `Automated-provisioning_WinMacIntel` folder.

Each VM (e.g., db01, web01, etc.) uses its own script to install and configure the required services.

---

### 🧠 Bash Script Basics

All scripts follow this pattern:

```bash
#!/bin/bash       # Start bash shell
VARIABLE=value    # Define variables
sudo apt update   # Install packages
sudo systemctl ... # Start/enable services
```

Special mentions:

- Database password is defined as a variable at the top.
- SQL queries are executed directly from shell using:

```bash
mysql -u root -p"$PASSWORD" -e "SQL_QUERY"
```

- Tomcat systemd service file is created using `cat <<EOF ... EOF` block inside the script.

---

### 🎯 Takeaway

Even without Bash scripting experience, you can:

- Read and understand each shell script.
- Know which part is provisioning which service.
- Identify which script belongs to which VM.

This modular structure makes it easy to debug, extend, or customize your provisioning logic in the future.

---

### 🧰 Next Step

You're now ready to provision the entire environment automatically:

```bash
vagrant up
```

This will:

- Spin up all VMs
- Run all the shell scripts
- Deploy and configure your application stack

Once provisioning is complete, proceed to the next section to test and validate the setup.

---

---

---

## 03. Execution

In this section, we execute our fully automated provisioning setup using a single command. You’ll see how each VM is created, configured, and verified without manual intervention.

---

## ✅ Objective

Provision the entire stack (MySQL, Memcache, RabbitMQ, Tomcat, Nginx) automatically using:

```bash
vagrant up
```

---

### 📁 Steps to Follow

1. Open Git Bash

Open Git Bash (or any terminal) and navigate to:

```bash
cd source-code/vagrant/automated-provisioning
```

💡 Choose the correct directory for your OS (Windows-Intel or Mac ARM64 as explained in the previous lesson).

2. Start the Provisioning Process

Run:

```bash
vagrant up
```

This command will:

- Start all VMs (db01, mc01, rmq01, app01, web01)
- Execute the relevant provisioning shell scripts
- Set up and configure all services

---

### ⚙️ Behind the Scenes

Here’s what happens VM by VM:

1. db01 (MySQL):

- Installs and configures MariaDB
- Sets the root password
- Loads the initial schema and data

2. mc01 (Memcache):

- Installs and starts Memcached service

3. rmq (RabbitMQ):

- Installs dependencies including Erlang and socat
- Starts and enables RabbitMQ

4. app01 (Tomcat):

- Installs Java and Tomcat
- Builds the application with Maven
- Deploys WAR to Tomcat

5. web01 (Nginx):

- Installs and configures Nginx
- Acts as a reverse proxy to Tomcat

⏱ Note: Provisioning may take 15–30 minutes depending on your internet speed. Be patient!

---

### 🌐 Validation Steps

After provisioning is complete:

1. Open your browser
2. Access the app via:

```bash
http://web01
```

You can also use the static IP defined in the `Vagrantfile`.

---

### 🧪 Application Test

- Login:

  - Username: admin_vp

  - Password: admin_vp

- Validate DB, Memcache, and RabbitMQ functionality through the UI.

You should see confirmations like:

- `DB Validated`

- `RMQ Validated`

- `Cache Inserted`

---

### 📴 Power Off the Stack

To stop the environment without destroying it:

```bash
vagrant halt
```

- This will power off all VMs but retain their state.
- Check status with:

```bash
vagrant status
```

---

### 🔁 Repeatability

Once provisioned:

- You can bring the stack back with:

```bash
vagrant up
```

- Scripts won’t run again unless the VM is destroyed.
- This makes your setup repeatable, reliable, and fully automated.

### 🧱 Infrastructure as Code (IaC)

This setup is a perfect example of **Infrastructure as Code**:

- Everything is version-controlled
- Provisioning is consistent every time
- No manual setup needed
- Easy to reproduce and share with team

---

### 🏁 Conclusion

With just a single command, we have:

- Set up a multi-tier architecture
- Installed and configured all services
- Validated application functionality
