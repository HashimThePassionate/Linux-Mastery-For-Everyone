# ⚙️ **Understanding Devices** in Linux

In Linux, **everything is treated as a file** — even devices.
Device files are special files in Unix and Linux systems. These special files act as **interfaces to device drivers**, but appear in the filesystem just like regular files.

This section explains how **Linux abstraction layers** work, and how hardware and software are interconnected.

---

## 🧩 Linux Abstraction Layers

A computer system is generally divided into two main layers:

### 🔹 1. Hardware Level

This level contains all the **physical components** of your machine, such as:

* **CPU (Central Processing Unit)** 🖥️
* **RAM (Memory)** 💾
* **Devices** including disks, network interfaces, ports, and controllers

---

### 🔹 2. Software Level

For hardware to function, the **Operating System (Linux)** provides abstraction layers.
These layers exist in the **kernel**, which is the **core software component** of Linux.

When the computer boots:

* The **Linux kernel** is loaded from disk into system memory (RAM).
* Memory is then divided into two regions:

#### 🟢 Kernel Space

* The **heart of Linux** ❤️.
* Manages **hardware components**, **processes**, **system calls**, and **devices**.
* Handles **device drivers** (which act as bridges between hardware and software).
* Only the kernel has direct access here (for security and stability).

#### 🔵 User Space

* This is where **user processes** (programs, applications, daemons, etc.) run.
* Processes are **isolated** so they don’t interfere with each other.
* If user processes need something from the kernel (like accessing memory or devices), they use **system calls**.

---

## 🖥️ Devices in the Linux Model

* Devices are **managed by the kernel**.
* The **kernel controls device drivers**, which translate software requests into hardware actions.
* Devices are accessible **only in kernel mode** (not directly from user space) to maintain security and streamline operations.

---

## 📚 How It Works (Step by Step)

1. **RAM cells** temporarily store data and instructions.
2. **CPU** executes tasks by fetching from RAM.
3. **Processes** run inside **user space**.
4. When a process needs hardware access (like disk I/O, networking, etc.):

   * It makes a **system call** to the kernel.
   * The **kernel processes the request**.
   * Data moves between **CPU ↔ RAM ↔ Devices** via the kernel.

✅ This ensures smooth execution and prevents processes from interfering with each other.

---

