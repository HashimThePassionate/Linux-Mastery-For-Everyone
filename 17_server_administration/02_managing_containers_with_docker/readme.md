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

# 🧠 Understanding Linux Namespaces

**Namespaces** are a critical Linux kernel feature. They are the primary mechanism that provides the "isolation" in containers.

In a nutshell, namespaces wrap a global system resource in an abstraction layer. This tricks a process inside the namespace into believing that it has its own independent copy of that resource.

### Why do we need them?
Normally, a user or process on Linux can see almost everything: other users, all running processes, network interfaces, and mounted disks. This transparency is bad for containers, which need to be isolated environments. Since containers don't have a hypervisor to enforce this separation, they rely on namespaces to create "virtual" views of the system.

### Types of Namespaces
There are several types of namespaces, each isolating a specific aspect of the system:

1.  **Mount (`mnt`):** Isolates filesystem mount points. A process inside this namespace sees a completely different set of mounted filesystems than the host. This allows containers to have their own private root filesystem (`/`).
2.  **UTS (Unix Time Sharing):** Isolates the hostname and domain name. This allows each container to have its own hostname (e.g., `webserver-01`) independent of the host machine.
3.  **IPC (Interprocess Communication):** Isolates communication resources like shared memory, message queues, and semaphores. Processes in different IPC namespaces cannot communicate via these standard methods.
4.  **PID (Process ID):** Isolates the process ID number space. Inside a new PID namespace, the first process created gets **PID 1** (just like the init system on a real server). It cannot see processes outside its namespace.
5.  **Network (`net`):** Isolates the network stack. Each namespace gets its own private network interfaces (e.g., `eth0`), IP addresses, routing tables, and firewall rules (`iptables`).
6.  **User (`user`):** Isolates User and Group IDs. This is powerful security: a process can be `root` (UID 0) *inside* the container but map to a powerless, unprivileged user (e.g., UID 1000) *outside* on the host.
7.  **Cgroup (`cgroup`):** Isolates the view of control groups (resource limits). It hides the host's full hierarchy, showing the container only its own resource limits.

### 👀 Viewing Namespaces
You can see the namespaces currently active on your system using the `lsns` command.


Figure 12.2 shows the output of `lsns`. Each row represents a namespace.
* **NS:** The unique ID of the namespace.
* **TYPE:** The type (mnt, net, pid, etc.).
* **NPROCS:** Number of processes running in that namespace.
* **COMMAND:** The command that created or is running in that namespace.

This command is a great way to "peek under the hood" and see the isolation boundaries the kernel is enforcing.
