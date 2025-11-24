# 🖥️ Introduction to Virtualization on Linux

Virtualization is a method for making more efficient use of computer hardware. It is fundamentally an **abstraction layer** that allows multiple, isolated environments (Virtual Machines or VMs) to run on a single physical computer.

In this section, you will learn about:
1.  **Efficiency:** How virtualization maximizes hardware usage.
2.  **The Hypervisor:** The key software that makes it all possible.
3.  **Types of Virtualization:** Specifically, how VMs run on top of a host OS.


## 📈 Efficiency in Resource Usage

The primary goal of virtualization is efficiency. A standard physical computer often has vast resources (CPU, RAM, Storage) that go unused when running a single operating system or application. Virtualization allows us to split those resources among multiple virtual machines.

### A Real-World Example
Imagine a small server lab with two physical machines:
1.  **Machine A:** Intel i3 CPU (4 cores), 16 GB RAM.
2.  **Machine B:** Intel Pentium CPU (4 cores), 12 GB RAM.

Running a single web server on Machine A would leave most of its 4 cores and 16 GB of RAM idle. This is wasteful.

**The Virtualization Solution:**
Instead of letting those resources sit idle, we can split Machine A into **four separate VMs**.
* **VM 1:** Web Server (1 CPU core, 2 GB RAM)
* **VM 2:** Database Server (1 CPU core, 4 GB RAM)
* **VM 3:** File Server (1 CPU core, 2 GB RAM)
* **VM 4:** Testing Environment (1 CPU core, 4 GB RAM)

Now, a single physical machine acts as four distinct servers. This consolidation reduces hardware costs, electricity usage, and physical space requirements.

<div align="center">
  <img src="./images/01.png" width="400"/>
</div>

As shown in Figure 11.1, a single computer might use only a fraction of its total capacity (Load). By using Virtual Machines, we can stack multiple loads onto the same hardware, utilizing nearly all of its potential.

### 🌍 Environmental Impact
Efficiency isn't just about saving money on hardware. In data centers, virtualization drastically increases **energy efficiency**. By running fewer physical servers at higher utilization rates, organizations consume less power for both computing and cooling. This reduction in energy consumption makes virtualization a significant player in reducing the carbon footprint of the IT industry.


## 🏗️ How Virtualization Works (The Architecture)

How do multiple operating systems run on one machine without conflicting? The answer is a software layer called the **Hypervisor**.

The diagram below illustrates the architecture of virtualization running on a Host OS (Hosted Virtualization).

<div align="center">
  <img src="./images/02.png" width="500"/>
</div>

### The Layers (Bottom to Top)

1.  **HARDWARE:** This is the physical machine (CPU, RAM, Disk, Network Card). It provides the raw power.
2.  **Host OS:** This is the primary operating system installed on the hardware (e.g., Linux, Windows). It manages the hardware directly.
3.  **Hypervisor:** This is the virtualization software layer (e.g., KVM, VirtualBox, VMware Workstation). It sits on top of the Host OS. Its job is to allocate hardware resources to the Guest OSs and ensure they remain isolated from one another.
4.  **Guest OS:** These are the virtual machines (e.g., Ubuntu, Fedora, Windows Server). Each Guest OS believes it has its own dedicated hardware, but it is actually interacting with the Hypervisor.
5.  **Bins/Libs & Apps:** Inside each Guest OS, you run your standard applications and libraries, just as you would on a physical machine.

**Key Constraint:** Because the Host OS and the Hypervisor itself need resources to run, you cannot allocate 100% of the hardware to your VMs. You must reserve some CPU and RAM overhead for the host system to remain stable.

---

# 🛡️ Introduction to Hypervisors

The software layer that makes virtualization possible is called a **hypervisor**.

Think of a hypervisor as a "traffic cop" for your computer's hardware. It takes the physical resources (CPU, RAM, Disk) and divides them up, allowing multiple "virtual computers" (VMs) to use them simultaneously.

Through a process called **emulation**, the hypervisor tricks the software inside a VM into thinking it's running on its own dedicated physical hardware. This allows you to overcome the limitations of a single physical machine and use your hardware much more effectively.

### 🏗️ Types of Hypervisors

Hypervisors are generally categorized into two types based on where they sit in the system architecture:

