# Remote Linux Server SSH Access with Multiple SSH Keys

## 📌 Project Overview

This project demonstrates how to set up a remote Linux server and configure SSH access using **two separate SSH key pairs**.

The objective is to:

* Provision a remote Linux server.
* Generate two independent SSH key pairs.
* Add both public keys to the remote server.
* Configure SSH authentication.
* Verify that the server can be accessed using either private key.

This is a basic but important DevOps/Linux administration exercise for understanding **SSH authentication and key-based access**.

---

## 🏗️ Architecture

```text
                    Internet
                       |
                       |
                SSH - Port 22
                       |
                       ▼
             ┌──────────────────┐
             │ Remote Linux     │
             │ Server           │
             │                  │
             │ ~/.ssh/          │
             │ authorized_keys  │
             │                  │
             │ ┌──────────────┐ │
             │ │ Public Key 1 │ │
             │ └──────────────┘ │
             │ ┌──────────────┐ │
             │ │ Public Key 2 │ │
             │ └──────────────┘ │
             └──────────────────┘
                    ▲      ▲
                    │      │
                 Key 1    Key 2
                    │      │
          ┌─────────┴──────┴─────────┐
          │       Local Machine      │
          │                          │
          │ devops_key1              │
          │ devops_key2              │
          └──────────────────────────┘
```

---

## 🛠️ Technologies Used

* Linux / Ubuntu
* AWS EC2 / DigitalOcean
* SSH
* OpenSSH
* Ed25519 SSH Keys
* Git & GitHub

---

# 🚀 Step 1: Provision the Linux Server

A remote Ubuntu server was provisioned using a cloud provider.

Example:

**Cloud Provider:** AWS EC2

**Operating System:** Ubuntu 24.04 LTS

**SSH Port:** 22

The server's security group/firewall was configured to allow SSH access.

Example inbound rule:

```text
Type: SSH
Protocol: TCP
Port: 22
Source: My IP
```

> For production environments, avoid opening SSH (`22`) to `0.0.0.0/0` unless there is a specific security requirement.

---

# 🔑 Step 2: Generate the First SSH Key Pair

On the local machine:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/devops_key1
```

This creates:

```text
~/.ssh/devops_key1
~/.ssh/devops_key1.pub
```

Where:

```text
devops_key1      → Private Key
devops_key1.pub  → Public Key
```

The private key must be kept secure and should never be committed to GitHub.

---

# 🔐 Step 3: Generate the Second SSH Key Pair

Generate another independent key pair:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/devops_key2
```

This creates:

```text
~/.ssh/devops_key2
~/.ssh/devops_key2.pub
```

The local machine now contains two SSH key pairs:

```text
~/.ssh/
│
├── devops_key1
├── devops_key1.pub
├── devops_key2
└── devops_key2.pub
```

---

# 📤 Step 4: Copy the First Public Key to the Server

The first public key can be copied using:

```bash
ssh-copy-id -i ~/.ssh/devops_key1.pub ubuntu@SERVER_IP
```

Alternatively, display the public key:

```bash
cat ~/.ssh/devops_key1.pub
```

Copy the output and add it to the server's:

```bash
~/.ssh/authorized_keys
```

---

# 📤 Step 5: Add the Second Public Key

Display the second public key:

```bash
cat ~/.ssh/devops_key2.pub
```

On the remote server:

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
```

Add the second public key below the first key.

The final file should look similar to:

```text
ssh-ed25519 AAAA... devops_key1
ssh-ed25519 BBBB... devops_key2
```

Set the correct permissions:

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

# ⚙️ Step 6: Verify SSH Configuration

Check the SSH server configuration:

```bash
sudo sshd -T | grep pubkeyauthentication
```

Expected output:

```text
pubkeyauthentication yes
```

SSH public-key authentication must be enabled.

---

# 🧪 Step 7: Test SSH Using Key 1

From the local machine:

```bash
ssh -i ~/.ssh/devops_key1 ubuntu@SERVER_IP
```

Verify the connection:

```bash
whoami
```

Expected:

```text
ubuntu
```

Exit the server:

```bash
exit
```

---

# 🧪 Step 8: Test SSH Using Key 2

Now test the second key:

```bash
ssh -i ~/.ssh/devops_key2 ubuntu@SERVER_IP
```

Verify:

```bash
whoami
```

Expected:

```text
ubuntu
```

Exit:

```bash
exit
```

Both keys should now successfully authenticate against the same server.

---

# 🔍 Verification

Check the authorized keys on the server:

```bash
cat ~/.ssh/authorized_keys
```

Expected:

```text
ssh-ed25519 AAAA... devops_key1
ssh-ed25519 BBBB... devops_key2
```

Test both connections:

```bash
ssh -i ~/.ssh/devops_key1 ubuntu@SERVER_IP
```

and:

```bash
ssh -i ~/.ssh/devops_key2 ubuntu@SERVER_IP
```

Both connections should succeed.

---

# 🔒 Security Best Practices

### 1. Never share private keys

Do not share:

```text
devops_key1
devops_key2
```

Only public keys should be placed on the server:

```text
devops_key1.pub
devops_key2.pub
```

### 2. Never commit private keys to Git

Add the following to `.gitignore`:

```gitignore
*.pem
*_key
*_key.pub
id_rsa
id_rsa.pub
```

### 3. Use correct SSH permissions

Private keys:

```bash
chmod 600 ~/.ssh/devops_key1
chmod 600 ~/.ssh/devops_key2
```

SSH directory:

```bash
chmod 700 ~/.ssh
```

Server authorized keys:

```bash
chmod 600 ~/.ssh/authorized_keys
```

### 4. Restrict SSH access

Instead of:

```text
0.0.0.0/0
```

prefer allowing SSH only from trusted IP addresses whenever practical.

---

# 📁 Project Structure

```text
remote-linux-ssh/
│
├── README.md
└── .gitignore
```

**Important:** SSH private keys should **not** be included in this repository.

---

# 🎯 What I Learned

Through this project, I practiced:

* Provisioning a remote Linux server.
* Connecting to Linux using SSH.
* Generating Ed25519 SSH key pairs.
* Understanding public/private key authentication.
* Managing `~/.ssh/authorized_keys`.
* Configuring SSH access for multiple keys.
* Testing SSH authentication.
* Applying basic Linux file permissions.
* Following SSH security best practices.

---

# ✅ Project Completion Checklist

* [x] Remote Linux server created
* [x] SSH access enabled
* [x] First SSH key pair created
* [x] Second SSH key pair created
* [x] First public key added to server
* [x] Second public key added to server
* [x] SSH connection tested with Key 1
* [x] SSH connection tested with Key 2
* [x] SSH key permissions configured
* [x] Private keys kept secure

---

## 👨‍💻 Author

**Anit Kumar Jha**

DevOps / Cloud Engineer

Skills: AWS | Linux | Docker | Terraform | Jenkins | Kubernetes | Git | CI/CD

