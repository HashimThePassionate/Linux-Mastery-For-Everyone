# 🚀 SSH Connectivity Guide: Windows Host to Ubuntu Server (VirtualBox)

This documentation provides a comprehensive, step-by-step walkthrough for establishing a secure **Secure Shell (SSH)** connection between a Windows host machine (using PowerShell) and an Ubuntu Linux server running on Oracle VirtualBox. It covers network configuration, server preparation, and the implementation of **Password-less Key-Based Authentication** using the modern `ED25519` standard.

---

## 📋 Table of Contents

1. [Network Configuration (Bridged Adapter)](https://www.google.com/search?q=%231-network-configuration-bridged-adapter)
2. [Server Preparation (Ubuntu)](https://www.google.com/search?q=%232-server-preparation-ubuntu)
3. [Establishing the Initial Connection](https://www.google.com/search?q=%233-establishing-the-initial-connection)
4. [Setting Up SSH Key Authentication (Password-less Login)](https://www.google.com/search?q=%234-setting-up-ssh-key-authentication-password-less-login)
5. [Final Verification](https://www.google.com/search?q=%235-final-verification)

---

## 1. Network Configuration (Bridged Adapter)

By default, VirtualBox uses **NAT**, which hides the Virtual Machine (VM) behind the host. To allow Windows to communicate directly with Ubuntu, we must switch to a **Bridged Adapter**. This makes the VM appear as a separate device on your physical network (router).

### ⚙️ Configuration Steps

1. **Power Off** the Ubuntu Virtual Machine.
2. Open **VirtualBox Manager**.
3. Right-click the Ubuntu VM and select **Settings**.
4. Navigate to the **Network** tab.
5. Change **"Attached to"** from *NAT* to **Bridged Adapter**.
6. Ensure the **Name** matches your active internet adapter (WiFi or Ethernet).
7. Click **OK** and **Start** the VM.

---

## 2. Server Preparation (Ubuntu)

Before connecting, the Ubuntu system must have the SSH server software installed and running.

### 📥 Step 2.1: Install OpenSSH Server

Open the terminal inside your Ubuntu VM and run the following commands:

```bash
sudo apt update && sudo apt install openssh-server -y

```

**Code Explanation:**

* `sudo apt update`: Refreshes the package lists to ensure you download the latest version.
* `&&`: Runs the second command only if the update is successful.
* `install openssh-server`: Installs the daemon (`sshd`) required to accept incoming connections.
* `-y`: Automatically answers "yes" to installation prompts.

### 🔍 Step 2.2: Verify Service Status

Ensure the service is active:

```bash
sudo systemctl status ssh

```

**Output Check:** Look for **`Active: active (running)`** in green text.

### 🆔 Step 2.3: Retrieve IP Address

Find the unique IP address assigned to your Ubuntu server by the router:

```bash
ip a

```

**Code Explanation:**

* `ip a`: Displays all network interfaces. Look for the interface named `enp0s3` (or similar).
* Identify the number following `inet`. It will look like `192.168.1.x` (e.g., `192.168.1.13`). **Note this IP down.**

---

## 3. Establishing the Initial Connection

Now, we test connectivity from the Windows Host using a password.

### 💻 Connect via PowerShell

Open **Windows PowerShell** and run:

```powershell
ssh hashim@192.168.1.13

```

**Code Explanation:**

* `ssh`: Calls the Secure Shell client.
* `hashim`: The username on the Linux server.
* `@192.168.1.13`: The target IP address of the Linux server.

> **Note:** On the first connection, you will see a fingerprint warning (`The authenticity of host...`). Type `yes` and press **Enter**. Then, type your Ubuntu user password.

---

## 4. Setting Up SSH Key Authentication (Password-less Login)

To enhance security and convenience, we will configure the system to use cryptographic keys instead of passwords.

### 🔑 Step 4.1: Generate Keys on Windows

In **Windows PowerShell** (ensure you are disconnected from the server), generate a key pair:

```powershell
ssh-keygen

```

**Code Explanation:**

* Generates a private and public key pair.
* Press **Enter** for all prompts (file location and passphrase) to accept defaults.
* **Modern Note:** Windows may generate an `ED25519` key by default (e.g., `id_ed25519`), which is newer and faster than the old `RSA` standard.

### 📋 Step 4.2: Copy the Public Key

We need to copy the content of the public key to the clipboard. Run this specific command in PowerShell:

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | clip

```

**Code Explanation:**

* `type`: Reads the content of the file.
* `$env:USERPROFILE`: Automatically finds your home folder (e.g., `C:\Users\Hashim`).
* `.ssh\id_ed25519.pub`: The path to your **Public Key** (the lock).
* `| clip`: Pipes the output directly to your Windows Clipboard (Copy).

### 🛠️ Step 4.3: Configure the Server (Ubuntu)

Log back into the Ubuntu server (`ssh hashim@192.168.1.13`) and perform the following setup:

```bash
# 1. Create the SSH directory if it doesn't exist
mkdir -p ~/.ssh

# 2. Open the authorized_keys file
nano ~/.ssh/authorized_keys

```

**Action:**
Inside the Nano editor, **Paste** the key you copied from Windows (Right-click or Ctrl+Shift+V).

* Press `Ctrl + O`, then `Enter` to **Save**.
* Press `Ctrl + X` to **Exit**.

### 🛡️ Step 4.4: Set Security Permissions

This step is critical. SSH will reject keys if the file permissions are too open.

```bash
chmod 600 ~/.ssh/authorized_keys

```

**Code Explanation:**

* `chmod 600`: Sets permissions so that **only the owner** (you) can read and write to this file. No one else can access it.

---

## 5. Final Verification

To confirm the setup is successful:

1. **Exit** the current session:
```bash
exit

```


2. **Reconnect** from Windows PowerShell:
```powershell
ssh hashim@192.168.1.13

```



**🎉 Success Criteria:** The system should log you in immediately **without** requesting a password.

---

Would you like me to guide you on how to give this server a "Static IP" so that the address `192.168.1.13` never changes in the future?