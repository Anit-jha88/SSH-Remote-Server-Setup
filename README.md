# Remote Linux Server with Multiple SSH Keys

## 📌 Project Overview

This project demonstrates how to configure a remote Linux server to allow SSH connections using **two separate SSH key pairs**.

### Objectives

* Create a remote Linux server.
* Generate two SSH key pairs.
* Add both public keys to the remote server.
* Configure SSH key-based authentication.
* Verify SSH access using both private keys.

---

## 🔗 Project Page URL

**GitHub Repository:**

https://github.com/Anit-jha88/SSH-Remote-Server-Setup



---

## 🛠️ Requirements

Before starting, make sure you have:

* AWS / DigitalOcean account
* Remote Ubuntu Linux server
* SSH client
* Terminal / PowerShell / WSL
* Git (optional)

---

# 🚀 Setup Instructions

## Step 1: Create a Remote Linux Server

Create an Ubuntu server using AWS EC2, DigitalOcean, or another cloud provider.

Example configuration:

```text
OS: Ubuntu 24.04 LTS
SSH Port: 22
```

Allow inbound SSH traffic on port `22` from your IP address.

Example:

```text
Protocol: TCP
Port: 22
Source: Your IP Address
```

---

## Step 2: Connect to the Server

If using AWS EC2:

```bash
chmod 400 aws-server.pem
```

Connect:

```bash
ssh -i aws-server.pem ubuntu@SERVER_IP
```

Replace:

```text
SERVER_IP
```

with your server's public IP address.

Verify the connection:

```bash
whoami
hostname
```

---

# 🔑 Step 3: Create SSH Key Pair 1

On your local machine:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/devops_key1
```

This creates:

```text
~/.ssh/devops_key1
~/.ssh/devops_key1.pub
```

The private key is:

```text
devops_key1
```

The public key is:

```text
devops_key1.pub
```

---

# 🔐 Step 4: Create SSH Key Pair 2

Generate the second key pair:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/devops_key2
```

This creates:

```text
~/.ssh/devops_key2
~/.ssh/devops_key2.pub
```

---

# 📤 Step 5: Add SSH Key 1 to the Server

Copy the first public key:

```bash
ssh-copy-id -i ~/.ssh/devops_key1.pub ubuntu@SERVER_IP
```

If `ssh-copy-id` is unavailable, display the key:

```bash
cat ~/.ssh/devops_key1.pub
```

Copy the output.

On the remote server:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste the public key and save the file.

Set permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

# 📤 Step 6: Add SSH Key 2 to the Server

Display the second public key:

```bash
cat ~/.ssh/devops_key2.pub
```

On the server:

```bash
nano ~/.ssh/authorized_keys
```

Add the second public key on a **new line**.

The file should contain two keys:

```text
ssh-ed25519 AAAA... devops_key1
ssh-ed25519 BBBB... devops_key2
```

Set permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

# 🧪 Step 7: Test SSH Using Key 1

From your local machine:

```bash
ssh -i ~/.ssh/devops_key1 ubuntu@SERVER_IP
```

Verify:

```bash
whoami
```

Expected output:

```text
ubuntu
```

Exit:

```bash
exit
```

---

# 🧪 Step 8: Test SSH Using Key 2

Connect using the second private key:

```bash
ssh -i ~/.ssh/devops_key2 ubuntu@SERVER_IP
```

Verify:

```bash
whoami
```

Expected output:

```text
ubuntu
```

Exit:

```bash
exit
```

---

# 🔍 Step 9: Verify Both Keys

On the remote server:

```bash
cat ~/.ssh/authorized_keys
```

You should see both public keys:

```text
ssh-ed25519 AAAA... devops_key1
ssh-ed25519 BBBB... devops_key2
```

Test both:

```bash
ssh -i ~/.ssh/devops_key1 ubuntu@SERVER_IP
```

and:

```bash
ssh -i ~/.ssh/devops_key2 ubuntu@SERVER_IP
```

Both connections should succeed.

---

# 🔒 Security

Never upload private SSH keys to GitHub.

Do **not** commit:

```text
*.pem
devops_key1
devops_key2
```

Recommended `.gitignore`:

```gitignore
*.pem
*_key
id_rsa
id_rsa.pub
```

Set appropriate permissions:

```bash
chmod 600 ~/.ssh/devops_key1
chmod 600 ~/.ssh/devops_key2
chmod 700 ~/.ssh
```

---

# 📁 Project Structure

```text
remote-linux-ssh/
│
├── README.md
└── .gitignore
```

Private keys are intentionally **not included** in the repository.

---

# ✅ Expected Result

After completing the setup, the same remote Linux server should accept SSH connections using either key:

```text
Local Machine
     │
     ├── devops_key1 ────────┐
     │                       │
     └── devops_key2 ────────┤
                             ▼
                     Remote Ubuntu Server
                             │
                      authorized_keys
                       ┌─────┴─────┐
                       │           │
                    Public Key 1  Public Key 2
```

Both commands should successfully connect:

```bash
ssh -i ~/.ssh/devops_key1 ubuntu@SERVER_IP
```

```bash
ssh -i ~/.ssh/devops_key2 ubuntu@SERVER_IP
```

---

# 🎯 Learning Outcomes

This project demonstrates practical knowledge of:

* Linux server administration
* SSH
* Public/private key authentication
* SSH key management
* Linux file permissions
* Cloud server provisioning
* Remote server access
* Basic server security

---

## 👨‍💻 Author

**Anit Kumar Jha**

DevOps / Cloud Engineer

**Skills:** AWS | Linux | Docker | Terraform | Jenkins | Kubernetes | Git | CI/CD
