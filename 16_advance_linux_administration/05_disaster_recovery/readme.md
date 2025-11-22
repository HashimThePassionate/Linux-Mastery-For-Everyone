# 🌩️ Planning for Disaster Recovery & Risk Management

Managing risks is a vital asset for every business and individual. For system administrators, this responsibility is immense. Managing risks should be an integral part of a broader **risk management strategy**.

IT footprints within companies have grown exponentially over the last decade. Today, almost every activity—whether in small businesses, large corporations, government agencies, or the health and education sectors—relies on IT operations. Because every activity is unique, it requires a specific type of assessment. However, in information security, risk management has often become a "one-size-fits-all" practice based on checklists.


## 🛡️ A Brief Introduction to Risk Management

**What is risk management?**
In a nutshell, it comprises specific operations set to mitigate any possible threat that could impact the overall continuity of a business. The risk management process is crucial for every IT department.

### 📜 Origins and Standards
Risk management frameworks initially emerged in the United States following the **Federal Information Systems Modernization Act (FISMA)** laws, which began in 2002. During this time, the **National Institute of Standards and Technology (NIST)** started creating new standards and methods for cybersecurity assessments across US government agencies.


Security certifications and compliances are paramount for every Linux distribution provider competing in the corporate and governmental space. Major distributions like **Red Hat**, **SUSE**, and **Canonical** hold certifications from:
* 🇺🇸 **NIST** (USA)
* 🇬🇧 **NCSC** (National Cyber Security Centre - UK)
* 🇷🇺 **FSTEC** (Federal Service for Technic and Export Control - Russia)

### 🏗️ The Risk Management Framework (NIST SP 800-37r2)
According to NIST, the framework consists of seven steps, ranging from preparation to daily monitoring. In summary, the framework is built upon several important branches:

* **📋 Inventory:** A thorough inventory of all on-premises systems and a complete list of software solutions.
* **🏷️ System Categorization:** Assessing the impact level for each data type regarding availability, integrity, and confidentiality.
* **🔒 Security Control:** Detailed procedures regarding the security of hundreds of computer systems (NIST SP 800-53r4).
* **🔍 Risk Assessment:** A series of steps covering threat source identification, vulnerability identification, impact determination, information sharing, risk monitoring, and periodic updates.
* **📝 System Security Plan:** A report based on every security control, detailing how future actions are assessed, implemented, and their effectiveness.
* **✅ Certification, Accreditation, Assessment, & Authorization:** The process of reviewing security assessments, highlighting issues, and detailing effective resolutions in a future plan of action.
* **🛠️ Plan of Action:** A tool used to track security weaknesses and apply the correct response procedures.


## ⚠️ Types of IT Risks

Risks in information technology vary widely, ranging from physical hazards to malicious acts:

* **Hardware Failure:** Equipment breakdown or malfunction.
* **Software Errors:** Bugs, glitches, and compatibility issues.
* **Spam and Viruses:** Malicious software and unwanted communications.
* **Human Error:** Mistakes made by users or administrators.
* **Natural Disasters:** 🔥 Fires, 🌊 floods, 🌍 earthquakes, 🌀 hurricanes, etc.
* **Criminal Activities:** Security breaches, employee dishonesty, corporate espionage, and cybercrime.


## 🔄 The Risk Management Strategy: 5 Distinct Steps

To address these risks effectively, an appropriate management strategy must be implemented. This strategy generally follows five key steps:

### 1. 🕵️ Identifying Risk
Identify possible threats and vulnerabilities that could impact your ongoing IT operations.

### 2. 📊 Analyzing Risk
Decide the magnitude of the risk (how big or small it is) based on thorough studies and data.

### 3. ⚖️ Evaluating Risk
Evaluate the potential impact on operations. The immediate action is to respond based on this impact assessment. This requires real actions to be performed at every level of operations.

### 4. 🛡️ Responding to Risk
Activate your **Disaster Recovery Plans (DRPs)**, combined with strategies for prevention and mitigation.