#### Type 1: Bare-Metal Hypervisors
* **How they run:** Directly on the physical hardware (the "bare metal"). There is no underlying operating system like Windows or Linux.
* **Examples:** Citrix XenServer, VMware ESXi.
* **Best for:** Enterprise data centers where performance is critical.

#### Type 2: Hosted Hypervisors
* **How they run:** On top of an existing Host OS (like installing an app on Windows or Linux).
* **Examples:** Oracle VirtualBox, VMware Workstation/Fusion.
* **Best for:** Desktop users who want to test different OSs without rebooting.

#### The Hybrid: KVM (Kernel-based Virtual Machine)
KVM is unique. It is built into the Linux kernel, which allows it to turn Linux itself into a Type 1 hypervisor. However, because you interact with it through a full Linux OS, it also behaves like a Type 2 hypervisor. This duality gives it incredible performance *and* usability.

> **Note:** In this chapter, we will focus exclusively on **KVM** as our hypervisor of choice.

---

# 🐧 Understanding Linux KVM

A Virtual Machine (VM) is a software-based computer. To the user (and the software inside it), it looks and acts exactly like a physical machine.

* **Resource Management:** The hypervisor manages resources. It can allocate 4GB of RAM to one VM and 2 CPU cores to another. It isolates them completely—if one VM crashes, the others (and the host) are unaffected.
* **Flexibility:** You can run different Guest Operating Systems (OSes) on the same host. A Linux host can run Windows, macOS, and other Linux distros simultaneously.

### Why KVM?
While tools like VirtualBox are great for beginners, **KVM** (Kernel-based Virtual Machine) is the preferred choice for Linux professionals.
* **Performance:** Because it's part of the kernel, it has very low overhead (it's fast!).
* **Control:** It offers a comprehensive command-line interface (CLI) via `virsh`, making it perfect for automation and scripting.
* **Ecosystem:** KVM powers enterprise clouds (like AWS and Google Cloud) and integrates with tools like **GNOME Boxes** for simple desktop use.

---

# ⚙️ KVM Under the Hood: QEMU and Libvirt

KVM doesn't work alone. It relies on a team of software components to deliver a full virtualization experience.

### 1. KVM (The Engine)
KVM is the kernel module that turns the Linux kernel into a hypervisor. It allows the CPU to run guest code directly.

### 2. QEMU (The Hardware Emulator)
**QEMU** (Quick Emulator) provides the "virtual hardware." KVM handles the CPU, but QEMU emulates the disk drives, network cards, USB ports, and monitors that the VM sees.

* **Software-Assisted Virtualization:** QEMU can emulate an entirely different CPU architecture (e.g., running ARM software on an Intel x86 PC) using dynamic binary translation. This is slower but very flexible.
* **Hardware-Assisted Virtualization:** When used with KVM, QEMU skips translation and executes instructions directly on the host CPU. This is incredibly fast and is the standard for running VMs today.

### 3. Libvirt (The Manager)
**Libvirt** is the "management layer." It provides a consistent API (Application Programming Interface) and a background service (daemon) called `libvirtd`.
* It talks to KVM/QEMU so you don't have to deal with complex, low-level commands.
* **Tools:**
    * **`virsh`**: The command-line tool for managing VMs (start, stop, create, destroy).
    * **`virt-manager`**: A graphical interface (GUI) that looks like VirtualBox but uses KVM/Libvirt backend.

---

# 🆚 Software vs. Hardware Assisted Virtualization

The diagram below illustrates the critical difference in how instructions are handled in these two modes.

<div align="center">
  <img src="./images/03.png" width="600"/>
</div>

### Left Side: Software-Assisted Virtualization
In this model, the hypervisor sits between the Guest OS and the Hardware for *everything*.
* **Privileged Instructions:** (OS-level commands) are caught by the hypervisor, translated/emulated, and then sent to the hardware.
* **Unprivileged Instructions:** (User app commands) are *also* caught and translated.
* **Result:** High overhead, slower performance.

### Right Side: Hardware-Assisted Virtualization (KVM)
Modern CPUs (Intel VT-x, AMD-V) have special features that allow the Guest OS to talk directly to the CPU for most tasks.
* **Unprivileged Instructions:** (User apps) bypass the hypervisor entirely and run directly on the hardware. This is why KVM is so fast—it's running at "native" speed.
* **Privileged Instructions:** (OS-level commands that might affect the whole system) are still trapped by the hypervisor to ensure safety and isolation.
* **Result:** Better performance, less complexity, and secure isolation.

