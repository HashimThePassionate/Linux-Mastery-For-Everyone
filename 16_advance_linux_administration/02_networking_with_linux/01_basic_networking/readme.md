# 📖 What is a **Computer Network**?

A **computer network** is a group of **two or more computers (or nodes)** connected via a **physical medium** (cable, wireless, optical) and communicating with each other using a **standard set of communication protocols**.

---

## 🏗️ Network Infrastructure

At a high level, a network infrastructure includes:

* 💻 **Computers & Devices**
* 🔀 **Switches**
* 🌐 **Routers**
* 🧵 **Ethernet or Optical Cables**
* 📡 **Wireless Environments**
* ⚙️ **Network Equipment**

---

## 🔎 Logical Layout

Beyond physical connectivity, networks are also defined by their **logical structure**, such as:

* **Topologies**
* **Tiers**
* **Data Flow**

📌 **Example:**
A **three-tiered networking hierarchy**:

1. 🛡️ **DMZ (Demilitarized Zone)** – Outward-facing, adds a security layer against the internet.
2. 🚧 **Firewall** – Controls and filters traffic between DMZ & internal network.
3. 🏠 **Internal Network** – The secure organizational network.

---

## 🆔 Network Device Identification

Devices on a network are identified by:

* **Network Addresses** → Locate nodes (e.g., IP addresses).
* **Hostnames** → Easy-to-remember labels, friendlier than numeric addresses.

---

## 📏 Classification of Networks

### 🔹 Local Area Network (LAN)

* Devices connected in a **single, limited area** (residence, office, school).
* Size can range from **a few devices → thousands of computers**.
* Examples:

  * 🏠 Home Wi-Fi network
  * ☕ Coffee shop’s free Wi-Fi
