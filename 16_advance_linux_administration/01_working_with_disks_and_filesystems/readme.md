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

# 📘 **Understanding Device Naming Conventions** in Linux

On a **Debian-based system**, the naming convention is designed for **predictability**. It is based on **hardware buses’ names**, which is similar across most modern Linux-based operating systems.

---

## 🔍 Checking Active `udev` Rules

You can check which **udev rules** are active on your system.

* On **Debian-based** and **Red Hat-based** distributions, the rules are stored in:

```
/lib/udev/rules.d/
```

These rules help the system **identify devices consistently** each time they are connected.

---

## 💽 Hard Drives and External Drives

Linux streamlines naming conventions for different types of drives. Below are the most common cases:

### 1️⃣ Classic **IDE Drivers** (used for ATA drives)

* `hda` → Master device on the first channel
* `hdb` → Slave device on the first channel
* `hdc` → Master device on the second channel
* `hdd` → Slave device on the second channel

---

### 2️⃣ **NVMe Drivers** (used for modern SSDs)

* `nvme0` → First device controller (**character device**)
* `nvme0n1` → First namespace (**block device**)
* `nvme0n1p1` → First namespace, first partition (**block device**)

💡 **Tip:** NVMe device names look longer, but the structure makes it easy to distinguish between controllers, namespaces, and partitions.

---

### 3️⃣ **MMC Drivers** (used for SD cards & eMMC chips)

* `mmcblk` → General identifier for SD/eMMC storage
* `mmcblk0` → First MMC device
* `mmcblk0p1` → First MMC device, first partition

---

### 4️⃣ **SCSI Drivers** (used for SATA & USB drives)

* `sd` → General identifier for mass storage devices
* `sda` → First registered SCSI/SATA/USB device
* `sdb` → Second device
* `sdc` → Third device
* `sg` → Refers to generic SCSI layers (**character device**)

💡 Today, most **SATA** and **USB** storage devices fall under this **SCSI-like naming scheme**.

---

## 📂 Why Device Naming Matters?

The devices we are most interested in are **mass storage devices**:

* **HDDs (Hard Disk Drives)**
* **SSDs (Solid-State Drives)**

These drives usually contain **partitions** with specific **filesystem structures**.

👉 Earlier (in **section 2: The Linux Shell and Filesystem**), we introduced the Linux directory structure.
👉 Now, it’s time to dive deeper into **filesystem types in Linux** to understand how data is organized inside these partitions.

---

# 📂 **Understanding Filesystem Types** in Linux

When we talk about **physical media** (like hard drives or external drives), we are not referring to the **directory structure**. Instead, we’re discussing the **structures created on the physical drive** during **formatting** or **partitioning**.

These structures are known as **filesystems**, and they determine **how files are stored, accessed, and managed** on the drive.

---

## 🔑 Key Idea: Filesystems

* A filesystem provides rules for organizing data into **files, directories, metadata, and permissions**.
* Different filesystems offer different **features, performance trade-offs, and reliability guarantees**.
* Some are **Linux-native** (e.g., `Ext4`, `XFS`, `ZFS`, `btrfs`) while others come from **Windows/macOS ecosystems** (e.g., `NTFS`, `APFS`).

---

## 🏆 Common Linux-Native Filesystems

### 🔹 **Ext Family (Ext, Ext2, Ext3, Ext4)**

* **Most widely used historically** in Linux.
* **Ext4** is the most advanced and stable version.
* ✅ Features:

  * Supports block sizes between `512` and `4096` bytes.
  * **Inode reservation** for performance when creating files.
  * Maximum filesystem size: **1 EB** (exabyte).
  * Supports **multi-block allocation** (better large file handling).
  * **Online defragmentation** support.
  * **Checksums** for journaling, improving reliability.
  * Extended timestamps (valid up to **year 2446 AD**).
* ❌ Limitations:

  * Weak in **data corruption detection**.
  * Not designed as a **next-gen enterprise filesystem**, but rather a robust workhorse.

---

### 🔹 **ZFS (Zettabyte File System)**

* Developed at **Sun Microsystems** (2001–2004).
* Combines **filesystem + volume manager** in one.
* Highly scalable **128-bit system**.
* ✅ Features:

  * **Copy-on-write mechanism** (never overwrites data in place).
  * Ensures **data integrity** and high performance.
  * Can handle **huge storage pools** with simple administration.
