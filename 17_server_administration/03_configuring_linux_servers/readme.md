# 🖥️ Introduction to Linux Servers and Services

This section clarifies the fundamental differences between a **Linux Server** and a **Linux Workstation**, and introduces the core concept of Linux services, also known as **Daemons**.

---

## 🆚 Linux Server vs. Linux Workstation

While both systems rely on the Linux kernel and can run on powerful hardware, their primary functions and usage patterns differ significantly.

### 1. The Linux Server ☁️

A **Server** is a system designed to "serve" content and resources over a network to other computers, known as **clients**.

* **Core Function:** It provides its hardware (CPU, RAM, Storage) and software resources to multiple users or devices simultaneously.
* **Hardware:** Usually consists of very powerful systems with extensive resources to handle requests from many clients at once.
* **Examples of Use:**
* **🌐 Web Server:** Accessed every time you type a URL into a browser to view a website.
* **🖨️ Print Server:** Accessed when you send a document to a shared printer at your workplace.
* **📧 Mail Server:** Accessed when you read or send an email.



### 2. The Linux Workstation 💻

A **Workstation** is a high-performance computer designed for **personal use**.

* **Core Function:** Used for intensive individual tasks (like video editing, coding, or data analysis).
* **Access:** It is generally **not** configured for external clients to access it over a network. It functions similarly to a regular desktop or laptop.

> **Note:** While the content in the upcoming chapters is optimized for server environments, the knowledge is often applicable to high-end workstations as well.

---

## ⚙️ Understanding Linux Services (Daemons)

When IT professionals talk about "setting up a Web Server" or an "Email Server," they are often referring to the **software**, not the physical metal box in a data center.

### What is a "Service"?

A service is a specific piece of software running on a computer that provides data to a client. The physical machine is the hardware; the "Server" application is the software.

### 👻 Daemons: The Ghosts in the Machine

In the Linux world, these background services are technically called **Daemons**.

* **Definition:** Programs that run in the background, waiting for requests or performing scheduled tasks, without direct user intervention.
* **Connection to Hardware:** A physical server might run multiple daemons (e.g., a web daemon *and* a mail daemon) simultaneously.

### 🚀 The `init` Process and `systemd`

To manage these daemons, Linux uses a master process that starts when the computer boots up.

* **The Mother of All Processes:** The **`init`** process is the very first process launched during boot. It is responsible for starting all other processes and daemons.
* **`systemd`:** This is the modern, standard version of the `init` process used by most major Linux distributions today, including:
* **Ubuntu**
* **CentOS**
* **Fedora**
* **openSUSE**



All service management (starting, stopping, and restarting daemons) is handled by `systemd`.

---

# 🕵️‍♂️ Analyzing the Linux Init Process and Boot Performance

In this section, we explore how to identify the "mother of all processes" (the init process) and how to analyze the performance of your system's boot sequence using `systemd` tools.

---

## 1. Identifying the Init Process

The **Init Process** is the very first process started by the kernel during booting. It has **Process ID (PID) 1**. On modern Linux systems (like Ubuntu 22.04), this role is handled by **systemd**, though it may appear under different names for compatibility.

### 🔍 Using the `ps` Command

We use the `ps` (process status) command to view currently running processes.

* **`-e`**: Select all processes.
* **`-f`**: Do a full-format listing (shows detailed info like UID, PPID, etc.).

**Command:**

```bash
ps -ef | less

```

**Output:**

```text
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  2 19:13 ?        00:00:09 /sbin/init splash

```

### 👨‍💻 Output Breakdown

* **UID (root):** The user running the process.
* **PID (1):** This confirms it is the first process.
* **CMD (`/sbin/init`):** Even though it says `init`, on modern Ubuntu systems, this is actually a symbolic link to **systemd**.
* *Tip:* You can verify this by typing `man init` in your terminal. The manual page that opens will likely be for `systemd-init`, confirming that systemd is in charge.



---

## 2. Analyzing System Boot Performance

One of the main advantages of **systemd** over older init systems is that it starts services in **parallel** (at the same time), rather than one by one. This makes booting much faster.

To measure exactly how fast your system boots and identify what might be slowing it down, we use the `systemd-analyze` tool.

### ⏱️ Check Total Boot Time

This command gives you a high-level summary of how long the kernel and userspace took to load.

**Command:**

```bash
systemd-analyze

```

**Output:**

```text
Startup finished in 20.457s (kernel) + 1min 2.083s (userspace) = 1min 22.541s 
graphical.target reached after 1min 2.082s in userspace.

```

* **Kernel Time:** Time taken by the Linux kernel to initialize hardware.
* **Userspace Time:** Time taken to start system services (Bluetooth, Networking, GUI, etc.).

---

### 🐢 Identifying Slow Services (The "Blame" Game)

If your boot time seems slow, you can use the `blame` command to list all running units ordered by how long they took to initialize. This helps you find the "bottleneck."

