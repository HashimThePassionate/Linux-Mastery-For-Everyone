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