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

# 🌐 Creating a Primary DNS Server

This guide explains how to configure a **Primary DNS Server** using BIND9. A primary server acts as the authoritative source of truth for a specific domain name (zone).

For this exercise, the text uses the domain `hashim.net` and the server IP `10.0.2.15`. You should replace these with your own domain and IP address when performing the setup.

---

## ⚙️ Step 1: Define the Zone in Configuration

First, we must tell BIND that it is responsible for the new domain. We do this by editing the local configuration file.

### 1. Edit the File

Open `/etc/bind/named.conf.local` and add the new zone definition.

```bash
zone "hashim.net" {
    type master;
    file "/etc/bind/db.hashim.net";
    allow-transfer { 10.0.2.15; };
    also-notify { 10.0.2.15; };
};

```

### 👨‍💻 Configuration Breakdown

* **`zone "hashim.net"`**: Defines the specific domain name this zone will serve.
* **`type master;`**: Designates this server as the **Primary** (Authoritative) source for this domain. Other types include `slave` (secondary), `forward`, or `hint`.
* **`file "...";`**: Specifies the file path where the actual DNS records (IPs, hostnames) will be stored.
* **`allow-transfer { ... };`**: Security setting. Lists the IP addresses of secondary DNS servers allowed to download (transfer) this zone data.
* **`also-notify { ... };`**: Lists the IPs of servers that should be automatically notified whenever this zone file changes.

---

## 📝 Step 2: Create the Zone File

Now we must create the actual file referenced in the step above (`db.hashim.net`). It is best practice to use an existing default file as a template to avoid syntax errors.

### 1. Copy the Template

We copy the standard `db.local` file to create our new zone file.

```bash
cd /etc/bind/
sudo cp db.local db.hashim.net

```

### 2. Verify Creation

Running `ls` should show your new file alongside the defaults:

```text
bind.keys          db.hashim.net   named.conf.default-zones   zones.rfc1918
db.0               db.empty           named.conf.local
db.127             db.local           named.conf.options
db.255             named.conf         rndc.key

```

---

## ✍️ Step 3: Configure DNS Records

Open your new file (`/etc/bind/db.hashim.net`) and populate it with your domain's data.

### 1. The Zone File Content

Here is the configuration for the `hashim.net` domain:

```dns
;
; BIND data file for hashim.net
;
$TTL    604800
@       IN      SOA     ns.hashim.net. admin.hashim.net. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns.hashim.net.
@       IN      A       10.0.2.15
ns      IN      A       10.0.2.15

```

### 🔍 Detailed Explanation of the Records

The file format follows a specific structure: **Hostname | Class | Type | Value**.

1. **Global Variables:**
* **`$TTL`**: Time To Live. Default time (in seconds) that other servers should cache these records.
* **`@` Symbol**: Represents the "Origin" or the root of the zone (in this case, `hashim.net`).


2. **SOA (Start of Authority) Record:**
* **`IN`**: Class "Internet".
* **`SOA`**: Indicates the authoritative server (`ns.hashim.net.`) and the admin email (`admin.hashim.net.`). *Note: The first dot in the email acts as the `@` symbol.*
* **Parameters inside parentheses:**
* **Serial:** A version number. You **must** increment this number every time you edit the file so secondary servers know to update.
* **Refresh/Retry/Expire:** Timers controlling how secondary servers sync data.




3. **Resource Records:**
* **`NS` (Name Server):** Defines which server answers DNS queries for this zone.
* **`A` (Address):** Maps a hostname to an IPv4 address.
* The line `@ IN A 10.0.2.15` maps the bare domain `hashim.net` to the IP.
* The line `ns IN A 10.0.2.15` maps `ns.hashim.net` to the IP.





---

## 🔄 Step 4: Apply and Test

After saving your files, you must instruct the BIND service to reload the configurations.

### 1. Reload Configuration

We use the **RNDC** (Remote Name Daemon Control) utility.

```bash
sudo rndc reload

```

### 2. Verify with `nslookup`

Test the server from a *different* computer on the network to ensure it is resolving correctly.

```bash
nslookup hashim.net 10.0.2.15

```

* **`hashim.net`**: The domain we are looking for.
* **`10.0.2.15`**: The specific DNS server we are querying (our new server).

**Expected Outcome:**
The command should return the IP address `10.0.2.15`, confirming that your primary DNS server is fully operational and correctly serving the zone file.

---

# 🔄 Setting Up a Secondary DNS Server