### 5. 📡 Monitoring and Reviewing Risk
Trigger a drastic monitoring and reviewing strategy. This ensures that all IT teams:
* Know how to respond to the risk.
* Have the necessary tools and abilities to isolate the risk.
* Can enforce and strengthen the company’s infrastructure.

> **Note:** Risk assessment is extremely important to any business and should be taken very seriously by IT management.

---

# 📉 Risk Calculation & Assessment

Risk assessment (also known as risk calculation or risk analysis) is the process of identifying, evaluating, and estimating the levels of risks involved in a situation. It focuses on finding and calculating solutions to potential threats and vulnerabilities.


## 🧮 Basic Terms for Risk Impact

To effectively discuss and calculate risk, you must understand the following key metrics.

### Financial Metrics
* **Annual Loss Expectancy (ALE):**
    * Defines the total monetary loss expected over the course of **1 year**.
    * **Formula:** `SLE x ARO = ALE`
* **Single Loss Expectancy (SLE):**
    * Represents the monetary loss expected from a **single** risky event at any given time.
* **Annual Rate of Occurrence (ARO):**
    * Represents the likelihood (probability) of a risky event occurring within a **1-year** period.

> **The Golden Formula:**
> $$SLE \times ARO = ALE$$
> Since each element provides a monetary value or rate, the final result (ALE) is expressed as a monetary value. This formula is essential for budgeting security measures.

### Operational & Recovery Metrics
* **Mean Time Between Failures (MTBF):**
    * Measures the average time elapsed between anticipated and reparable failures of a system.
* **Mean Time To Failure (MTTF):**
    * Measures the average time a system operates before experiencing an **irreparable** failure (total death of the component).
* **Mean Time To Restore (MTTR):**
    * Measures the average time needed to repair a failed system and get it back online.
* **Recovery Time Objective (RTO):**
    * Represents the **maximum** acceptable amount of time allocated for downtime.
* **Recovery Point Objective (RPO):**
    * Defines the specific point in time to which data must be restored (i.e., how much data loss is acceptable).


## 🛡️ Risk Assessment Strategies

Risk assessment relies on two major categories of action: Proactive (preventative) and Non-active (reactive/passive).

### 1. Proactive Actions
These are steps taken *before* an incident occurs.

* **Risk Avoidance:**
    * Based on identifying a risk and implementing a quick solution to completely avoid its occurrence.
* **Risk Mitigation:**
    * Based on taking specific actions to **reduce** the likelihood or impact of a possible risk.
* **Risk Transference:**
    * Transferring the potential outcome (and financial burden) of the risk to an external entity, such as an insurance company.
* **Risk Deterrence:**
    * Based on implementing systems and policies (like warning banners or security cameras) that discourage attackers from attempting to exploit the system.

### 2. Non-Active Actions
These are decisions made regarding risks that cannot or will not be mitigated.

* **Risk Acceptance:**
    * Accepting the risk without action. This is chosen if the cost of proactive measures (mitigation) exceeds the cost of the potential harm caused by the risk.


## ☁️ Risk in Cloud Computing

The strategies above apply to generic, on-premises computing. However, cloud computing is dominating the modern IT landscape.

**How does risk apply to the cloud?**
In cloud computing, you use a third party's infrastructure to store and process your own data. This is effectively **outsourcing**. Consequently, risks that were previously internal threats in an on-premises environment are now transferred to third-party providers (like Amazon, Microsoft, or Google).

### The Three Major Cloud Paradigms

Different cloud models shift different levels of responsibility (and risk) from the customer to the provider.

#### 1. Software as a Service (SaaS)
* **Definition:** A software solution for companies aimed at reducing IT maintenance costs by relying on subscriptions.
* **Examples:** Slack, Microsoft 365, Google Apps, Dropbox.
* **Risk Profile:** The provider manages almost everything; user responsibility is minimal.

#### 2. Platform as a Service (PaaS)
* **Definition:** A platform that allows you to deploy software applications to clients using another entity's infrastructure, runtimes, and dependencies. This can exist on public, private, or hybrid clouds.
* **Examples:** Microsoft Azure, AWS Lambda, Google App Engine, SAP Cloud Platform, Heroku, Red Hat OpenShift.
* **Risk Profile:** You manage the application and data; the provider manages the runtime and infrastructure.