**Command:**

```bash
systemd-analyze blame

```

**Output:**

```text
28.052s plymouth-quit-wait.service
12.930s dev-sda2.device
11.401s snapd.seeded.service
11.354s snapd.service
 9.325s dev-loop16.device
 9.280s dev-loop17.device
 9.083s dev-loop15.device
 8.931s snap-firefox-7477.mount
 8.825s dev-loop12.device
 8.742s dev-loop13.device
 8.727s dev-loop8.device
 8.706s dev-loop9.device
 8.668s dev-loop14.device
 8.648s dev-loop11.device
 8.617s dev-loop10.device
 8.523s vboxadd.service
 7.409s NetworkManager.service
 7.019s apparmor.service
 5.213s accounts-daemon.service
 5.097s udisks2.service
lines 1-20

```

### 🧐 Analysis of the Output

* **Services (`.service`):** Background programs. For example, `NetworkManager.service` took 7.4s to start network connections.
* **Devices (`.device`):** Hardware initialization. `dev-sda2.device` took 12.93s, which is the time waiting for the hard drive partition to be ready.
* **Mounts (`.mount`):** Filesystem mount points. `snap-firefox...mount` took 8.9s.

**Note:** Just because a service takes a long time doesn't always mean it's "slowing down" the boot. Because systemd runs things in parallel, a slow service might be starting in the background while other things are happening simultaneously.

---

# 🔐 Setting Up and Securing SSH

Secure Shell (SSH) is the standard protocol for securely accessing remote servers. This guide details how to install, configure, and secure OpenSSH on an **Ubuntu Server 22.04.2 LTS** system. We will walk through the entire process—from installation to key-based authentication—using the username **hashim**.

---

## 📦 Step 1: Installing OpenSSH

First, we must ensure the OpenSSH server software is installed on your machine.

### The Command

```bash
sudo apt install openssh-server

```

* **`sudo`**: Runs the command with administrative privileges.
* **`apt install`**: Uses the package manager to download and install software.
* **`openssh-server`**: The specific package required to accept incoming SSH connections.

### 🕵️ Verifying Installation

It is possible that SSH is already installed. If the command says "openssh-server is already the newest version," you can proceed to the next step.

---

## ⚡ Step 2: Enabling and Starting the Service

Once installed, we need to make sure the SSH service is running now and starts automatically whenever the server turns on.

### The Command

```bash
sudo systemctl enable ssh && sudo systemctl start ssh

```

* **`systemctl enable ssh`**: Configures the system to launch SSH automatically at boot time.
* **`&&`**: Runs the second command only if the first one succeeds.
* **`systemctl start ssh`**: Immediately starts the SSH service so you can use it right now.

---

## 🔑 Step 3: Setting Up Key-Based Authentication

Before we secure the server settings, we must set up **SSH Keys**. This allows you to log in using a cryptographic key file instead of a password, which is much more secure.

**⚠️ Important:** Perform these steps on your **local computer** (the one you are connecting *from*), not the server.

### 1. Generate a Key Pair

Run the following command to create a new public/private key pair.

```bash
ssh-keygen -t rsa -b 4096

```

* **`ssh-keygen`**: The tool used to create the keys.
* **`-t rsa`**: Specifies the algorithm type (RSA is a standard, robust choice).
* **`-b 4096`**: Specifies the number of bits in the key (4096 is very secure).

**Output:**
The system will ask you where to save the file (press Enter for default) and for a passphrase (optional, for extra security).

### 2. Copy the Public Key to the Server

Now, we transfer your public key to the server so it recognizes you. Replace `10.0.2.15` with your server's actual IP address.

```bash
ssh-copy-id hashim@10.0.2.15

```

* **`ssh-copy-id`**: A utility that automatically copies your public key to the remote server's `authorized_keys` file.
* **`hashim`**: The username on the remote server.
* **`10.0.2.15`**: The IP address of the remote server.

You will be asked for **hashim**'s password one last time to authorize this copy.

---

## ⚙️ Step 4: Configuring and Securing SSH

Now that keys are set up, we will edit the configuration file to lock down the server.

**⚠️ Warning:** Do not disable password authentication (Step 4.3) until you have successfully tested your SSH Key login. If you do it too early, you might lock yourself out.

### 1. Open the Configuration File

The main configuration file is located at `/etc/ssh/sshd_config`.

```bash
sudo nano /etc/ssh/sshd_config

```

### 2. Disable Root Login

For security, you should never log in directly as the `root` user. Find the line containing `PermitRootLogin` and change it to:

```ssh
PermitRootLogin no

```

* This ensures that even if an attacker guesses the root password, they cannot log in via SSH.

### 3. Enable Public Key Authentication

Ensure the server is looking for SSH keys. Find or uncomment the line:

```ssh
PubkeyAuthentication yes

```

* This explicitly allows the server to verify users using the keys we generated earlier.