* 🔗 [Cisco: What is a LAN?](https://www.cisco.com/c/en/us/products/switches/what-is-a-lan-local-area-network.html)

### 🔹 Wide Area Network (WAN)

* A **network of networks** connecting multiple LANs.
* Examples:

  * 🌍 **Internet** (largest WAN)
  * Multinational company networks across regions.
* **Built by service providers** & leased to businesses/institutions.
* Variations:

  * 👤 **PAN (Personal Area Network)**
  * 🏙️ **MAN (Metropolitan Area Network)**
  * ☁️ **IAN (Internet/Cloud Area Network)**
* 🔗 [Cisco: What is a WAN?](https://www.cisco.com/c/en/us/products/switches/what-is-a-wan-wide-area-network.html)

---

# 📚 OSI Model

The **Open Systems Interconnection (OSI) model** is a **theoretical representation of multilayer communication** between computer systems over a network.

* 📅 Introduced: **1983**
* 🏢 Organization: **ISO (International Organization for Standardization)**
* 🎯 Purpose: To provide a **standard framework** for different computer systems to communicate.

---

## 📊 OSI Layers

<div align="center">
  <img src="./images/01.png" alt="" width="600px"/>
</div>

| Layer           | Number | Data Unit           | Description                                                                        |
| --------------- | ------ | ------------------- | ---------------------------------------------------------------------------------- |
| 🖥️ Application | 7      | Data                | User interaction & high-level APIs                                                 |
| 🎨 Presentation | 6      | Data                | Translates data → usable format (encrypt/decrypt, encode/decode, compress/deflate) |
| 🔗 Session      | 5      | Data                | Manages sessions (connections, sockets, ports)                                     |
| 🚚 Transport    | 4      | Segments, Datagrams | Reliable delivery via **TCP/UDP**                                                  |
| 📦 Network      | 3      | Packets             | Adds addressing, routing, & traffic control                                        |
| 📑 Data Link    | 2      | Frames              | Formats reliable frames with MAC & LLC headers                                     |
| ⚡ Physical      | 1      | Bits                | Transmission/reception of raw bit streams                                          |

---

## 🔄 Data Encapsulation & Decapsulation

<div align="center">
  <img src="./images/02.png" alt="" />
</div>

### 🔹 Encapsulation (Sending Data)

Data moves **downward (Layer 7 → Layer 1)**:

1. **Layer 7 – Application**: User interacts with application.
2. **Layer 6 – Presentation**: Data converted → usable format.
3. **Layer 5 – Session**: Manages connection/session.
4. **Layer 4 – Transport**: Data split into **segments**; TCP/UDP header added.
5. **Layer 3 – Network**: Data becomes **packets**; IP header added.
6. **Layer 2 – Data Link**: Packets become **frames**;

   * Adds **MAC addresses (source & destination)**
   * Adds **LLC header**
   * Adds **FCS (Frame Check Sequence)** for error detection.
7. **Layer 1 – Physical**: Frames converted into **bits** for transmission.

---

### 🔹 Decapsulation (Receiving Data)

Data moves **upward (Layer 1 → Layer 7)**:

1. **Layer 1 – Physical**: Bits received & synchronized.
2. **Layer 2 – Data Link**:

   * Error checked via **FCS** using **CRC (Cyclic Redundancy Check)**.
   * Frame → Packet.
3. **Layer 3 – Network**: IP header processed.
4. **Layer 4 – Transport**: TCP/UDP validation.
5. **Layer 5 – Session**: Session re-established.
6. **Layer 6 – Presentation**: Data decrypted, decompressed, converted.
7. **Layer 7 – Application**: Data delivered to end-user application.

---

## 📡 Physical Layer (Layer 1)

The **physical layer** is the foundation of the OSI model. It deals with the **hardware infrastructure** that connects devices and enables communication.

### 🔑 Responsibilities

* Handles **conversion between raw bit streams** and the communication medium.
* Regulates **bit-rate control** (the speed of communication).
* Transfers data through **electrical, radio, or optical signals**.

### ⚙️ Components

* **Cables** (coaxial, fiber optics, twisted pair, etc.)
* **Wireless environments** (Wi-Fi, Bluetooth, etc.)
* **Optical mediums** (laser, infrared, fiber optics)
* **Connectors & switches**

### 📜 Protocols Operating at Layer 1

* **Ethernet**
* **Universal Serial Bus (USB)**
* **Digital Subscriber Line (DSL)**

---

## 🔗 Data Link Layer (Layer 2)

The **data link layer** ensures a **reliable data flow** between two directly connected devices (e.g., nodes in a **WAN** or devices in a **LAN**).

### 🔑 Responsibilities

* Ensures **flow control** (adapting to the physical layer’s speed).
* Detects and **corrects communication errors** from the physical layer.
* Handles **framing** (breaking data into manageable units).
* Prevents and recovers from **frame collisions** when multiple devices access the same channel.

### ⚙️ Subsystems

1. **Media Access Control (MAC)**

   * Uses **MAC addresses** to identify and connect devices.
   * Controls **permissions** for devices to transmit/receive data.

2. **Logical Link Control (LLC)**

   * Identifies and encapsulates **network layer protocols**.
   * Performs **error checking** and **frame synchronization**.

### 🗂️ Frames

* A **frame** = data transmission unit at the data link layer.
* It acts as a **container for a single network packet**.
* Packets inside frames move to the next OSI level (**network layer**).

### 📦 Types of Frames

* **Ethernet Frames**

  * IEEE 802.3 (original format)
  * **802.3 SNAP** (SubNetwork Access Protocol)
  * **Ethernet II (extended)**

### 📜 Example Protocol

* **Point-to-Point Protocol (PPP)**

  * Binary networking protocol.
  * Widely used in **high-speed broadband communication networks**.


## 🌍 Network Layer (Layer 3)

The **network layer** is responsible for **finding the optimal communication path** between devices across networks. It ensures data packets move from **source → destination** effectively.

### 🔑 Responsibilities

* **Routing:** Determines the best path for data using **IP addresses**.
* **Packetization:**

  * **Transmitting end:** Breaks **transport layer segments** into **network packets**.
  * **Receiving end:** Reassembles **frames (from Data Link Layer)** into **packets**.

### 🛠️ Key Protocols

* **Internet Control Message Protocol (ICMP):**

  * Used for **diagnosing network issues**.
  * Sends error/status messages such as:

    * `Destination network unreachable`
    * `Timer expired`
    * `Source route failed`

---

## 🚚 Transport Layer (Layer 4)

The **transport layer** ensures reliable **end-to-end delivery** of data, working with **segments (TCP)** or **datagrams (UDP)**.

### 🔑 Responsibilities

* **Data Segmentation:**

  * **Transmitting end:** Breaks data (from Session Layer) into **segments**.
  * **Receiving end:** Reassembles **packets (from Network Layer)** into **segments**.
* **Quality of Service (QoS):** Guarantees specific delivery quality.
* **Reliability:** Maintains integrity with error detection and retransmission.
* **Flow Control:** Matches transfer rate between sender & receiver to prevent overload.
* **Error Control:** Requests retransmission if data is corrupted or missing.

### 🛠️ Key Protocols

* **Transmission Control Protocol (TCP):**

  * Reliable, connection-oriented.
  * Ensures ordered delivery and retransmission if errors occur.
* **User Datagram Protocol (UDP):**

  * Faster, connectionless.
  * No error correction—used in streaming, gaming, VoIP.

## 💬 Session Layer (Layer 5)

The **session layer** manages the **lifetime of communication sessions** (channels) between devices. A session defines when communication starts, how long it lasts, and how it ends.

### 🔑 Responsibilities

* Establishes, manages, and terminates **sessions**.
* Uses **network addresses, sockets, and ports** to define sessions.
* Ensures **data integrity** within a session.
* Provides **checkpointing & recovery** → if a session is interrupted, it resumes from the last checkpoint.

### 🛠️ Protocols

* **Remote Procedure Call (RPC):** Used in interprocess communications.
* **NetBIOS (Network Basic Input/Output System):** Provides **file-sharing** and **name-resolution** services.

---

## 🎨 Presentation Layer (Layer 6)

The **presentation layer** acts as the **translator** of the OSI model. It ensures that data sent from one system can be understood by another, regardless of their internal formats.

### 🔑 Responsibilities

* Converts data into a **system-independent representation** before transmission.
* Transforms incoming data into **application-friendly formats**.
* Handles:

  * 🔒 **Encryption / Decryption** (e.g., SSL/TLS)
  * 📦 **Compression / Decompression** (e.g., ZIP)
  * 🔤 **Encoding / Decoding**
  * 🔄 **Serialization / Deserialization**

### 📂 Examples of Standard Data Formats

* **ASCII** (American Standard Code for Information Interchange)
* **XML** (Extensible Markup Language)
* **JSON** (JavaScript Object Notation)
* **JPEG** (Image format)
* **ZIP** (Compression format)

⚡ Note: In practice, the **presentation layer and application layer** are often tightly coupled.

---

## 🖥️ Application Layer (Layer 7)

The **application layer** is the **closest layer to the end user**. It does not run applications themselves, but rather provides **communication services** that applications use.

### 🔑 Responsibilities

* Provides **input/output handling** for application data.
* Acts as a **bridge** between user applications and the network.
* Supports end-user services like **web browsing, file transfers, and email**.

### 🛠️ Protocols

* **DNS** (Domain Name System)
* **HTTP** (HyperText Transfer Protocol)
* **FTP** (File Transfer Protocol)
* **Email Protocols:**

  * **POP** (Post Office Protocol)
  * **IMAP** (Internet Message Access Protocol)
  * **SMTP** (Simple Mail Transfer Protocol)

---

# 🏗️ OSI vs TCP/IP Model

* The **OSI Model** (7 layers) provides a **theoretical framework** for how network communication should work.
* The **TCP/IP Model** is a **practical implementation** with fewer layers (some OSI layers are collapsed).

### ✅ Why They Matter

* **OSI Model:** Great for **understanding networking concepts** and troubleshooting.
* **TCP/IP Model:** Widely used in real-world networking because of its **protocol-centric approach**.

---

# 🌐 **TCP/IP Network Stack Model**

The **TCP/IP model** is a **four-layer interpretation** of the OSI networking stack, where some of the OSI layers are consolidated.

📌 **Chronology:**

* The **TCP/IP model** was developed **before** the OSI model.
* Proposed by the **US Department of Defense (DoD)** as part of a DARPA (Defense Advanced Research Projects Agency) project.
* This project later evolved into the **modern internet**.

Both **OSI** and **TCP/IP** provide a layered approach to networking, but TCP/IP is more **protocol-centric** and practical, whereas OSI is more **theoretical**.

---

## 🗂️ Comparison: OSI vs TCP/IP Models

<div align="center">
  <img src="./images/03.png" width="500px"/>
  
**Figure 7.3 – The OSI and TCP/IP models**
</div>


---

## 📶 Layers of the TCP/IP Model

### 1️⃣ Network Interface Layer

* Responsible for **data delivery over a physical medium** (wire, wireless, optical).
* Combines **Physical Layer + Data Link Layer** of the OSI model.

🔑 **Protocols:**

* Ethernet
* Token Ring
* Frame Relay

---

### 2️⃣ Internet Layer

* Provides **connectionless data delivery** between nodes.
* Uses **routing functions** to determine the best path.
* Breaks data into **packets** at sender, reassembles at receiver.
* Maps to **Network Layer (L3)** in the OSI model.

🔑 **Protocols:**

* IP (Internet Protocol)
* ARP (Address Resolution Protocol)
* ICMP (Internet Control Message Protocol)
* IGMP (Internet Group Management Protocol)

---

### 3️⃣ Transport Layer

* Also called **Transmission Layer** or **Host-to-Host Layer**.
* Ensures **reliable communication** between endpoints.
* Implements **error detection & correction**.
* Maps directly to **Transport Layer (L4)** in the OSI model.

🔑 **Protocols:**

* TCP (Transmission Control Protocol)
* UDP (User Datagram Protocol)

---

### 4️⃣ Application Layer

* Provides **data communication services** to applications.
* Combines **Session, Presentation, and Application Layers** of the OSI model.

🔑 **Protocols:**

* DNS (Domain Name System)
* HTTP/HTTPS (Web communication)
* FTP (File Transfer Protocol)
* SMTP, POP, IMAP (Email protocols)
* SNMP (Simple Network Management Protocol)
* Telnet

---

## 🌍 Key Differences

| Feature      | OSI Model (7 Layers)                | TCP/IP Model (4 Layers)                 |
| ------------ | ----------------------------------- | --------------------------------------- |
| **Concept**  | Theoretical model for communication | Practical model for the internet        |
| **Layers**   | 7 (detailed separation)             | 4 (consolidated)                        |
| **Focus**    | Functionality of each layer         | Protocols and real-world implementation |
| **Examples** | Education, troubleshooting          | Internet, real-world networking         |

---


# 🌐 **TCP/IP Protocols**

The **TCP/IP protocol suite** is the backbone of the internet. These protocols define **how data is addressed, transmitted, routed, secured, and interpreted** across networks.

Each protocol is standardized in an **RFC (Request for Comments)**, published by the [IETF](https://www.ietf.org/standards/rfcs/).

---

## 🧩 Layer 3 – Network Layer Protocols

### 🌍 Internet Protocol (**IP**) – RFC 791

* **OSI Layer:** Network (L3)
* **Role:** The core protocol of TCP/IP, responsible for delivering data packets across networks.
* **Key Features:**

  * Provides **logical addressing** through IPv4 or IPv6.
  * Splits data into **datagrams** for transport.
  * Supports **fragmentation and reassembly** of large packets.
  * Handles **routing** between networks using routers.
* **Use Case:** Every device on the internet has an IP address (e.g., `192.168.1.1`, `2001:db8::1`).

---

### 🔗 Address Resolution Protocol (**ARP**) – RFC 826

* **OSI Layer:** Data Link (L2)
* **Role:** Maps **IPv4 addresses → MAC addresses** so data can be delivered over Ethernet.
* **Key Features:**

  * Operates within local networks.
  * Maintains an **ARP cache** to store IP-to-MAC mappings.
* **Use Case:** When you visit a website, your computer first asks, *“Who has this IP?”* ARP replies with the MAC address of the device.

---

### 🧭 Neighbor Discovery Protocol (**NDP**) – RFC 4861

* **OSI Layer:** Data Link (L2)
* **Role:** IPv6 replacement for ARP.
* **Key Features:**

  * Handles **address resolution, neighbor reachability, and router discovery**.
  * Uses **ICMPv6 messages**.
* **Use Case:** In IPv6-only networks, NDP enables devices to automatically discover neighbors and configure themselves.

---

### 🛠️ Internet Control Message Protocol (**ICMP**) – RFC 792

* **OSI Layer:** Network (L3)
* **Role:** Provides **diagnostic and error-reporting functions**.
* **Key Features:**

  * Reports unreachable destinations.
  * Alerts when packet TTL (Time To Live) expires.
  * Used in **ping** and **traceroute** commands.
* **Use Case:**

  * `ping google.com` uses ICMP Echo Request/Reply to test connectivity.
  * `traceroute` uses ICMP messages to trace the path packets take.

---

## 🚚 Layer 4 – Transport Layer Protocols

### 📦 Transmission Control Protocol (**TCP**) – RFC 793

* **OSI Layer:** Transport (L4)
* **Role:** Reliable, connection-oriented protocol.
* **Key Features:**

  * Uses **3-way handshake** (SYN, SYN-ACK, ACK) to establish connections.
  * Guarantees **delivery, order, and error correction**.
  * Implements **flow control** (sender doesn’t overwhelm receiver).
* **Use Case:**

  * Web browsing (HTTP/HTTPS)
  * Emails (SMTP, IMAP, POP)
  * File transfers (FTP)

---

### 🚀 User Datagram Protocol (**UDP**) – RFC 768

* **OSI Layer:** Transport (L4)
* **Role:** Lightweight, connectionless protocol.
* **Key Features:**

  * No handshake → faster but less reliable.
  * No retransmission or ordering guarantees.
* **Use Case:**

  * Online gaming 🎮
  * Video/voice streaming 🎥🎙️
  * DNS lookups

---

## 🖥️ Layer 7 – Application Layer Protocols

### 🖧 Dynamic Host Configuration Protocol (**DHCP**) – RFC 2131

* **Role:** Automatically assigns **IP addresses, subnet masks, gateways, and DNS servers**.
* **Process:**

  * DHCP Discover → DHCP Offer → DHCP Request → DHCP Acknowledge (DORA process).
* **Use Case:** When you connect your phone to Wi-Fi, DHCP gives it an IP automatically.

---

### 🌐 Domain Name System (**DNS**) – RFC 2929

* **Role:** Translates **domain names → IP addresses**.
* **Key Features:**

  * Hierarchical system (root → TLD → authoritative servers).
  * Caches results to improve speed.
* **Use Case:**

  * `www.google.com` → `142.250.190.14`.

---

### 🌍 HyperText Transfer Protocol (**HTTP**) – RFC 2616

* **Role:** Core protocol of the **World Wide Web**.
* **Key Features:**

  * Client/server model.
  * Stateless (each request is independent).
* **Use Case:** Loading web pages, APIs.

---

### 📂 File Transfer Protocol (**FTP**) – RFC 959

* **Role:** Transfers files between client ↔ server.
* **Modes:**

  * Active & Passive (different ways of handling connections).
* **Use Case:** Uploading/downloading files to a web server.

---

### 💻 Terminal Network Protocol (**TELNET**) – RFC 854

* **Role:** Provides **text-based remote login**.
* **Problem:** Insecure (sends data in plaintext).
* **Use Case:** Rarely used today, replaced by SSH.

---

### 🔒 Secure Shell (**SSH**) – RFC 4253

* **Role:** Secure remote login and administration.
* **Key Features:**

  * Encryption, authentication, tunneling.
* **Use Case:**

  * Managing Linux servers remotely.
  * Secure file transfer (SCP, SFTP).

---

### 📧 Simple Mail Transfer Protocol (**SMTP**) – RFC 5321

* **Role:** Email sending protocol.
* **Key Features:**

  * Supports authentication & encryption (STARTTLS, SMTPS).
* **Use Case:**

  * Outlook, Gmail sending emails to mail servers.

---

### 📡 Simple Network Management Protocol (**SNMP**) – RFC 1157

* **Role:** Used for **network monitoring and management**.
* **Key Features:**

  * Agents run on devices → report data to SNMP manager.
  * Supports monitoring CPU, memory, bandwidth, etc.
* **Use Case:** Enterprises monitor switches, routers, servers.

---

### ⏰ Network Time Protocol (**NTP**) – RFC 5905

* **Role:** Synchronizes clocks across devices.
* **Key Features:**

  * Uses time servers and hierarchical structure (Stratum levels).
  * Accuracy: within milliseconds over the internet.
* **Use Case:** Financial systems, databases, logging, authentication (Kerberos).

---

## 📊 OSI Mapping of TCP/IP Protocols

| Protocol (Full Name)                | Abbreviation | RFC  | OSI Layer        | Purpose                       |
| ----------------------------------- | ------------ | ---- | ---------------- | ----------------------------- |
| Internet Protocol                   | IP           | 791  | Network (L3)     | Addressing, routing           |
| Address Resolution Protocol         | ARP          | 826  | Data Link (L2)   | IPv4 → MAC mapping            |
| Neighbor Discovery Protocol         | NDP          | 4861 | Data Link (L2)   | IPv6 → MAC mapping            |
| Internet Control Message Protocol   | ICMP         | 792  | Network (L3)     | Diagnostics, error reporting  |
| Transmission Control Protocol       | TCP          | 793  | Transport (L4)   | Reliable delivery             |
| User Datagram Protocol              | UDP          | 768  | Transport (L4)   | Fast, connectionless delivery |
| Dynamic Host Configuration Protocol | DHCP         | 2131 | Application (L7) | Automatic IP allocation       |
| Domain Name System                  | DNS          | 2929 | Application (L7) | Domain → IP resolution        |
| HyperText Transfer Protocol         | HTTP         | 2616 | Application (L7) | Web communication             |
| File Transfer Protocol              | FTP          | 959  | Application (L7) | File transfers                |
| Terminal Network Protocol           | TELNET       | 854  | Application (L7) | Remote text login             |
| Secure Shell                        | SSH          | 4253 | Application (L7) | Secure remote access          |
| Simple Mail Transfer Protocol       | SMTP         | 5321 | Application (L7) | Email transmission            |
| Simple Network Management Protocol  | SNMP         | 1157 | Application (L7) | Device monitoring             |
| Network Time Protocol               | NTP          | 5905 | Application (L7) | Time synchronization          |

---


# 🌐 **IP Addressing, Subnets & Broadcast Addresses**

An **IP address** is a **unique identifier (UID)** for devices in a network. Devices locate and communicate with each other using IP addresses—similar to how postal mail is delivered using house addresses.

---

## 📮 IPv4 vs IPv6

* **IPv4 (Internet Protocol version 4):**

  * 32-bit address → 4,294,967,296 (\~4.3 billion) possible addresses.
  * Written as **4 groups of 8 bits (1 byte each)** separated by dots.
  * Example: `192.168.1.53`

* **IPv6 (Internet Protocol version 6):**

  * 128-bit address → virtually unlimited addresses.
  * Introduced due to IPv4 exhaustion.
  * Example: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`

---

## 🧮 IPv4 Structure

Example: **192.168.1.53**

<div align="center">
  <img src="./images/04.png" width="500px"/>
</div>

* Each number = **8 bits (1 byte)**
* Total = **32 bits (4 bytes)**
* Binary form:

  ```
  11000000.10101000.00000001.00110101
  ```

---

## 🏷️ Network Classes (Classful Addressing)

Originally, IP addresses were divided into **classes** (introduced in 1981) to separate large, medium, and small networks.

<div align="center">
  <img src="./images/05.png" width="500px"/>
</div>

| Class       | Leading Bits | Start Address | End Address     | Default Subnet Mask | Usage               |
| ----------- | ------------ | ------------- | --------------- | ------------------- | ------------------- |
| **Class A** | 0            | 0.0.0.0       | 127.255.255.255 | 255.0.0.0           | Very large networks |
| **Class B** | 10           | 128.0.0.0     | 191.255.255.255 | 255.255.0.0         | Medium networks     |
| **Class C** | 110          | 192.0.0.0     | 223.255.255.255 | 255.255.255.0       | Small networks      |
| **Class D** | 1110         | 224.0.0.0     | 239.255.255.255 | Not defined         | Multicasting        |
| **Class E** | 1111         | 240.0.0.0     | 255.255.255.255 | Not defined         | Experimental        |

⚠️ Today, class-based addressing is mostly replaced by **CIDR (Classless Inter-Domain Routing)**.

---

## 🖧 Subnets

A **subnet (subnetwork)** is a logical division of a network.

Example:

* IP Address: `192.168.1.53`
* **Network ID:** `192.168.1`
* **Host ID:** `53`

<div align="center">
  <img src="./images/06.png" width="500px"/>
</div>

### 🔑 Subnet Mask

* Defines which part of the IP is **network** and which is **host**.
* Example: `192.168.1.0` with mask `255.255.255.0`

### 🔢 CIDR Notation

* Example: `192.168.1.0/24`
* `/24` = first 24 bits represent the **network ID**.

---

## 🧩 Subnetting Example

Suppose we want host addresses between **100 → 125** in network `192.168.1.x`.

1. Starting point: `192.168.1.100`

   ```
   11000000.10101000.00000001.01100100
   ```

   → Host identifier = `100`.

2. Closest reserved boundary = `96`.
   → Equivalent binary: `11100000`

3. Subnet mask:

   ```
   11111111.11111111.11111111.11100000
   = 255.255.255.224
   ```

4. CIDR notation:

   ```
   192.168.1.96/27
   ```

5. Host range:

   * Start: `192.168.1.97`
   * End: `192.168.1.126`
   * Broadcast: `192.168.1.127`

---

## 📢 Broadcast Addresses

A **broadcast address** is reserved for sending data to **all devices** in a subnet.

* Always the **last IP** in the subnet range.

### Examples:

* `192.168.1.0/24` → Broadcast = `192.168.1.255`
* `192.168.1.96/27` → Broadcast = `192.168.1.127`

---

## 📚 References

* [RFC 791 – Internet Protocol](https://tools.ietf.org/html/rfc791)
* [RFC 870 – Classful Networks](https://tools.ietf.org/html/rfc870)
* [RFC 1918 – Subnets & Private Addressing](https://tools.ietf.org/html/rfc1918)
* [RFC 6308 – Multicast Addressing](https://tools.ietf.org/html/rfc6308)

---

# 🌐 **IP Addressing: IPv6, Sockets & Ports**

After IPv4 exhaustion, **IPv6** was introduced as a long-term solution, offering a **128-bit address space**. Alongside IP addressing, the concepts of **sockets** and **ports** enable applications to communicate over networks.

---

## 🔢 IPv6 Addresses

* **Length:** 128 bits (16 bytes).
* **Representation:** Up to **8 groups of 16-bit hexadecimal numbers**, separated by `:` (colons).
* **Range:** Each group ranges from `0000` to `FFFF`.

### Example

```
2001:0b8d:8a52:0000:0000:8b2d:0240:7235
```

### Shortened Form

* Leading zeros omitted.
* All-zero groups collapsed to `::`.

```
2001:b8d:8a52::8b2d:240:7235/64
```

### Prefix Length (/n)

* `/64` → First **64 bits** represent the **network prefix** (subnet).
* Similar to **CIDR in IPv4**.

### Example Subnet

```
2001:b8d:8a52::/64
```

* First 4 groups (`2001:b8d:8a52:0000`) = **Network ID**
* Last 4 groups = **Host ID**

📌 Reference: [RFC 2460 – IPv6 Specification](https://tools.ietf.org/html/rfc2460)

---

## ⚡ Sockets

A **socket** is a **software data structure** representing a communication endpoint in networking.

* In Linux, a socket = **file descriptor** managed via system calls.
* Used by applications to **send/receive data** over the network.
* Exists only for the lifetime of the **process** that created it.
* Operates at the **Transport Layer (L4)** of the OSI model.

### 🔑 Characteristics

* Two endpoints: **Sender** & **Receiver**.
* Each endpoint has an **IP address**.
* Multiple sockets can exist between two devices (parallel connections).

---

## 🔌 Ports

A **port** is a **logical identifier** that specifies which process/service on a host is communicating.

* **Range:** 0 – 65535
* **Well-Known Ports (0–1024):** Reserved for common services.
* **Ephemeral Ports (>1024):** Temporary, used by applications.
* Always associated with an **IP address** → `(IP address : Port)` pair.

### 📋 Common Well-Known Ports

| Port      | Service                              | Protocol |
| --------- | ------------------------------------ | -------- |
| **21**    | FTP (File Transfer Protocol)         | TCP      |
| **22**    | SSH (Secure Shell)                   | TCP      |
| **25**    | SMTP (Simple Mail Transfer Protocol) | TCP      |
| **53**    | DNS (Domain Name System)             | UDP/TCP  |
| **67/68** | DHCP (67 = server, 68 = client)      | UDP      |
| **80**    | HTTP (HyperText Transfer Protocol)   | TCP      |
| **443**   | HTTPS (HTTP Secure)                  | TCP      |

---

## 🔑 Relationship: Socket = IP + Port

A socket is uniquely identified by:

```
<IP Address> : <Port Number>
```

### Example

* Web browser connecting to Google:

```
IP: 142.250.190.14
Port: 443
Socket = 142.250.190.14:443
```

📌 References:

* [RFC 147 – TCP/IP Socket Concepts](https://tools.ietf.org/html/rfc147)
* [RFC 1340 – Assigned Numbers (Well-Known Ports)](https://tools.ietf.org/html/rfc1340)

---

# 🐧 **Linux Network Configuration**

## 🧰 Prerequisites

* A Linux system (Ubuntu 22.04 used in examples).
* Sudo privileges for editing system network settings.
* Basic familiarity with a text editor like `nano` (or `vim`).

---

## 🔍 Viewing Current IP Configuration

### ✅ Command

```bash
ip addr show
```

### 🧠 What it does

* Uses the `ip` (iproute2) suite to **list all network interfaces** and their assigned **IPv4/IPv6 addresses**, **MAC**, **MTU**, link state, and more.

### 🧩 Important output fields

Your sample:

```bash
hashim@hashim-V:~$ ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:55:08:5a brd ff:ff:ff:ff:ff:ff
    altname enx08002755085a
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 84651sec preferred_lft 84651sec
```

* `1: lo:` — The **loopback** interface (local traffic on the same host).

  * Flags: `LOOPBACK`, `UP` (enabled), `LOWER_UP` (carrier present).
  * `mtu 65536` — Max packet size (loopback uses a large MTU).
  * `inet 127.0.0.1/8` — IPv4 loopback.
  * `inet6 ::1/128` — IPv6 loopback.
* `2: enp0s3:` — A **physical**/virtual Ethernet interface.

  * Flags: `BROADCAST`, `MULTICAST`, `UP`, `LOWER_UP` (link detected).
  * `mtu 1500` — Typical Ethernet MTU.
  * `qdisc fq_codel` — Queue discipline used for buffering/latency control.
  * `link/ether 08:00:27:55:08:5a` — **MAC address**.
  * `altname enx08002755085a` — An alternate kernel/udev-generated stable name.
  * `inet 10.0.2.15/24` — IPv4 address with a `/24` mask.
  * `brd 10.0.2.255` — Calculated broadcast address.
  * `scope global` — Globally scoped (not limited to host or link only).
  * `dynamic` — Assigned by **DHCP**.
  * `noprefixroute` — Kernel won’t auto-add a connected route; routing handled by manager.
  * `valid_lft` / `preferred_lft` — Lease lifetimes from DHCP.

> 💡 Tip: On VirtualBox NAT, `10.0.2.15` is the classic DHCP IP for the guest.

---

## 🧭 Ubuntu Network Configuration with **Netplan**

Ubuntu 22.04 uses **Netplan** to generate backend configs for either:

* **NetworkManager** (desktop-oriented), or
* **systemd-networkd** (server/cloud oriented).

### 📁 Where configs live

```bash
ls /etc/netplan/
```

Your output:

```bash
hashim@hashim-V:~$ ls /etc/netplan/
01-network-manager-all.yaml  50-cloud-init.yaml  90-NM-1eef7e45-3b9d-3043-bee3-fc5925c90273.yaml
```

* Multiple YAML files can exist. **Netplan merges** them by filename order.
* Lower numbers (e.g., `01-...`) apply **first**, higher numbers (e.g., `90-...`) can **override**.
* Files like `50-cloud-init.yaml` may be provisioned by cloud-init.
* You can safely **back up** before editing:

### ✅ Command (backup)

```bash
sudo cp /etc/netplan/50-cloud-init.yaml /etc/netplan/50-cloud-init.yaml.bak
```

* **What it does:** Copies the file to a `.bak` backup.
* **When to use:** Before making changes so you can revert quickly.

---

## 🌐 Dynamic IP (DHCP) Configuration

### 1) Edit the Netplan YAML

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Paste/update (your example):

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
```

* `version: 2` — Netplan schema version.
* `ethernets.enp0s3` — Configure the interface named `enp0s3`.
* `dhcp4: true` — Enable **DHCPv4** to automatically get IP, gateway, DNS.

### 2) Test the configuration safely

```bash
sudo netplan try
```

**What you saw:**

```text
Press ENTER before the timeout to accept the new configuration

Changes will revert in 114 seconds
Configuration accepted.
```

* **What it does:** Applies config **temporarily** with a fail-safe timeout.
* **Why it’s great:** If you lose connectivity (e.g., remote SSH), it auto-reverts after the countdown.
* **Accept:** Press **ENTER** to keep changes permanently.

---

## 📍 Static IP Configuration

### 1) Edit the YAML for a static address

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Your content:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.122.22/24
      routes:
        - to: 0.0.0.0/0
          via: 192.168.122.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

**Explanation:**

* `dhcp4: false` — Turn off DHCPv4.
* `addresses` — One or more **CIDR** addresses. Here: `192.168.122.22/24`.
* `routes` — Static routes. `0.0.0.0/0` means **default route** (send all traffic) via **gateway** `192.168.122.1`.
* `nameservers.addresses` — DNS servers (**Google DNS** in this case).

> ⚠️ **Caution (remote systems):** Switching to static can drop your SSH session if misconfigured (wrong gateway/DNS). Always use `sudo netplan try` first.

### 2) Test, accept, and verify

```bash
sudo netplan try
```

Your prompt:

```text
Do you want to keep these settings?

Press ENTER before the timeout to accept the new configuration

Changes will revert in 117 seconds
Configuration accepted.
```

### 3) Check the result

```bash
ip addr show
```

Your intermediate output:

```bash
...
inet 192.168.122.22/24 ... enp0s3
...
inet 10.0.2.15/24 ... dynamic ... enp0s3
```

#### 🔎 Why did you see **two IPv4 addresses** on the same interface?

Because **another Netplan YAML** (or the backend) was still enabling DHCP on `enp0s3`. You found it with:

```bash
sudo grep -nH "dhcp4" /etc/netplan/*.yaml
```

Output:

```bash
/etc/netplan/50-cloud-init.yaml:5:      dhcp4: false
/etc/netplan/90-NM-1eef7e45-3b9d-3043-bee3-fc5925c90273.yaml:7:      dhcp4: true
```

* `-n` — Show line numbers.
* `-H` — Show filename.
* Pattern `"dhcp4"` — Find lines configuring DHCPv4.
* **Interpretation:** `50-cloud-init.yaml` disables DHCP, but `90-NM-*.yaml` re-enables it later (higher precedence), so both a **static** and a **DHCP** address were active.

### 4) Inspect the other YAML (and what fields mean)

```bash
sudo cat /etc/netplan/90-NM-1eef7e45-3b9d-3043-bee3-fc5925c90273.yaml
```

You saw:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      renderer: NetworkManager
      match: {}
      dhcp4: false
      networkmanager:
        uuid: "1eef7e45-3b9d-3043-bee3-fc5925c90273"
        name: "netplan-enp0s3"
        passthrough:
          connection.timestamp: "1757402215"
          ipv6.method: "disabled"
          ipv6.ip6-privacy: "-1"
          proxy._: ""
```

**Explanation of special fields:**

* `renderer: NetworkManager` — Tells Netplan to render configs for **NetworkManager** (desktop). Alternative is `networkd` (servers).
* `match: {}` — No specific match rules; applies by interface name (`enp0s3`).
* `networkmanager: ...` — NM-specific metadata:

  * `uuid`/`name` — Connection profile identity in NM.
  * `passthrough` — Raw NM key/values passed directly (e.g., disable IPv6).
  * `connection.timestamp` — Last change epoch timestamp.
  * `ipv6.method: disabled` — IPv6 disabled for this profile.
  * `ipv6.ip6-privacy: -1` — NM privacy setting (default/auto).

> ✅ You ultimately set **`dhcp4: false`** here and then applied Netplan, removing the DHCP address so only the static address remained.

### 5) Apply permanently

```bash
sudo netplan apply
```

* **What it does:** Writes backend configs and activates them permanently (no auto-revert timer).
* **When to use:** After verifying with `netplan try`, or when working locally.

### 6) Final verification

```bash
ip addr show
```

Your final state:

```bash
...
inet 192.168.122.22/24 brd 192.168.122.255 scope global noprefixroute enp0s3
   valid_lft forever preferred_lft forever
```

Now only the **static** IP remains on `enp0s3`.

---

## 🧪 Command Reference — With Detailed Explanations

### 1) `ip addr show`

* **Purpose:** List interfaces, IPs, MAC, MTU, flags, lifetimes.
* **Useful flags:**

  * `ip -br addr` — Brief, one-line per interface.
  * `ip -4 addr` / `ip -6 addr` — Only IPv4 / only IPv6.
* **When to use:** Quick status before/after changes.

### 2) `ls /etc/netplan/`

* **Purpose:** Show all Netplan YAMLs being merged in order.
* **When to use:** If behavior is unexpected — there may be **multiple files**.

### 3) `sudo cp SRC DST`

* **Purpose:** Back up configs before editing.
* **When to use:** **Always** before risky edits.

### 4) `sudo nano /etc/netplan/<file>.yaml`

* **Purpose:** Edit Netplan config.
* **Tips:** Maintain **YAML indentation** (2 spaces). Tabs break YAML.

### 5) `sudo netplan try`

* **Purpose:** Test configuration with **auto-revert** safety (great over SSH).
* **Flow:**

  1. Applies config temporarily.
  2. Countdown displayed.
  3. Press **ENTER** to keep; do nothing to revert.

### 6) `sudo netplan apply`

* **Purpose:** Apply the configuration **permanently** (no timer).
* **When to use:** After you’re confident the config is correct.

### 7) `sudo grep -nH "dhcp4" /etc/netplan/*.yaml`

* **Purpose:** Find where DHCP is configured across multiple Netplan files.
* **Flags:**

  * `-n` (line numbers), `-H` (filenames).
* **When to use:** Duplicate or conflicting settings symptoms (e.g., two IPs).

### 8) `sudo cat /etc/netplan/<file>.yaml`

* **Purpose:** Inspect exact file contents.
* **When to use:** Auditing merged behavior and understanding overrides.

---

## 🧠 Netplan Merging & Precedence (Why Multiple Files Matter)

* Netplan **merges all YAMLs** in `/etc/netplan/` in **lexicographical order**.
* Higher numbers (e.g., `90-...yaml`) can **override** earlier settings (`01-...yaml`, `50-...yaml`).
* If two files touch the **same interface** (`enp0s3`) and one says `dhcp4: true` while another says `false`, the **later file wins**.
* **Best practice:** Keep a **single source of truth** for each interface to avoid confusion.

---

## 🧬 DHCP vs Static — When to Use What?

* **DHCP (dynamic)**

  * Pros: Zero manual config, works anywhere, ideal for laptops/VMs that move networks.
  * Cons: IP can change; not ideal for servers needing stable addresses.
* **Static**

  * Pros: Stable IP for servers/services, port forwarding, firewall rules, DNS.
  * Cons: Must set **address**, **gateway**, and **DNS** correctly; mistakes can isolate the machine.

---

## 🧰 Quick Troubleshooting

* See generated backend files:

  ```bash
  sudo netplan generate --debug
  ```

  *(Shows how Netplan renders your YAML and which renderer is being used.)*

* Check routing/gateway:

  ```bash
  ip route
  ```

  Look for a default route like:

  ```
  default via 192.168.122.1 dev enp0s3
  ```

* Test connectivity:

  ```bash
  ping -c 3 8.8.8.8        # tests basic IP connectivity
  ping -c 3 google.com      # tests DNS resolution too
  ```

* If using **NetworkManager**, confirm what NM thinks:

  ```bash
  nmcli device show enp0s3
  nmcli connection show
  ```

> 💡 If you’re on a GUI desktop, NetworkManager might manage the interface even when Netplan is present. Ensuring the **renderer** is consistently `NetworkManager` (or `networkd`) avoids conflicts.

---


# 🌍 **Linux Hostname Configuration**


## 📌 What is a Hostname?

* The **hostname** is the **name of your computer** on a network.
* It helps other systems and users identify your machine.
* Example: `hashim-V`, `server01`, `db-node-1`.

---

## 🔎 Viewing the Current Hostname

### ✅ Commands

```bash
hostname
```

or

```bash
hostnamectl
```

### 🧠 Explanation

* `hostname` → Prints the current short hostname only.
* `hostnamectl` → Displays **detailed information**, including:

  * Static hostname
  * Chassis type (e.g., `vm`, `server`, `laptop`)
  * Machine ID, Boot ID
  * Virtualization platform
  * OS, Kernel, Architecture
  * Hardware vendor/model

### 🖥️ Example (your output)

```bash
hashim@hashim-V:~$ hostname
hashim-V

hashim@hashim-V:~$ hostnamectl
 Static hostname: Hashim
       Icon name: computer-vm
         Chassis: vm 🖴
      Machine ID: e075bdba9be14d3296eb19435df3db2b
         Boot ID: e2fc4f55bf784f36942c819d22cdcf39
  Virtualization: oracle
Operating System: Ubuntu 25.04
          Kernel: Linux 6.14.0-15-generic
    Architecture: x86-64
 Hardware Vendor: innotek GmbH
  Hardware Model: VirtualBox
Firmware Version: VirtualBox
   Firmware Date: Fri 2006-12-01
    Firmware Age: 18y 9month 3w 1d
```

---

## 🛠️ Changing the Hostname (Permanent Method)

The most convenient way is with the **`hostnamectl`** command.

### ✅ Command

```bash
sudo hostnamectl set-hostname Hashim
```

### 🧠 Explanation

* `hostnamectl` → Systemd tool for managing hostnames.
* `set-hostname` → Changes the **static hostname** (stored in `/etc/hostname`).
* Requires `sudo` because it modifies system files.

### 🔎 Verification

```bash
hostname
hostnamectl
```

Your results:

```bash
hashim@hashim-V:~$ hostname
Hashim

hashim@hashim-V:~$ hostnamectl
 Static hostname: Hashim
 ...
```

✔️ The hostname is now **Hashim** and will **persist after reboot**.

---

## ⚡ Changing the Hostname (Temporary Method)

Alternatively, you can use the `hostname` command to set the hostname:

### ✅ Command

```bash
sudo hostname jupiter
```

### 🧠 Explanation

* This changes the hostname **immediately**, but only in memory.
* After a reboot, it will **revert** to the permanent hostname (stored in `/etc/hostname`).

---

## 📁 Making Hostname Changes Permanent Manually

If you use the temporary method, you must update the following files:

1. **`/etc/hostname`**

   * Contains the system’s permanent hostname (one line).
   * Example:

     ```text
     Hashim
     ```

2. **`/etc/hosts`**

   * Maps hostnames to IP addresses.
   * Must be updated to reflect your new hostname.
   * Example (changing `hashim-V` → `Hashim`):

     ```text
     127.0.0.1   localhost
     127.0.1.1   Hashim
     ```

Without updating `/etc/hosts`, some applications may fail to resolve the hostname.

---

## 🧪 Command Reference — With Explanations

### 1) `hostname`

* **Purpose:** Print or set the hostname temporarily.
* **Usage:**

  * `hostname` → View current hostname.
  * `sudo hostname newname` → Change hostname until reboot.

### 2) `hostnamectl`

* **Purpose:** Manage hostnames (via systemd).
* **Usage:**

  * `hostnamectl` → View detailed system info.
  * `sudo hostnamectl set-hostname newname` → Change hostname permanently.

### 3) Editing `/etc/hostname`

* **Purpose:** Define the permanent hostname.
* **Usage:** Place the desired hostname (single line) in the file.

### 4) Editing `/etc/hosts`

* **Purpose:** Ensure the hostname resolves locally.
* **Usage:** Update `127.0.1.1` entry with your new hostname.

---

## ✅ Quick Checklists

### Permanently Change Hostname

1. Run:

   ```bash
   sudo hostnamectl set-hostname NewName
   ```
2. Verify:

   ```bash
   hostname
   hostnamectl
   ```
3. Check `/etc/hostname` and `/etc/hosts`.

---

### Temporarily Change Hostname

1. Run:

   ```bash
   sudo hostname TempName
   ```
2. Verify with `hostname`.
3. Note: Reverts after reboot unless `/etc/hostname` is updated.

---