#### 3. Infrastructure as a Service (IaaS)
* **Definition:** Online services that provide high-level APIs to access physical computing resources (servers, networking).
* **Notable Example:** OpenStack.
* **Risk Profile:** You manage the OS, applications, and data; the provider manages the physical hardware.

### Managing Cloud Risks
While many infrastructure risks are transferred to the third party, new risks emerge:
* **Data Integration:** Ensuring data flows correctly between systems.
* **Compatibility:** Ensuring legacy systems work with cloud environments.

**Comparison of Risk Challenges:**
* **On-Premises:** You manage all components in-house. Risk assessment is **challenging** because you own every risk.
* **Cloud (IaaS/PaaS/SaaS):** Risk assessment becomes **less challenging** as responsibilities are gradually transferred to the external entity.

---
# 📝 Designing a Disaster Recovery Plan (DRP)

A **Disaster Recovery Plan (DRP)** is a structured document outlining the specific steps to take when an incident occurs. It is usually a subset of a broader **Business Continuity Plan (BCP)**, which ensures a company keeps operating despite infrastructure failures.



[Image of Disaster Recovery Plan Cycle]


## 🏗️ The Pillars of a Good DRP

To build an effective DRP, you must start with a solid foundation of knowledge about your current environment.

### 1. Comprehensive Inventory
You cannot protect what you don't know exists. A DRP starts with three accurate inventories:
* **Hardware Inventory:** Servers, storage devices, network gear, user devices.
* **Software Inventory:** OS versions, applications, licenses, dependencies.
* **Data Inventory:** Where data lives, who owns it, and how critical it is.

### 2. Standardization Policy
There should be a clear policy for **standardized hardware**.
* **Benefits:** Faulty hardware is easier to replace, drivers are better supported (crucial in Linux), and optimization is easier.
* **Trade-off:** This limits **BYOD (Bring Your Own Device)** practices. Employees must use company-provided hardware to ensure the IT department has full control over the software environment and security configurations.

### 3. Defining Responsibilities & Communication
* **IT Role:** The IT department designs recovery strategies based on **RPO** (Recovery Point Objective - max data loss) and **RTO** (Recovery Time Objective - max downtime).
* **Role Assignment:** Everyone must know exactly what to do during a crisis to reduce response time.
* **Communication Strategy:** Clear procedures for every organizational level ensure centralized decision-making. There must be a **succession plan** for backup personnel (if the lead admin is unavailable, who takes over?).

> **Critical Step:** DRPs must be tested **at least twice a year**. A plan that hasn't been tested is just a theory.

---

# 💾 Backing Up and Restoring the System

Disasters can strike at any time. Backing up your system is the ultimate safety net. It is always better to practice prevention than to learn the hard way through data loss.

Your strategy must be driven by your RTO and RPO requirements:
* **RTO (How fast?):** How quickly must we be back online to avoid hurting business operations?
* **RPO (How much?):** How much data can we afford to lose? (e.g., 1 hour's worth? 1 day's worth?)

## 🗂️ Types of Backups

Understanding the difference between these types is critical for balancing storage space vs. recovery speed.



| Backup Type | What is Backed Up | Pros | Cons |
| :--- | :--- | :--- | :--- |
| **Full Backup** | **All files** in the destination target. | Easiest to restore (you only need one file). | Slowest to run; takes the most storage space. |
| **Incremental Backup** | Only files changed since the **last backup** (whether it was full or incremental). | Fastest to run; uses the least storage. | Slowest to restore (must restore Full + all Incrementals in order). |
| **Differential Backup** | Only files changed since the **previous FULL backup**. | Faster to restore than Incremental (Full + last Differential). | Uses more storage than Incremental; slower to run as time goes on. |

## 🛠️ Methods of Backup