---

# 🚀 Installing the KVM Hypervisor

Installing KVM and its related tools (like QEMU and Libvirt) is a straightforward process that uses your Linux distribution's package manager.

Here is how to install it on the major distributions:

**Debian / Ubuntu**

```bash
sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager
```

**Fedora**
Fedora has a convenient group install:

```bash
sudo dnf group install --with-optional virtualization
```

**openSUSE**

```bash
sudo zypper install -t pattern kvm_server kvm_tools
sudo zypper install libvirt-daemon
```

### 🏁 Starting the Service

Once the packages are installed, you must enable and start the `libvirtd` daemon. This service runs in the background and manages your virtual machines.

```bash
sudo systemctl start libvirtd
sudo systemctl enable libvirtd
```

### ✅ Validating Your Host

Before creating VMs, it's good practice to ensure your hardware supports virtualization and everything is configured correctly.

Run the validation tool:

```bash
sudo virt-host-validate
```

**Interpreting the Output (Figure 11.4):**

  * **PASS:** Good news\! The feature is working.
  * **WARN:** A warning. The feature might work, but not optimally.
  * **FAIL:** The feature is missing or broken.

```bash
hashim@debian:~$ sudo virt-host-validate
[sudo] password for hashim: 
  QEMU: Checking for hardware virtualization                                 : PASS
  QEMU: Checking if device /dev/kvm exists                                   : PASS
  QEMU: Checking if device /dev/kvm is accessible                            : PASS
  QEMU: Checking if device /dev/vhost-net exists                             : PASS
  QEMU: Checking if device /dev/net/tun exists                               : PASS
  QEMU: Checking for cgroup 'cpu' controller support                         : PASS
  QEMU: Checking for cgroup 'cpuacct' controller support                     : PASS
  QEMU: Checking for cgroup 'cpuset' controller support                      : PASS
  QEMU: Checking for cgroup 'memory' controller support                      : PASS
  QEMU: Checking for cgroup 'devices' controller support                     : PASS
  QEMU: Checking for cgroup 'blkio' controller support                       : PASS
  QEMU: Checking for device assignment IOMMU support                         : PASS
  QEMU: Checking if IOMMU is enabled by kernel                               : PASS
  QEMU: Checking for secure guest support                                    : WARN
 (Unknown if this platform has Secure Guest support)
   LXC: Checking for Linux >= 2.6.26                                         : PASS
   LXC: Checking for namespace ipc                                           : PASS
   LXC: Checking for namespace mnt                                           : PASS
   LXC: Checking for namespace pid                                           : PASS
   LXC: Checking for namespace uts                                           : PASS
   LXC: Checking for namespace net                                           : PASS
   LXC: Checking for namespace user                                          : PASS
   LXC: Checking for cgroup 'cpu' controller support                         : PASS
   LXC: Checking for cgroup 'cpuacct' controller support                     : PASS
   LXC: Checking for cgroup 'cpuset' controller support                      : PASS
   LXC: Checking for cgroup 'memory' controller support                      : PASS
   LXC: Checking for cgroup 'devices' controller support                     : PASS
   LXC: Checking for cgroup 'freezer' controller support                     : FAIL
 (Enable 'freezer' in kernel Kconfig file or mount/enable cgroup controller in your system)
   LXC: Checking for cgroup 'blkio' controller support                       : PASS
   LXC: Checking if device /sys/fs/fuse/connections exists                   : PASS
```

In the example image, you might see a failure regarding "LXC" (Linux Containers). Since we are focusing on KVM virtualization in this section, you can safely ignore LXC errors. As long as the QEMU/KVM checks pass, you are ready to go.

# 🛠️ Working with Basic KVM Commands

Once KVM is installed, you manage it using the command line. The primary tool for this is `virsh` (Virtual Shell), along with `virt-install` for creating new machines.

## 1\. Setting Up the Network

Before creating a VM, your host needs a "bridge" network so the VMs can talk to the outside world. KVM usually creates a `default` network for this, but it might not be active.

**Check Network Status:**

```bash
sudo virsh net-list --all
```

*(The `--all` flag ensures you see inactive networks too).*

**Start the Network:**
If the default network is inactive, start it:

```bash
sudo virsh net-start default
```

**Auto-Start the Network:**
Ensure this network starts automatically every time you reboot your computer:

```bash
sudo virsh net-autostart default
```

Figure 11.5 shows the output of these commands, confirming the network is now `active` and `autostart` is set to `yes`.

## 2\. Creating a Virtual Machine (CLI)

Now we can create our first VM. We will deploy an **Ubuntu 22.04 Server** virtual machine.

**Step A: Download the ISO**
First, download the operating system installation image (ISO).

```bash
wget https://releases.ubuntu.com/22.04.2/ubuntu-22.04.2-live-server-amd64.iso
```

**Step B: Move the ISO**
For security reasons, KVM often cannot access files in your home directory. Move the ISO to the default KVM image directory:

```bash
sudo mv ubuntu-22.04.2-live-server-amd64.iso /var/lib/libvirt/images/
```

**Step C: Run `virt-install`**
This single command defines the VM's hardware and starts the installation.

```bash
sudo virt-install \
  --virt-type=kvm \
  --name ubuntu-vm1 \
  --vcpus=2 \
  --memory=2048 \
  --os-variant=ubuntufocal \
  --cdrom=/var/lib/libvirt/images/ubuntu-22.04.2-live-server-amd64.iso \
  --network=default \
  --disk size=20
```

**Command Breakdown:**

  * **`--virt-type=kvm`**: Tells the system to use the KVM hypervisor (hardware acceleration).
  * **`--name ubuntu-vm1`**: The name you will use to manage this VM later.
  * **`--vcpus=2`**: Give the VM 2 virtual CPU cores.
  * **`--memory=2048`**: Allocate 2048 MB (2 GB) of RAM.
  * **`--os-variant=ubuntufocal`**: Optimizes settings for Ubuntu 20.04/22.04. (Use `osinfo-query os` to see a full list of variants).
  * **`--cdrom=...`**: The path to the installation ISO we downloaded.
  * **`--network=default`**: Connects the VM to the bridge network we enabled earlier.
  * **`--disk size=20`**: Creates a 20 GB virtual hard drive.

Once you run this, a graphical window (`virt-viewer`) should pop up showing the Ubuntu installer running inside your new VM\!

---

# 🕹️ Basic VM Management

Managing VMs involves a few core tasks: listing them, starting them, stopping them, and connecting to them. You can do this via the GUI (`virt-manager`), but the Command Line Interface (CLI) using **`virsh`** is far more powerful and what most system administrators use.

### Listing VMs

To see what virtual machines are currently running or defined on your system, use the following command.

> **Important Note:** You must run these commands as `root` or with `sudo` to see the system-wide VMs.

```bash
sudo virsh list --all
```

  * `list`: Shows running VMs.
  * `--all`: Shows *all* VMs, including those that are shut down.

### Basic Lifecycle Commands

Here is a quick reference for controlling your VMs using `virsh`. Replace `ubuntu-vm1` with your VM's name.

| Action | Command | Description |
| :--- | :--- | :--- |
| **Start** | `sudo virsh start ubuntu-vm1` | Boots up the VM. |
| **Reboot** | `sudo virsh reboot ubuntu-vm1` | Sends a restart signal to the Guest OS (graceful reboot). |
| **Stop (Force)** | `sudo virsh destroy ubuntu-vm1` | Like pulling the power plug. Immediate shutdown. |
| **Shutdown** | `sudo virsh shutdown ubuntu-vm1` | Sends a shutdown signal (ACPI) for a graceful stop. |
| **Suspend** | `sudo virsh suspend ubuntu-vm1` | Pauses the VM in memory (freezes it). |
| **Resume** | `sudo virsh resume ubuntu-vm1` | Unpauses a suspended VM. |
| **Delete** | `sudo virsh undefine ubuntu-vm1` | Deletes the VM configuration (but keeps the disk file). |

For a full list of options, you can always check the manual: `man virsh`.


# 🚀 Advanced KVM Management

Managing VMs goes beyond just turning them on and off. Let's look at how to manage multiple VMs and connect to them remotely.

### Creating Additional VMs

For these exercises, we will assume you have created two more VMs (`ubuntu-vm2` and `ubuntu-vm3`) using the same `virt-install` command we learned earlier.