* Used in: **Solaris, FreeBSD, Ubuntu (via OpenZFS)**.
* 📖 More info: [OpenZFS Docs](https://openzfs.github.io/openzfs-docs/Getting%20Started/index.html)

---

### 🔹 **XFS**

* Origin: **Silicon Graphics, Inc. (IRIX OS)**.
* Focus: **High performance & parallel I/O**.
* ✅ Features:

  * Handles **large datasets** efficiently.
  * Maximum filesystem size: **16 EB**.
  * Single file support: up to **8 EB**.
  * Journals **quota information**.
  * Supports **online maintenance**: defrag, expansion, restore.
  * Backup & restore tools: `xfsdump`, `xfsrestore`.
* Default in **Red Hat Enterprise Linux (RHEL 7+)**.

---

### 🔹 **btrfs (B-tree Filesystem)**

* Designed as a **modern filesystem** for Linux.
* ✅ Features:

  * Supports **snapshots, pooling, checksums, and spanning across devices**.
  * Great for **enterprise environments** requiring robust data handling.
* ❌ Issues:

  * Still under development.
  * Performance concerns in **multi-disk volume managers**.
* Adoption:

  * Used in **SUSE Linux Enterprise** & **openSUSE**.
  * Dropped by **Red Hat**.
  * Chosen as the **future default filesystem in Fedora 33+**.

---

## 📦 Other Filesystems

* **ReiserFS, GlusterFS** → experimental/cluster filesystems.
* **NFS, SMB (Samba/CIFS)** → network filesystems.
* **ISO9660, Joliet** → CD/DVD media.
* **FAT, NTFS (Windows)**.
* **exFAT, APFS, HFS+ (macOS)**.

👉 To check supported filesystems on your Linux system:

```bash
cat /proc/filesystems
```

---

## 🏗️ Virtual File System (VFS)

Linux uses a **special abstraction layer** called the **Virtual File System (VFS)**.

* Acts as a **bridge between the kernel and various filesystem types/hardware**.
* Ensures applications can **open, read, write, and manage files** seamlessly, regardless of the underlying filesystem.

### 📊 Diagram: Linux VFS Abstraction Layer

<div align="center">
  <img src="./images/02.png" width="450"/>
</div>

**Layers Explained:**

1. **Kernel** → Core part of Linux OS.
2. **Virtual File System (VFS)** → Middleware that unifies filesystem operations.
3. **Supported Filesystems** → Ext3, Ext4, XFS, ZFS, btrfs, NTFS, FAT, APFS, NFS, SMB…
4. **Hardware** → Physical storage like HDDs, SSDs, Tapes.

---

## ⚙️ Core Filesystem Functions

* **Namespace Provisioning** → Directory structures & hierarchy.
* **Metadata Management** → File size, timestamps, permissions.
* **Disk Block Allocation** → Organizing how files occupy storage blocks.
* **Access Control** → Defines rules for who can access files.
* **API Support** → System calls (open, read, write, delete, search).
* **Snapshots & Volume Management** (in advanced FS like ZFS/btrfs).

---

# 💽 **Understanding Disks and Partitions** in Linux

Understanding **disks and partitions** is a key skill for every **system administrator**. Formatting and partitioning are critical from the very beginning (system installation) and remain essential for storage management.

---

## 📀 Common Disk Types

A **disk** is a hardware component that stores data. It comes in different types and uses different interfaces:

* **HDD (Hard Disk Drive)** → Traditional spinning disks.
* **SSD (Solid-State Drive)** → Faster, uses flash memory.
* **NVMe (Non-Volatile Memory Express)** → Extremely fast, RAM-like storage with low latency.

### 🔌 Interfaces

* **IDE (Integrated Drive Electronics)** → Old, low transfer rate, now deprecated.
* **SATA (Serial ATA)** → Replaced IDE, supports up to **16 GB/s transfer rate**.
* **SCSI (Small Computer Systems Interface)** → Used in enterprise servers, often with RAID.
* **SAS (Serial Attached SCSI)** → High-reliability enterprise interface, similar speed to SATA.
* **USB (Universal Serial Bus)** → Used for external drives & flash drives.

---

## 📏 Disk Geometry

Each disk has:

* **Heads**
* **Cylinders**
* **Tracks**
* **Sectors**

In Linux, you can view this geometry using:

```bash
sudo fdisk -l
```

---

## 🔎 Command Example

We ran `fdisk -l` on a Debian 12 system with a **50 GB SSD** and a **USB device** inserted.

### 📜 Command Used

```bash
sudo fdisk -l
```

* `sudo` → Run with root privileges.
* `fdisk` → Tool to manage disk partitions.
* `-l` → List all disks and partitions.

---

## 📊 Output Explained

Here’s the actual output (abridged):

```text
Disk /dev/loop0: 4 KiB, 4096 bytes, 8 sectors
...
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 4B260F40-3DAC-4B10-8131-933A12162613

Device     Start       End   Sectors Size Type
/dev/sda1   2048      4095      2048   1M BIOS boot
/dev/sda2   4096 104855551 104851456  50G Linux filesystem
```

---

### 🔹 Loop Devices (`/dev/loopX`)

Example:

```text
Disk /dev/loop0: 4 KiB, 4096 bytes, 8 sectors
```

* **Loop devices** are virtual devices.
* They are **files mounted as block devices**.
* Commonly used by **snap packages**, squashfs images, or mounting ISO files.
* Each `/dev/loopX` is a separate virtual device.

---

### 🔹 Main Disk (`/dev/sda`)

```text
Disk /dev/sda: 50 GiB, 53687091200 bytes, 104857600 sectors
Disk model: VBOX HARDDISK
Disklabel type: gpt
Disk identifier: 4B260F40-3DAC-4B10-8131-933A12162613
```

* **Device name:** `/dev/sda` → first detected hard drive.
* **Size:** 50 GiB (VirtualBox hard disk).
* **Sectors:** 512 bytes each.
* **Partition Table (Disklabel type):** GPT (GUID Partition Table).
* **Disk Identifier:** Unique UUID for this disk.

---

### 🔹 Partitions on `/dev/sda`

| Device      | Start | End       | Sectors   | Size | Type             |
| ----------- | ----- | --------- | --------- | ---- | ---------------- |
| `/dev/sda1` | 2048  | 4095      | 2048      | 1M   | BIOS boot        |
| `/dev/sda2` | 4096  | 104855551 | 104851456 | 50G  | Linux filesystem |

* **`/dev/sda1` (1 MB)** → BIOS boot partition (used when booting in legacy BIOS mode with GPT).
* **`/dev/sda2` (50 GB)** → Main Linux filesystem partition.

---

### 🔹 Example of Loop Devices Sizes

* `/dev/loop1` → 73.91 MiB
* `/dev/loop3` → 245.13 MiB
* `/dev/loop5` → 516.01 MiB
* `/dev/loop11` → 246.43 MiB

👉 These correspond to **mounted squashfs images** (commonly from Snap packages in Ubuntu/Debian).

---

# 📀 **Partitioning Disks in Linux**

Disks are rarely used as a single block of storage — instead, they are divided into **partitions**. Partitioning allows better organization, multiple operating systems, and efficient disk usage.

---

## 🧩 Understanding Partitions

* **Partitions** are **contiguous sets of sectors and/or cylinders** on a disk.
* They help divide a single physical disk into multiple logical sections.
* Even with SSDs, this legacy concept remains relevant.

### 🔑 Partition Types

1. **Primary Partitions**

   * Maximum of **4 primary partitions**.
   * Can directly hold data or an OS.

2. **Extended Partitions**

   * Only **1 extended partition** is allowed per disk.
   * It acts like a container to hold **logical partitions**.

3. **Logical Partitions**

   * Created inside the extended partition.
   * Allow up to a **total of 15 partitions** per disk.

---

## 🗂️ MBR vs GPT Partitioning

### 🔹 Master Boot Record (MBR)

* Used until around **2010**.
* Limitations:

  * Only **4 primary partitions**.
  * Max partition size: **2 TB**.
* Stores partition table in the first **512 bytes** of disk.
* Uses **hexadecimal codes** for partition types:

  * `0x0c` → FAT
  * `0x07` → NTFS
  * `0x83` → Linux filesystem
  * `0x82` → Swap

### 🔹 GUID Partition Table (GPT)

* Part of **UEFI standard**.
* Solves MBR’s limitations:

  * Supports **128 partitions**.
  * Supports disks up to **75.6 Zettabytes (ZB)**.
  * Better redundancy (multiple copies of partition table).
* Modern systems use GPT.

---

## 📑 Partition Table Layout

The **MBR** (first 512 bytes of a disk) is structured as:

| Section              | Size      | Purpose                                       |
| -------------------- | --------- | --------------------------------------------- |
| Bootloader code      | 446 bytes | Loads the OS (e.g., **GRUB** in Linux).       |
| Partition table      | 64 bytes  | Stores details of up to 4 primary partitions. |
| End of sector marker | 2 bytes   | Identifies end of MBR.                        |

Each **partition entry** (16 bytes) includes:

* Start Cylinder/Head/Sector (CHS)
* Partition type code
* End CHS
* Starting sector (LBA)
* Number of sectors

---

## 🏷️ Naming Partitions in Linux

Linux uses **device nodes** in `/dev` to represent disks and partitions.

### Convention:

* First hard drive → `/dev/sda`
* Second hard drive → `/dev/sdb`
* Third hard drive → `/dev/sdc`, and so on.
* Partitions are numbered:

  * First partition on first disk → `/dev/sda1`
  * Second partition on first disk → `/dev/sda2`
  * First partition on second disk → `/dev/sdb1`

```bash
Disk: /dev/sda (500GB)
 ├── /dev/sda1  → Linux OS (Primary)
 ├── /dev/sda2  → Windows OS (Primary)
 ├── /dev/sda3  → Data Storage (Primary)
 ├── /dev/sda4  → Extended Partition
        ├── /dev/sda5 → Movies (Logical)
        ├── /dev/sda6 → Backup (Logical)
        └── /dev/sda7 → Extra Files (Logical)
```

👉 Note: This applies to **SCSI and SATA devices**. Letters (`a`, `b`, `c`) are assigned based on device **ID**, not physical order.

---

## ⚙️ Checking Partition Attributes

To inspect partitions, use the **`lsblk` command**.

### 🔹 Command:

```bash
lsblk
```

### 📖 Explanation:

* `lsblk` → Lists information about all block devices (disks, partitions, loop devices, etc.).
* Columns:

  * **NAME** → Device name (`sda`, `sda1`, etc.).
  * **MAJ\:MIN** → Major and minor device numbers (kernel identifiers).
  * **RM** → Removable device? (`1` for removable, `0` for fixed).
  * **SIZE** → Size of the disk or partition.
  * **RO** → Read-only status (`1` = read-only).
  * **TYPE** → Device type (`disk`, `part`, `loop`, `rom`).
  * **MOUNTPOINTS** → Where it is mounted in the filesystem.

---

## 📊 Sample Output Breakdown

```bash
hashim@hashim-VirtualBox:~$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0  73.9M  1 loop /snap/core22/2111
loop1    7:1    0     4K  1 loop /snap/bare/5
loop2    7:2    0  73.9M  1 loop /snap/core22/2045
loop3    7:3    0 245.1M  1 loop /snap/firefox/6565
loop4    7:4    0 246.4M  1 loop /snap/firefox/6738
loop5    7:5    0   516M  1 loop /snap/gnome-42-2204/202
loop6    7:6    0  11.1M  1 loop /snap/firmware-updater/167
loop7    7:7    0  91.7M  1 loop /snap/gtk-common-themes/1535
loop8    7:8    0  10.8M  1 loop /snap/snap-store/1270
loop9    7:9    0  49.3M  1 loop /snap/snapd/24792
loop10   7:10   0  50.8M  1 loop /snap/snapd/25202
loop11   7:11   0   576K  1 loop /snap/snapd-desktop-integration/315
sda      8:0    0    50G  0 disk 
├─sda1   8:1    0     1M  0 part 
└─sda2   8:2    0    50G  0 part /
sr0     11:0    1  50.7M  0 rom  /media/hashim/VBox_GAs_7.2.0
```

### 🔎 Analysis:

* **loop devices (`loop0` → `loop11`)**:
  Virtual devices created by the **snap package system** (read-only).

* **sda (50G, disk)**:
  Main virtual hard disk of the system.

  * **sda1 (1M)**: Tiny partition, often used for boot/grub or alignment.
  * **sda2 (50G, mounted at `/`)**: Root filesystem (Linux OS).

* **sr0 (ROM, 50.7M)**:
  Represents the **virtual CD-ROM drive**, currently mounted at `/media/hashim/VBox_GAs_7.2.0`.

---

## 🚀 Summary

* Disks are divided into **partitions** for organization.
* **MBR** (old) vs **GPT** (modern) define how partitions are stored.
* Linux names disks like `/dev/sda`, `/dev/sdb`, etc.
* `lsblk` helps visualize partitions and mount points.
* Bootloader (**GRUB**) uses the partition table to find the active partition and boot the OS.

---

