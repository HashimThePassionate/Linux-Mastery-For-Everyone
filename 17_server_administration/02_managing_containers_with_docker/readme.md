# 🐳 Containers vs. Virtual Machines (VMs)

Virtual Machines (VMs) emulate an entire machine's hardware, acting as if they are distinct computers with their own dedicated hardware. In contrast, containers do not emulate hardware at all.

A container shares the host operating system's kernel, along with the necessary shared libraries and binaries to run specific applications. Applications run inside these isolated containers, separate from the rest of the system. Containers also share the host's network interface to provide connectivity similar to VMs.

### 📊 Figure 12.1: The Architecture Comparison

The diagram below visually compares the software stacks of Containers versus Virtual Machines.

<div align="center">
  <img src="./images/01.png" width="500"/>

**Explanation of Figure 12.1:**
</div>


#### Left Side: Containers (The Lightweight Approach)
* **Top Layer (User Space):** You have multiple isolated applications (`Apps`) running with their specific dependencies (`Bins/Libs`).
* **Middle Layer:** Instead of a full OS for each app, they all sit on top of a **Container Engine** (like Docker, LXC, or LXD).
* **Bottom Layer:** The Container Engine shares the single **Host Operating System** kernel across all containers. This sits directly on the **Physical Hardware**.
* **Key Takeaway:** There is no "Guest OS" layer. This removes massive overhead, making containers start in milliseconds.

#### Right Side: Virtual Machines (The Heavyweight Approach)
* **Top Layer:** Like containers, you have your `Apps` and `Bins/Libs`.
* **Guest OS Layer:** Crucially, *every* VM requires its own full **Guest Operating System** (e.g., a full install of Ubuntu or Windows). This duplicates the kernel, system processes, and memory usage for every single VM.
* **Hypervisor Layer:** The **Hypervisor** (like KVM) manages these Guest OSs.
* **Bottom Layer:** The Hypervisor sits on the **Host Operating System** (or bare metal), which sits on the **Physical Hardware**.

---

## 🏗️ Understanding Container Technology

Container engines provide **OS-level virtualization**, allowing developers to deploy and test applications bundled with only the libraries and dependencies they need. This ensures that an application behaves exactly the same way on any machine, solving the "it works on my machine" problem.

### A Brief History
Containerization isn't new. On Unix, the `chroot` tool has provided basic isolation since 1982. On Linux, modern containerization began with **LXC (Linux Containers)** in 2008, which was later extended by **LXD**. However, **Docker**, introduced in 2013, revolutionized the landscape and fueled the DevOps movement.

### How It Works Under the Hood
Containers use specific userspace interfaces that leverage Linux kernel features to isolate resources. They replicate a standard Linux environment without needing a separate kernel.

The backbone of any container system (like LXC or Docker) relies on these key kernel technologies:
1.  **Kernel Namespaces:** Provide isolation for processes, networking, and user IDs.
2.  **Cgroups (Control Groups):** Manage and limit resource usage (CPU, RAM) for groups of processes.
3.  **Chroots:** Isolate the file system view.
4.  **Security Profiles:** Use AppArmor and SELinux to enforce security boundaries.

### LXC vs. Docker
* **LXC/LXD:** One of the earliest modern container forms. It was popular for its API support across languages like Python, Go, and Ruby. While less dominant now, it is still supported (LXC 4.0 until 2025, LXC 5.0 until 2027).
* **Docker:** The current industry standard. It took the crown by making containers easier to use and portable, becoming the center of modern container engine usage.

We will focus on Docker for our examples, as it is the most relevant tool today.