| Method | Description |
| :--- | :--- |
| **Manual Backup** | User-initiated (e.g., dragging files to a USB). Unreliable because it depends on human memory. |
| **Local Automated** | Scheduled scripts/software writing to local disks or tapes. Fast recovery but vulnerable to local disasters (fire/flood). |
| **Remote Automated** | Scheduled backups sent over the network to a remote server or cloud. Slower recovery but protects against local site disasters. |

---

# 🛡️ The Golden Rule: The 3-2-1 Backup Strategy

This is the industry standard for data protection. If you take away one thing from this section, let it be this rule.



### 3️⃣ Copies of Data
You should have at least **three** copies of your data:
1.  The primary (production) data.
2.  Backup copy #1.
3.  Backup copy #2.

### 2️⃣ Different Media
Store the copies on **two** different types of media.
* *Example:* One copy on an internal hard drive, one copy on an external tape drive or NAS (Network Attached Storage). This protects against a specific type of media failing (e.g., a batch of bad hard drives).

### 1️⃣ Off-Site
Keep at least **one** copy **off-site** (at a different geographical location).
* *Why?* If your office burns down or gets flooded, your local backups will be destroyed along with your servers. Cloud storage is a common solution for this today.

> **Note:** This rule can be adapted (e.g., 3-1-2, 3-2-2), but the core principle of redundancy and geographic separation remains checking **Backup Integrity** is also vital—a backup is worthless if it is corrupted and cannot be restored.

---

# 💾 Disk Cloning Solutions

Sometimes, the best backup is an exact replica of your hard drive. This is called **disk cloning**. It's excellent for disaster recovery because you don't just get your files back; you get your entire operating system, installed programs, and configurations exactly as they were.

Linux offers powerful tools for this job. Let's look at three of them: `dd`, `ddrescue`, and `Relax-and-Recover (ReaR)`.

-----

## 1\. The `dd` Command (The "Data Duplicator")

`dd` is a classic, powerful, and dangerous tool. It copies data **block by block**, meaning it copies every single bit from the source to the destination, regardless of whether it's a file or empty space. It doesn't care about the filesystem type (ext4, XFS, NTFS)—it just copies raw data.

### 📝 How to Clone an Entire Disk

**Scenario:** You have a 20 GB virtual disk (`/dev/vda`) and you want to clone it to a 128 GB USB drive (`/dev/sda`).

**Step 1: Verify Disk Sizes**
Always check that your destination is larger than your source.

```bash
sudo fdisk -l
```

**Step 2: Perform the Clone**

```bash
sudo dd if=/dev/vda of=/dev/sda conv=noerror,sync status=progress
```

**Explanation of Options:**

  * `if=/dev/vda`: **I**nput **F**ile (source drive).
  * `of=/dev/sda`: **O**utput **F**ile (destination USB drive).
  * `conv=noerror`: Continue copying even if read errors occur.
  * `sync`: If an error occurs, fill the missing data block with zeros to keep everything aligned (synced).
  * `status=progress`: Shows a progress bar and statistics so you know it's working.

> **Warning:** `dd` will overwrite the destination without asking "Are you sure?". Triple-check your `of=` device\!

-----

## 2\. The `ddrescue` Command (The Data Saver)

If your hard drive is failing or has bad sectors, `dd` might fail or get stuck. This is where `ddrescue` shines. It attempts to rescue data from a failing drive.

**How it works:** It tries to copy the easy, healthy parts first. Then, it goes back and tries to read the bad sectors repeatedly to squeeze out any readable data.

**Installation (Ubuntu):**

```bash
sudo apt install gddrescue
```

**Cloning with `ddrescue`:**

```bash
sudo ddrescue -n /dev/vda /dev/sda rescue.map --force
```

**Explanation of Options:**

  * `-n`: Skip the "scraping" phase (trying to read bad areas) initially to get the good data fast.
  * `/dev/vda`: Source.
  * `/dev/sda`: Destination.
  * `rescue.map`: A "mapfile" (log) that records which blocks are good and which are bad. If the process stops, you can resume later using this mapfile\!
  * `--force`: Overwrite the destination.

-----

## 3\. Using Relax-and-Recover (ReaR)