```bash
# Create VM 2
sudo virt-install --name ubuntu-vm2 ... --noautoconsole

# Create VM 3
sudo virt-install --name ubuntu-vm3 ... --noautoconsole
```

  * `--noautoconsole`: This flag prevents the graphical viewer from popping up automatically, which is useful when creating multiple VMs via script.


## 📡 Connecting to a VM

You usually want to connect to your server VMs using **SSH** from your terminal, rather than using a graphical console window. To do this, you need the VM's **IP address**.

### Finding the IP Address

Since KVM uses a virtual network (usually `virbr0`), you can't just guess the IP.

**Method 1: `ip neighbor` (The Hard Way)**
You can check the ARP table on your host:

```bash
ip neighbor
```

This shows IP addresses (like `192.168.122.129`) associated with the `virbr0` interface. However, it doesn't tell you *which* IP belongs to *which* VM.

**Method 2: `virsh domifaddr` (The Easy Way)**
This command queries the VM directly to ask for its network interface address.

1.  **List your VMs:**
    ```bash
    sudo virsh list --all
    ```
2.  **Get the IP for a specific VM:**
    ```bash
    sudo virsh domifaddr ubuntu-vm1
    ```

**Output:**

```
 Name       MAC address          Protocol     Address----
 vnet0      52:54:00:3b:56:e5    ipv4         192.168.122.129/24
```

Now we know that `ubuntu-vm1` is at `192.168.122.129`.

### Connecting via SSH

Now that you have the IP, you can connect just like any other server.

```bash
ssh hashim@192.168.122.129
```

*(Replace `hashim` with the username you created when installing Ubuntu).*

### Connecting via Graphical Console (`virt-viewer`)

If networking isn't working, or you need to see the boot screen, you can use `virt-viewer` to open a direct console window.

```bash
virt-viewer --connect qemu:///system ubuntu-vm1
```

This opens a window that acts like a physical monitor connected to the VM.

> **Note:** If you run this command in a terminal, closing the terminal (Ctrl+C) will close the viewer window, but the VM will keep running in the background.


### 🖥️ GUI Tools

While the CLI is powerful, modern Linux distributions with GNOME include excellent GUI tools:

  * **Virtual Machine Manager (`virt-manager`):** A full-featured interface for `libvirt`. It lets you manage networks, storage, and VM hardware details visually.
  * **GNOME Boxes:** A simplified, user-friendly tool for quickly spinning up VMs for desktop use.

---

# 🔄 Cloning VMs

Cloning creates an exact copy of a virtual machine. This is incredibly useful if you want to test something without breaking your original VM, or if you want to deploy multiple identical servers quickly.

### 🛑 Step 1: Stop the VM

You cannot clone a running VM safely (the disk data would be inconsistent). First, shut down the VM you want to clone.

```bash
sudo virsh shutdown ubuntu-vm1
```

### 🐑 Step 2: Clone the VM

We will use the **`virt-clone`** command. This tool handles copying the disk images and defining the new VM configuration automatically.

```bash
sudo virt-clone --original ubuntu-vm1 --name ubuntu-vm1-clone1 --auto-clone
```

  * `--original`: The name of the VM you are copying.
  * `--name`: The name of the *new* VM you are creating.
  * `--auto-clone`: Automatically generates a new disk path for the clone and ensures it doesn't conflict with the original.

**Output:**

```
Allocating 'ubuntu-vm1-clone1.qcow2'              | 3.9 GB  00:04 ... 
Clone 'ubuntu-vm1-clone1' created successfully.
```

Now, if you list your VMs, you will see the new clone:

```bash
sudo virsh list --all
 Id    Name                  State
 -     ubuntu-vm1            shut off
 -     ubuntu-vm1-clone1     shut off
```

### ⚠️ The "Identity Crisis" Problem

When you clone a VM, you copy *everything*, including:

  * The hostname
  * The IP address configuration
  * The MAC address (though `virt-clone` usually updates this for you)
  * SSH host keys (security risk\!)

If you start both VMs at the same time, they might conflict on the network. You usually need to log into the clone via the console (using `virt-viewer`) and reset its network settings so it gets a new IP address from DHCP.


# 📄 Creating VM Templates (The Better Way)

A better approach than cloning "dirty" VMs is to create a pristine **Template**. A template is a VM that has been "cleaned" of all unique identifiers (like MAC addresses, user history, and SSH keys).