### 4. Disable Password Authentication

**Only perform this step if your SSH keys are working.** This prevents anyone from logging in using a password, protecting you from brute-force password guessing attacks.

Find and modify these lines:

```ssh
PasswordAuthentication no
PermitEmptyPasswords no

```

* **`PasswordAuthentication no`**: Completely turns off password logins.
* **`PermitEmptyPasswords no`**: Ensures accounts with blank passwords cannot log in.

---

## 🔄 Step 5: Apply Changes and Connect

After saving the configuration file (Press `Ctrl+O`, `Enter`, then `Ctrl+X` in Nano), you must restart the SSH service for the changes to take effect.

### 1. Restart SSH Service

```bash
sudo systemctl restart ssh

```

### 2. Test the Connection

Now, try to connect from your local machine.

```bash
ssh hashim@10.0.2.15

```

* **`ssh`**: The client command to connect.
* **`hashim`**: Your username.
* **`10.0.2.15`**: The target server IP.

If configured correctly, you should be logged in immediately without being asked for a user password (though you may be asked for your key passphrase if you set one).


## 📚 Further Resources

OpenSSH is a powerful tool with many more options. For advanced configurations, you can consult:

* [Ubuntu Server OpenSSH Documentation](https://ubuntu.com/server/docs/service-openssh)
* [OpenSSH Manual Pages](https://www.openssh.com/manual.html)

---


# 🌐 Setting Up a DNS Server with BIND9

This guide details how to configure a DNS server using **Berkeley Internet Name Domain 9 (BIND 9)** on **Ubuntu Server 22.04.2 LTS**. BIND 9 is one of the most widely used DNS services.

The goal is to set up a **caching name server** and a **primary name server** to manage hostnames and private IP addresses on a local network.

### 🧠 Key Concepts

* **Caching Name Server:** This type of server handles recursive requests from clients, remembering (caching) the answers to speed up future requests.
* **Primary/Secondary (Authoritative) Servers:** These hold the actual DNS records for specific zones. The primary server is the source of truth, while the secondary gets its data from the primary.

---

## 🛠️ Phase 1: Installation and Initial Testing

### 1. Install BIND9 Packages

First, install the necessary software packages on your Ubuntu server.

```bash
sudo apt install bind9 bind9utils bind9-doc

```

* **`bind9`**: The main DNS server software.
* **`bind9utils`**: Utility tools for testing and management.
* **`bind9-doc`**: Documentation.

### 2. Verify Installation

After installation, test if BIND is running correctly by querying itself (localhost) using the `nslookup` command.

```bash
nslookup google.com 127.0.0.1

```

**Expected Output:**
The output should show a server address of `127.0.0.1` and a non-authoritative answer for `google.com` (showing IP addresses), confirming the service is active.

---

## ⚡ Phase 2: Configuring a Caching DNS Server

A caching server is the default behavior for BIND9, so the setup is straightforward.

### ⚠️ Pre-requisite: Backup Configuration Files

Before making changes, it is highly advised to back up these critical configuration files:

* `/etc/bind/named.conf`
* `/etc/bind/named.conf.options`
* `/etc/hosts`
* `/etc/resolv.conf`

### 1. Configure the Firewall

Allow DNS traffic through the firewall.

```bash
sudo ufw allow Bind9

```

### 2. Edit the Options File

Open the configuration file `/etc/bind/named.conf.options` in a text editor. We will modify the `options` directive (everything between the curly brackets `{ }`).

**Step A: Disable IPv6 (Optional)**
If you are only using IPv4, comment out the IPv6 listening option by adding `//` at the start of the line:

```bash
// listen-on-v6 { any; };

```

**Step B: Configure Forwarders**
Define where the server should look if it doesn't know an answer (forwarding queries). We will use Google's Public DNS, but you can use your ISP's DNS.
Add/Edit the `forwarders` block:

```bash
forwarders {
    8.8.8.8;
    8.8.4.4;
};

```

**Step C: Define Allowed Queries**
Restrict who can query your server for security. Add the `allow-query` block to include your local network (e.g., `10.0.2.0/24`) and localhost, to find ip address use this command `ip a` on a terminal.

```bash
allow-query {
    localhost;
    10.0.2.0/24;
};

```

**Step D: Define Listening Interfaces**
Specify which network interfaces the server should listen on for IPv4 queries.

```bash
listen-on {
    10.0.2.15/24;
};

```

### 3. Verify and Restart

1. **Check Syntax:** Run `named-checkconf`. If there is no output, your syntax is correct.
2. **Restart Service:** Restart BIND9 to apply changes.

### 4. Final Testing

Test your new DNS server from **another computer** on the same network. Replace `10.0.2.15` with your server's actual IP address.

```bash
nslookup google.com 10.0.2.15

```

**Expected Output:**
You should see your server's IP (`10.0.2.15`) returning the answer. This confirms you have a working caching DNS server on your private network.


---