**ReaR** is an enterprise-grade disaster recovery tool. Unlike `dd`, which just copies data, ReaR creates a **bootable recovery image**.

If your system dies, you boot from the ReaR image. It will automatically recreate your partition layout, format the filesystems, and restore your data from the backup.

**Installation (Ubuntu):**

```bash
sudo apt install rear
```

**Configuration File:** `/etc/rear/local.conf`

### 🅰️ Scenario A: Backing up to a Local NFS Server

**Goal:** Back up your local machine ("client") to a central storage server ("NFS Server").

**Step 1: Configure the NFS Server**

1.  Create a directory for backups:
    ```bash
    sudo mkdir -p /home/export/rear
    ```
2.  Change ownership (ReaR usually runs as root but writes as `nobody` over NFS):
    ```bash
    sudo chown -R nobody:nogroup /home/export/rear/
    ```
3.  Edit `/etc/exports` to share this directory:
    ```bash
    /home/export/rear 192.168.124.0/24(rw,sync,no_subtree_check)
    ```
4.  Restart NFS:
    ```bash
    sudo systemctl restart nfs-kernel-server
    ```

**Step 2: Configure the Client (The machine to back up)**
Edit `/etc/rear/local.conf`:

```bash
OUTPUT=ISO
OUTPUT_URL=nfs://192.168.124.112/home/export/rear
BACKUP=NETFS
BACKUP_URL=nfs://192.168.124.112/home/export/rear
```

  * `OUTPUT=ISO`: Create a bootable ISO image.
  * `BACKUP=NETFS`: Use the internal backup tool to save files as a `tar.gz` archive.
  * `*_URL`: Point both the ISO and the backup archive to the NFS server.

**Step 3: Run the Backup**

```bash
sudo rear -v -d mkbackup
```

  * `-v`: Verbose (show details).
  * `-d`: Debug (show even more details).
  * `mkbackup`: The command to creating the rescue system AND back up the files.

**Result:** On the NFS server, you will see `rear-hostname.iso` (the boot disk) and `backup.tar.gz` (your data).

-----

### 🅱️ Scenario B: Backing up directly to USB

**Goal:** Create a self-contained USB recovery stick.

**Step 1: Format the USB**
ReaR has a built-in command to format the USB drive properly for booting.

```bash
sudo rear format /dev/sda
```

*(Type `Yes` when prompted).*

**Step 2: Configure `/etc/rear/local.conf`**

```bash
OUTPUT=USB
BACKUP_URL="usb:///dev/disk/by-label/REAR-000"
```

  * `OUTPUT=USB`: Make the USB drive bootable.
  * `BACKUP_URL`: Save the backup data (`tar.gz`) onto the partition labeled `REAR-000` (which was created by the format command).

**Step 3: Run the Backup**

```bash
sudo rear -v mkbackup
```

**Step 4: How to Recover**

1.  Plug the USB into the crashed computer.
2.  Boot from the USB.
3.  Select **"Recover [hostname]"** from the menu.
4.  ReaR handles the rest\!

---

# 🛠️ Introduction: Linux Troubleshooting

One of Linux's greatest strengths is its "openness." Because the system is open, there are many tools available to look under the hood and see exactly what is going on.

**Troubleshooting** is simply the process of solving problems based on data (diagnostics) gathered by these tools. To make this easy to digest, we will divide the problems into four categories:

1.  **Boot Issues:** The computer won't start.
2.  **General System Issues:** Problems with storage, RAM, or the CPU being too busy.
3.  **Network Issues:** Connection problems.
4.  **Hardware Issues:** Physical component failures.

-----

## 1\. Tools for Troubleshooting Boot Issues

To fix a computer that won't start, you first need to understand the sequence of events that happens when you press the power button.

### The Linux Boot Process

There are 4 main stages:

1.  **BIOS POST:** The computer hardware checks itself (Power-On Self-Test). If this fails, it's a hardware issue, not a Linux issue. The BIOS looks for a bootable disk.
2.  **GRUB2 Initialization:** This is the **Bootloader**. It’s the menu where you choose your OS. It loads the Linux Kernel into memory.
3.  **Kernel Initialization:** The Kernel is the "brain" of the OS. It extracts itself, takes control of the hardware, and starts the first process.
4.  **systemd Initialization:** This is the "Init system." It mounts your hard drives and starts background services (like the graphical interface).