### Step 1: Install `libguestfs-tools`

This package contains `virt-sysprep`, the magic tool for cleaning VMs.

```bash
sudo apt install libguestfs-tools
```

### Step 2: Create the Base VM

Create a standard Ubuntu VM just like before. Let's call it `ubuntu-template`.

```bash
sudo virt-install --name ubuntu-template ... [standard options]
```

Install the OS, run updates (`apt update && apt upgrade`), and shut it down.

### Step 3: Backup the Image (Optional but Smart)

Before cleaning, make a backup of the disk image just in case.

```bash
sudo cp /var/lib/libvirt/images/ubuntu-template.qcow2 /var/lib/libvirt/images/ubuntu-back-template.qcow2
```

### Step 4: "Prep" the Template

Run **`virt-sysprep`** on the VM. This tool performs operations like:

  * Removing SSH host keys
  * Cleaning bash history
  * Removing network interface configurations (so it gets a fresh IP on boot)
  * Removing log files

<!-- end list -->

```bash
sudo virt-sysprep -d ubuntu-template
```

### Step 5: Use the Template

Now you have a clean "Golden Image." You can use `virt-clone` on this template to create new, unique VMs (`ubuntu-webserver`, `ubuntu-db`, etc.) without worrying about network conflicts or security issues.


# 📊 Obtaining VM and Host Resource Information

When managing a server via CLI, you need commands to see how much CPU and RAM you actually have available.

### Checking Host Resources

Use `virsh nodeinfo` to see the physical server's stats.

```bash
sudo virsh nodeinfo
```

**Output:**

```
CPU model:           x86_64
CPU(s):              16
CPU frequency:       3197 MHz
Memory size:         48113628 KiB
```

This tells us we have **16 CPU cores** and roughly **48 GB of RAM** available on the physical host.

### Checking VM Resources

To see what resources a specific VM is allocated, use `virsh dominfo`.

```bash
sudo virsh dominfo ubuntu-vm1
```

**Output:**

```
Name:           ubuntu-vm1
State:          shut off
CPU(s):         2
Max memory:     2097152 KiB
Used memory:    2097152 KiB
```

This confirms `ubuntu-vm1` has **2 vCPUs** and **2 GB (2048 MB)** of RAM.

### Checking VM Disk Usage

To see how much disk space a VM is actually using inside its virtual drive, use `virt-df`.

```bash
sudo virt-df -h -d ubuntu-vm1
```

  * `-h`: Human-readable sizes (GB/MB).
  * `-d`: Specifies the domain (VM).

**Output:**

```
Filesystem                            Size       Used  Available  Use%
ubuntu-vm1:/dev/sda2                  1.7G       130M       1.5G    8%
ubuntu-vm1:/dev/ubuntu-vg/ubuntu-lv   9.7G       4.7G       4.5G   49%
```


# ⚙️ Managing VM Resource Usage

Sometimes you need to resize a VM. Maybe it's too slow and needs more RAM, or maybe it's too big and wasting CPU cores. You can change these settings using `virsh`.

**Scenario:** We want to downgrade `ubuntu-vm1-clone1` to **1 vCPU** and **1 GB RAM**.

### Changing CPU Count

You must set both the *maximum* allowed and the *current* count. The `--config` flag ensures the change is saved to the VM's configuration file (requires a reboot to take effect).

```bash
# Set the maximum limit to 1
sudo virsh setvcpus --domain ubuntu-vm1-clone1 --maximum 1 --config

# Set the current active count to 1
sudo virsh setvcpus --domain ubuntu-vm1-clone1 --count 1 --config
```

### Changing Memory (RAM)

Similarly, you set the maximum memory and the current memory. Note that `1G` is a shortcut for 1 Gigabyte.

```bash
# Set the maximum memory limit
sudo virsh setmaxmem ubuntu-vm1-clone1 1G --config

# Set the current memory
sudo virsh setmem ubuntu-vm1-clone1 1G --config
```

### Verifying Changes

After running these commands (and restarting the VM if it was running), you can verify the new settings:

```bash
sudo virsh dominfo ubuntu-vm1-clone1
```

**Output:**

```
CPU(s):         1
Max memory:     1048576 KiB
Used memory:    1048576 KiB
```

The VM is now successfully resized\!

---