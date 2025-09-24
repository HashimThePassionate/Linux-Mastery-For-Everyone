# 🌐 **Working with Network Services in Linux**

In this section, we’ll enumerate some of the most common **network services** running on Linux.

> ⚠️ **Note**: Not all services mentioned here are installed or enabled by default on your Linux distribution.

📖 For more in-depth details:

* **Section** → *Securing Linux*
* **Section** → *Disaster Recovery, Diagnostics, and Troubleshooting*

Those Sections will explain **installation** and **configuration** of these services.
Here, our focus is on:

✅ What these network services are
✅ How they work
✅ Which networking protocols they use for communication

---

## 🖥️ What is a Network Service?

A **network service** is typically a **system process** that implements **application layer (OSI Layer 7)** functionality for **data communication**.

* These services allow devices, applications, or users to interact across a network.
* They are usually designed as **peer-to-peer** or **client-server** architectures.

---

## 🔄 Peer-to-Peer Networking

In **peer-to-peer (P2P) networking**:

* Multiple network nodes each run their **own equally privileged instance** of a service.
* These nodes **share** and **exchange** a common set of data.

📌 **Example**:

* A network of **DNS servers** sharing and updating domain name records.

---

## 👨‍💻 Client-Server Networking

In **client-server networking**:

* One or more **server nodes** exist in a network.
* Multiple **clients** communicate with these servers.

📌 **Example**:

* **SSH (Secure Shell)**

  * An SSH client connects to a remote **SSH server** via a secure terminal session.
  * This is often used for **remote administration** purposes.

---

# 📡 **DHCP Servers in Linux**

## 📝 What is DHCP?

**DHCP (Dynamic Host Configuration Protocol)** is a network service that:

* Dynamically assigns **IP addresses** to devices (clients) on a network.
* Eliminates the need for **manual configuration** of IP addresses.
* Provides additional client configuration data like:

  * **MAC addresses**
  * **DNS server addresses**
  * **Default gateway**

---

## ⚙️ How DHCP Works

1. **Client Broadcast Request (Layer 2)**

   * A device sends a **broadcast message** on the local network to find a DHCP server.
   * Since this discovery happens at the **Data Link Layer (Layer 2)**, the request **cannot cross network boundaries** (it stays inside the local subnet).

2. **Server Reply (Layer 2 → Layer 4)**

   * The DHCP server responds with an **IP address** and additional configuration.
   * After the initial handshake, DHCP uses **UDP** at **Layer 4 (Transport Layer)**.

3. **Ports Used**

   * **UDP Port 67** → Used by DHCP server.
   * **UDP Port 68** → Used by DHCP client.

4. **IP Address Leasing**

   * DHCP assigns IP addresses for a specific **lease time** (finite or infinite).
   * The client must **renew the lease** before it expires.
   * If renewal fails → IP may be reassigned to another device.
   * Late renewal → Client may receive a **new IP address** if the old one is taken.

---

## 🔗 Communication Flow

* **Client → Broadcasts DHCP Discover**
* **Server → Offers IP + Config**
* **Client → Requests IP**
* **Server → Acknowledges & Assigns IP**

This entire workflow is often called **DORA** (Discover, Offer, Request, Acknowledge).

---

## 💻 Linux Example: Querying DHCP

You can verify your DHCP-assigned settings using:

```bash
ip route
```

### ✅ Example Output:

```bash
hashim@Hashim:~$ ip route
default via 10.0.2.2 dev enp0s3 proto dhcp src 10.0.2.15 metric 100 
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
```

### 🔍 Explanation

* **default via 10.0.2.2**

  * Default **gateway/router** (assigned by DHCP).
  * All traffic going outside your local network is sent here first.

* **dev enp0s3**

  * Network interface card (NIC) being used.

* **proto dhcp**

  * Confirms that the route was assigned via **DHCP protocol**.

* **src 10.0.2.15**

  * The IP address **assigned to your machine**.

* **metric 100**

  * Route priority; lower metric = higher priority.

* **10.0.2.0/24 dev enp0s3**

  * Local subnet assigned (range: `10.0.2.0` to `10.0.2.255`).
  * `/24` = subnet mask `255.255.255.0`.

---

## 🌍 Testing Connectivity: `traceroute`

Another useful command is:

```bash
traceroute google.com
```

### ✅ Example Output:

```bash
hashim@Hashim:~$ traceroute google.com
traceroute to google.com (216.58.209.142), 30 hops max, 60 byte packets
 1  10.0.2.2 (10.0.2.2)  26.096 ms  1.129 ms  1.549 ms
```

### 🔍 Explanation

* **traceroute google.com** → Traces the path packets take from your system to Google.
* **30 hops max** → Maximum number of intermediate routers it will check.
* **First hop: 10.0.2.2** → This is your **default gateway** (router) provided by the DHCP server.
* The response times (`26.096 ms`, `1.129 ms`, `1.549 ms`) show the **latency** between your system and the gateway.

