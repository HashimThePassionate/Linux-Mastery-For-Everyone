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

