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

<div align="center">
  <img src="./images/01.png" alt="" width="300px"/>
</div>

✅ This ensures smooth execution and prevents processes from interfering with each other.

---

# 📂 **Device Files and Naming Conventions** in Linux

After learning about abstraction layers, the next question is:
👉 **How does Linux actually manage devices?**

Linux uses **userspace /dev (udev)** — a **device manager for the kernel**.
This system works with **device nodes** (also called *device files*), which are special files used as an **interface to drivers**.

---

## 🔧 udev: The Linux Device Manager

* **udev** runs as a **daemon** (`udevd`).
* It listens to kernel **userspace calls** (via **netlink sockets**) to detect and manage devices.
* Configurations for `udevd` are stored in:

```bash
/etc/udev/udev.conf
```

👉 To view this configuration, you can run:

```bash
cat /etc/udev/udev.conf
```

### 📝 Explanation of the command

* `cat` → Prints the contents of a file.
* `/etc/udev/udev.conf` → Path to the **udev configuration file**.
* This lets you see how device management is set up.

---

## 📜 Default udev Rules

Each Linux distribution comes with a **default set of rules** for `udevd`.
These rules are stored in:

```bash
/etc/udev/rules.d/
```

Example (from inside a container/VM):

```bash
root@5ca0168c9be1:/# ls /etc/udev/
hwdb.d  iocost.conf  rules.d  udev.conf

root@5ca0168c9be1:/# ls -l /etc/udev/rules.d/
total 0
```

👉 Here, the `rules.d` directory is empty, but normally it contains `.rules` files that define how devices should be named and managed.

---

## 🔗 Netlink Socket

* The **kernel communicates events** (like device add/remove) to userspace through a **netlink socket**.
* It allows **inter-process communication (IPC)** between kernel space and user space.

---

## 📂 The /dev Directory

The `/dev` directory is the **interface** between:

* **User processes** 👨‍💻
* **Devices managed by the kernel** ⚙️

To view its contents:

```bash
ls -la /dev | head -40
```

### 📝 Explanation of the command

* `ls -la` → Lists files with **long format** (`-l`) and shows **hidden files** (`-a`).
* `/dev` → Path where device files are located.
* `head -40` → Shows only the **first 40 lines** for readability.

---

### 🔤 Device File Types

Inside `/dev`, you’ll see files starting with certain letters:

* **b** → Block devices (e.g., disks)
* **c** → Character devices (e.g., serial ports, terminals)
* **p** → Named pipes (FIFOs)
* **s** → Sockets

Example output:

```bash
crw-rw-rw-  1 root root      1,   7 Aug 29 03:47 full
brw-rw----  1 root disk      7,   0 Aug 29 03:47 loop0
crw-r--r--  1 root root      1,  11 Aug 29 03:47 kmsg
```

* **`c`** → Character device (`crw-rw-rw-`)
* **`b`** → Block device (`brw-rw----`)

---

## 💽 Disk Devices in `/dev`

Disk devices also appear under `/dev` with specific names.
For example:

* `/dev/sda` → First SCSI/SATA disk
* `/dev/sdb` → Second disk, and so on
* `/dev/loop0`, `/dev/loop1` → Loop devices

These names can vary depending on whether you’re on:

* **Virtual machines**
* **Bare-metal servers**

---