A **Secondary DNS Server** acts as a backup and load balancer. It holds a read-only copy of the zone data, which it automatically downloads from the Primary server.

**Infrastructure Setup:**

* **Primary Server:** `10.0.2.15` (Already configured)
* **Secondary Server:** `10.0.2.20` (New machine, also running Ubuntu Server 22.04.2 LTS)

---

## 🛠️ Phase 1: Configuring the Primary Server

Before touching the new machine, we must authorize the Primary server (`10.0.2.15`) to share its data with the new Secondary server (`10.0.2.20`).

### 1. Update Zone Transfer Settings

We need to tell the primary server that `10.0.2.20` is a trusted friend allowed to copy the zone.

**File:** `/etc/bind/named.conf.local`
Edit the zone definition to include the secondary IP in `allow-transfer` and `also-notify`.

```conf
zone "hashim.net" {
    type master;
    file "/etc/bind/db.hashim.net";
    allow-transfer { 10.0.2.15; 10.0.2.20; };
    also-notify { 10.0.2.15; 10.0.2.20; };
};

```

* **`allow-transfer`**: Permits the listed IPs to download the zone data.
* **`also-notify`**: Tells the primary server to actively send a "poke" (notification) to these IPs whenever the zone data changes.

### 2. Configure Global Options and ACLs

Next, we secure the server by defining exactly who is "trusted" and setting recursion rules.

**File:** `/etc/bind/named.conf.options`

**Step A: Create an ACL (Access Control List)**
Add this block *above* the `options { ... };` block. This defines a group called "trusted" containing both your servers.

```conf
acl "trusted" {
    10.0.2.15;
    10.0.2.20;
};

```

**Step B: Update the Options Block**
Inside the `options` block, add the following security directives:

```conf
options {
    directory "/var/cache/bind";
    
    recursion yes;
    allow-recursion { trusted; };
    listen-on { 10.0.2.15; };
    allow-transfer { none; };
    
    // ... (other existing options)
};

```

**Directive Explanation:**

* **`recursion yes;`**: Allows the server to perform recursive queries (looking up answers it doesn't know by asking other servers).
* **`allow-recursion { trusted; };`**: Restricts recursive queries only to the IPs listed in our "trusted" ACL. This prevents strangers from using your server to attack others.
* **`allow-transfer { none; };`**: Sets the *default* rule to deny zone transfers. This is overridden by the specific `allow-transfer` rule we set in the zone file in Step 1.

### 3. Restart Primary Service

Apply the changes by restarting BIND9.

```bash
sudo systemctl restart bind9.service

```

---

## 🖥️ Phase 2: Configuring the Secondary Server

Now move to the second machine (`10.0.2.20`). Ensure BIND9 is installed (`sudo apt install bind9`).

### 1. Configure Global Options

Just like the primary, we need to define our trusted network and listening interfaces.

**File:** `/etc/bind/named.conf.options`

Add the **ACL** and update the **Options** block:

```conf
acl "trusted" {
    10.0.2.15;
    10.0.2.20;
};

options {
    directory "/var/cache/bind";
    
    recursion yes;
    allow-recursion { trusted; };
    listen-on { 10.0.2.20; };   // Note: Secondary Server IP
    allow-transfer { none; };
    
    forwarders {
        8.8.8.8;
        8.8.4.4;
    };
};

```

### 2. Define the Secondary Zone

This is the most critical step. We tell the server that for `hashim.net`, it is **not** the master; it is a secondary (slave) and must get its data from the primary.

**File:** `/etc/bind/named.conf.local`

Add the following zone definition:

```conf
zone "hashim.net" {
    type secondary;
    file "/etc/bind/db.hashim.net";
    primaries { 10.0.2.15; };
};

```

### 🆚 Comparison: Primary vs. Secondary Config

Here is a side-by-side view of how the `named.conf.local` file differs between the two servers.

| Feature | Primary Server (`.113`) | Secondary Server (`.140`) |
| --- | --- | --- |
| **Zone Type** | `type master;` | `type secondary;` |
| **Source of Data** | Is the source. Defines transfers. | `primaries { 10.0.2.15; };` |
| **Transfer Rule** | `allow-transfer { ... };` | N/A (Does not send data) |

### 3. Finalize Secondary Setup

Allow the service through the firewall and restart it to initiate the first zone transfer.

```bash
sudo ufw allow Bind9 && sudo systemctl restart bind9

```

You now have a fully redundant DNS system with one Primary and one Secondary server!

---