### How to Repair GRUB2

If the bootloader (GRUB2) breaks, Linux won't start. To fix this, you need a **Live USB drive** (like an Ubuntu installation stick).

**The Steps:**

1.  Insert the Live USB and boot from it (select "Try Ubuntu").
2.  Open a terminal.
3.  Find your main Linux partition using `sudo fdisk -l`.
4.  **Mount** that partition so you can access it. (Let's assume your Linux is on `/dev/sda1`):
    ```bash
    sudo mount -t ext4 /dev/sda1 /mnt
    ```
5.  **Reinstall GRUB** onto the disk. We use `chroot` to pretend the USB terminal is actually inside your hard drive's system:
    ```bash
    sudo chroot /mnt
    grub-install /dev/sda
    grub-install –recheck /dev/sda
    update-grub
    ```
6.  **Exit and Reboot:**
    ```bash
    exit
    sudo unmount /mnt
    # Reboot the computer
    ```

-----

## 2\. General System Issues

These issues usually involve running out of space, running out of memory (RAM), or the computer running slowly (System Load).

### A. Disk-Related Issues (Space)

Hard drives (HDDs or SSDs) store your data. If they fill up, the system crashes.

**Command: `df` (Disk Free)**
This shows you how much space is used on your drives.

  * `-h`: Makes the output "Human-readable" (e.g., shows "GB" instead of a long number).

<!-- end list -->

```bash
df -h
```

*(If a disk is at 100%, you need to delete files).*

**Command: `du` (Disk Usage)**
This helps you find *which* folders are taking up the most space. The command below finds the top 5 largest directories in `/home`.

```bash
sudo du -h /home | sort -rh | head -5
```

**Output:**

```text
181M    /home/packt
18M     /home
16K     /home/alex
12K     /home/packt/reports
12K     /home/packt/.local
```

**The "Inode" Problem**
Sometimes you have plenty of space (GBs free), but you can't create new files. This happens if you run out of **Inodes**. An Inode is like an index card for a file. If you have millions of tiny files, you run out of cards.

**Command:**

```bash
df -i
```

**Output:**

```text
| Filesystem                              | Inodes   | IUsed   | IFree    | IUse% | Mounted on      |
|-----------------------------------------|----------|---------|----------|-------|-----------------|
| tmpfs                                   | 252890   | 810     | 252080   | 1%    | /run            |
| /dev/mapper/ubuntu--vg-ubuntu--lv       | 655360   | 120212  | 535148   | 19%   | /               |
| tmpfs                                   | 252890   | 1       | 252889   | 1%    | /dev/shm        |
| tmpfs                                   | 252890   | 3       | 252887   | 1%    | /run/lock       |
| tmpfs                                   | 252890   | 3       | 252887   | 1%    | /run/user/1000  |
| tmpfs                                   | 50578    | 25      | 50553    | 1%    | /run/user/1000  |
```

-----

### B. Memory (RAM) Issues

If RAM fills up, the system slows down or apps crash.

**Command: `free`**
Shows total, used, and free RAM.

  * `-h`: Human-readable.

<!-- end list -->

```bash
free -h
```

**Output:**

```text
              total        used        free      shared  buff/cache   available
Mem:          1.9Gi       251Mi       745Mi       1.0Mi       978Mi       1.5Gi
Swap:         1.8Gi       0.0Ki       1.8Gi
```

  * **buff/cache:** Linux uses free RAM to store recently accessed files to speed things up. This is good.
  * **available:** This is the real number indicating how much RAM is ready for new apps.

**Command: `top`**
This is a real-time dashboard showing running processes and memory usage.

```bash
# The command usually runs interactively. This is a snapshot of the memory section:
PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
4157 root      20   0       0      0      0 I   0.0   0.0   0:00.15 kworker/0:2-events
   1 root      20   0  167740  13212   8256 S   0.0   0.3   0:01.74 systemd
...
```

**Command: `vmstat` (Virtual Memory Statistics)**
Shows memory, swap, I/O, and CPU activity in one line.

```bash
vmstat
```

**Output:**

```text
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 1  0    268 763164 552328 450412    0    0  1616  1418  511 1045  0  1 85 14  0
```

  * **si/so (Swap In/Out):** If these numbers are high, your RAM is full and the computer is using the hard drive as fake RAM (which is very slow).

**Command: `sar` (System Activity Report)**
This tool records system history. You usually need to install it (`sudo apt install sysstat`) and enable the service (`sysstat`). It helps you see what happened in the past.

To check memory usage every 2 seconds, 5 times:

```bash
sudo sar -r 2 5
```

**Output:**

```text
07:52:29 PM kbmemfree   kbavail   kbmemused  %memused  ...
07:52:31 PM    820640   1564124      219720     10.86  ...
...
Average:       820640   1564124      219726     10.86  ...
```

-----

### C. System Load Issues

System Load is a measurement of how "busy" the computer is.

**Command: `uptime`**

```bash
uptime
```

**Output:**

```text
20:01:48 up 1:53,  2 users,  load average: 0.00, 0.00, 0.00
```

  * **Load Average:** The three numbers represent the load for the last 1, 5, and 15 minutes.
  * If you have 1 CPU core, a load of 1.00 means it is 100% busy. If the load is 5.00, processes are waiting in line to be processed.

**Command: `top` (Batch Mode)**
Running `top` normally updates constantly. Using `-b` allows you to save a snapshot to a file.

```bash
top -b -n 1 | tee top-command-output
```

**Output:**

```text
top - 15:20:30 up 3 min,  2 users,  load average: 0.21, 0.17, 0.07
Tasks: 154 total,   1 running, 153 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
...
```

  * **%Cpu(s):**
      * **us:** User time (apps).
      * **sy:** System time (kernel).
      * **id:** Idle time (doing nothing).
      * **wa:** Wait time (waiting for the hard drive/IO). High `wa` means a slow disk.

**Command: `iostat`**
Used to see if the CPU is waiting on the Disk (Input/Output).

```bash
iostat
```

**Output:**

```text
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.10    0.01    0.82    11.40    0.01    87.67

Device       tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read      kB_wrtn      kB_dscd
dm-0         8.14       78.94        39.11       446.01     558397       276624      3154984
loop0        0.05        0.16         0.00         0.00       1698            0            0
...
sda        155.27      616.49      2329.90         0.00    4360936     16481240            0
vda         21.32     2068.83        39.13       446.01   14634494       276780      3154984
```

**Command: `iotop`**
Requires installation (`sudo apt install iotop`). It shows specifically *which* program is reading/writing to the disk.

```bash
sudo iotop
```

**Output:**

```text
Total DISK READ:        0.00 B/s | Total DISK WRITE:        0.00 B/s
  TID  PRIO  USER      DISK READ  DISK WRITE  SWAPIN      IO>     COMMAND
    1 be/4 root         0.00 B/s    0.00 B/s  ?unavailable?  init
...
```

-----

## 3\. Tools for Troubleshooting Network Issues

Network troubleshooting is 80% of an administrator's job. We diagnose this by layers (like a cake), starting from the bottom (Physical) up to the applications.

### Layer 1: Physical (Cables & Hardware)

**Command: `ping`**
The most basic test. Sends data to a server (like Google) and waits for a reply.

```bash
ping -c 4 google.com
```

**Output:**

```text
PING google.com (142.250.181.238) 56(84) bytes of data.
64 bytes from fra16s56-in-f14.1e100.net (142.250.181.238): icmp_seq=1 ttl=51 time=34.7 ms
...
--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
```

**Command: `ip link show`**
Checks if your network card is recognized and turned on.

  * Look for **state UP**. If it says DOWN, the interface is off or unplugged.

<!-- end list -->

```bash
ip link show
```

**Output:**

```text
1: lo: <LOOPBACK,UP,LOWER_UP> ... state UNKNOWN ...
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> ... state UP ...
```

**Command: `ethtool`**
Checks the physical speed of the cable/card (e.g., 1000Mb/s).

```bash
ethtool enp1s0
```

### Layer 2: Data Link (MAC Addresses)

This layer handles connecting devices on the same local network using MAC addresses (hardware IDs).

**Command: `arp -a`**
Shows the mapping between IP addresses and MAC addresses.

```bash
arp -a
```

**Output:**

```text
fedora (192.168.124.1) at 52:54:00:4c:07:aa [ether] on enp1s0
```

**Command: `ip neighbor show`**
A modern version of `arp`.

```bash
ip neighbor show
```

**Output:**

```text
192.168.0.1      dev enp4s0  lladdr [redacted]           DELAY
192.168.124.112 dev virbr0  lladdr 52:54:00:c2:77:7f  REACHABLE
```

### Layer 3: Internet Layer (IP Addresses & Routing)

**Command: `ip route show`**
Shows how your computer sends data to the outside world.

  * Look for "default via" — this is your Gateway (Router).

**Command: `traceroute`**
Shows the path (every router/hop) your data takes to reach Google.
*(Requires `sudo apt install traceroute`)*.

```bash
traceroute google.com
```

**Output:**

```text
traceroute to google.com (142.250.181.238), 30 hops max, 60 byte packets
 1  fedora (192.168.124.1)  0.689 ms  0.641 ms  0.628 ms
 2  _gateway (192.168.0.1)  0.949 ms  0.967 ms  1.117 ms
 ...
 8  bucuresti.nxdata.br01.[redacted].ro (81.22.150.2)  6.869 ms
```

**Command: `tracepath`**
A newer tool installed by default on Ubuntu. Similar to traceroute.

```bash
tracepath google.com
```

**Output:**

```text
 1?: [LOCALHOST]                       pmtu 1500
 1:  fedora                                                0.301ms
 2:  _gateway                                              1.589ms
 ...
```

**Command: `nslookup`**
Used to test DNS (converting "google.com" to an IP address). If `ping` fails but `nslookup` works, you have a routing issue, not a DNS issue.

### Layers 4 & 5: Transport (Ports)

This deals with TCP and UDP connections.

**Command: `ss` (Socket Statistics)**
Shows all open network connections on your computer.

  * `-t`: Show TCP sockets.
  * `-u`: Show UDP sockets.
  * `-l`: Show only Listening sockets (servers waiting for connections).

<!-- end list -->

```bash
ss -t
```

**Output:**

```text
Netid State       Recv-Q Send-Q Local Address:Port    Peer Address:Port    Process
udp   UNCONN      0      0      127.0.0.53%lo:domain  0.0.0.0:*
```

**Checking `TIME_WAIT`**
Sometimes connections get stuck closing. You can check this with:

```bash
ss -o state time-wait
```

**Output:**

```text
| Netid | Recv-Q | Send-Q | Local Address:Port       | Peer Address:Port | Process                        |
|-------|--------|--------|--------------------------|-------------------|--------------------------------|
| tcp   | 0      | 0      | 192.168.0.211:59832      |                   | timer (time-wait, 8.248ms, 0)  |
```

-----

## 4\. Tools for Troubleshooting Hardware Issues

If the software is fine, maybe a physical part is broken.

**Command: `dmidecode`**
Reads hardware info directly from the BIOS. It can tell you details about your RAM (Type 17) without opening the computer case.

```bash
sudo dmidecode -t 17
```

**Other Hardware Commands:**

  * **`lspci`**: Lists devices connected to the motherboard (like Graphics Cards, Network Cards).
  * **`lsblk`**: Lists all Hard Drives and Partitions.
  * **`lscpu`**: Shows details about the Processor (CPU).

**Command: `dmesg`**
This prints the **Kernel Ring Buffer**. These are messages from the Linux kernel itself. If a hardware device fails (like a USB disconnecting or a hard drive error), it will appear here instantly.

```bash
dmesg | more
```


---