This proves that:

* Your system is connected via **DHCP**.
* The first step in your network path is your **local gateway/router**.

---

# 🌐 **DNS Servers in Linux**

## 📝 What is DNS?

**DNS (Domain Name System)**, also known as a **name server**, is a service that:

* Converts **hostnames** (like `wikipedia.org`) into **IP addresses** (like `208.80.154.224`).
* Makes it easier for users and applications to communicate without remembering complex numeric IPs.
* Uses the **DNS protocol** (Application Layer, OSI Layer 7).

📖 Analogy:
DNS is like an **address book** 📖 — you look up a name (hostname) and get the exact address (IP).

---

## 🏗️ DNS in TCP/IP Networks

* Devices can communicate with each other using **hostnames**, not just IPs.
* On the internet, DNS relies on a **globally distributed network of DNS servers**.
* Even in small local networks, DNS simplifies communication between machines.

---

## 🛠️ Types of DNS Servers

DNS servers are organized in a **hierarchical system**, working together to resolve queries.

1. **Recursive Servers**

   * Act as resolvers for user queries.
   * Contact multiple DNS servers on your behalf to find the final IP.
   * Use **caching** for faster future lookups.

2. **Root Servers**

   * The top-level servers in DNS hierarchy.
   * Direct queries to appropriate **TLD servers**.

3. **TLD Servers**

   * Manage top-level domains (`.com`, `.org`, `.net`, etc.).
   * Direct queries to **authoritative servers** for the requested domain.

4. **Authoritative Servers**

   * Contain the **actual DNS records** (zone files) of domains.
   * Provide the final IP address to complete the lookup.

---

## 🔄 Recursive vs Iterative Queries

* **Recursive Query**:

  * A DNS server (resolver) does all the work and gives you the final answer.
  * Faster because it uses **caching**.

* **Iterative Query**:

  * Each DNS server replies with a reference to another DNS server.
  * The client follows the chain until the answer is found.

---

## 📂 DNS Zone Files

* DNS servers store **hostname-to-IP mappings** in **zone files**.
* Zone files are usually simple **ASCII text files**.
* On Linux, a local resolver file is:

```bash
/etc/resolv.conf
```

---

## 💻 Querying DNS in Linux

### 🔎 1. Checking Local DNS Resolver

```bash
cat /etc/resolv.conf | grep nameserver
```

#### Example Output:

```bash
hashim@Hashim:~$ cat /etc/resolv.conf | grep nameserver
nameserver 127.0.0.53
```

✅ **Explanation**:

* `127.0.0.53` → Local loopback DNS resolver (systemd-resolved).

---

### 🔎 2. Using `nslookup`

Install **dnsutils** if not available:

```bash
sudo apt install dnsutils
```

#### Example 1: Lookup Local Host

```bash
nslookup neptune.local
```

Output:

```bash
Server:  127.0.0.53
Address: 127.0.0.53#53
```

✅ This shows the local DNS resolver is handling the request.

---

#### Example 2: Interactive `nslookup`

```bash
nslookup
> wikipedia.org
```

Output:

```bash
Server:   127.0.0.53
Address:  127.0.0.53#53

Non-authoritative answer:
Name:     wikipedia.org
Address:  103.102.166.224
Name:     wikipedia.org
Address:  2001:df2:e500:ed1a::1
```

✅ **Explanation**:

* **Server**: Local resolver (loopback).
* **Name**: Domain being queried (`wikipedia.org`).
* **Address**: IPv4 (`103.102.166.224`) and IPv6 (`2001:df2:e500:ed1a::1`) results.

To exit → Press **Ctrl + C**.

---

#### Example 3: Reverse Lookup

```bash
nslookup 8.8.8.8
```

Output:

```bash
8.8.8.8.in-addr.arpa  name = dns.google.
```

✅ This resolves the IP `8.8.8.8` (Google’s public DNS) back to its hostname `dns.google`.

---

### 🔎 3. Using `dig`

Install if missing:

```bash
sudo apt install dnsutils   # Ubuntu/Debian
sudo dnf install bind-utils # Fedora
```

#### Example: Forward Lookup

```bash
dig google.com
```

Output (shortened):

```bash
;; ANSWER SECTION:
google.com.   278 IN A  216.58.209.142
```

✅ Shows that `google.com` resolves to **216.58.209.142**.

---

#### Example: Reverse Lookup

```bash
dig -x 8.8.4.4
```

Output:

```bash
;; ANSWER SECTION:
4.4.8.8.in-addr.arpa. 8199 IN PTR dns.google.
```

✅ Confirms IP `8.8.4.4` belongs to `dns.google`.

---

## 📊 DNS in OSI Model

* Operates at **Application Layer (Layer 7)**.
* Uses **Port 53** (both TCP & UDP).
* Works closely with DHCP to provide:

  * **IP addressing** (DHCP)
  * **Name resolution** (DNS)

---


