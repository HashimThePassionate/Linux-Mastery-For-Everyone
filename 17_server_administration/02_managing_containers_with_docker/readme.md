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

# 🧠 Linux cgroups (Control Groups)

**Cgroups** (short for Control Groups) are a Linux kernel feature that allows you to allocate resources—such as CPU time, system memory, network bandwidth, or combinations of these resources—among hierarchically ordered groups of processes.

While **namespaces** isolate *what* a process can see (like files or network interfaces), **cgroups** restrict *how much* of the system's resources a process can use.

### ⚙️ What do Cgroups Control?
Cgroups provide a mechanism to limit, account for, and isolate the resource usage (CPU, memory, disk I/O, network, etc.) of a collection of processes.

Key resources managed by cgroups include:
* **🧠 Memory:** Limits how much RAM a group of processes can use. If they exceed the limit, the Out of Memory (OOM) killer might terminate them.
* **💻 CPU:** Limits the amount of CPU time a group can consume (e.g., "Container A gets 20% of the CPU").
* **💾 I/O (Input/Output):** Limits read/write speeds to block devices (hard drives), preventing one container from hogging all disk performance.
* **🌐 Network:** (Often managed via `net_cls` or `net_prio`) Tags network packets to control bandwidth priority.

### 🌳 The Hierarchy Concept
Cgroups are organized hierarchically, similar to a file system tree.
* **Inheritance:** Every child group inherits the attributes and limits of its parent group.
* **Multiple Hierarchies:** You can have different hierarchies for different resources. For example, you might group processes one way for CPU usage and a completely different way for Memory usage, though modern implementations (cgroups v2) tend to unify this into a single hierarchy for simplicity.

### 🤝 The Power Couple: Cgroups + Namespaces
This is the secret sauce behind modern containers like Docker:

1.  **Namespaces:** Provide **Isolation**. (The container thinks it's the only thing running on the system).
2.  **Cgroups:** Provide **Resource Management**. (The container is stopped from using 100% of the host's RAM or CPU).

By combining these two technologies, resources are allocated and managed for each container separately. This architecture allows containers to be lightweight and run as isolated entities compared to the heavier hardware emulation required by Virtual Machines.

# 🐳 Understanding Docker

**Docker** is a platform designed for developing, shipping, and running applications. Like LXC/LXD, it is built on fundamental Linux kernel technologies, specifically **namespaces** and **cgroups**.

The Docker platform provides the infrastructure that allows containers to operate securely. Docker containers are lightweight, standalone units that run directly on the host's kernel. The platform offers a suite of tools to create and manage these isolated, containerized applications.

The **container** is the base unit for modern application development, testing, and distribution. When an app is ready for production, it can be shipped as a container or as part of an orchestrated service (often managed by tools like Kubernetes).

---

## 🏗️ Docker Architecture

To understand how Docker works, we must look at its layered architecture.

<div align="center">
  <img src="./images/02.png" width="500"/>
</div>

As shown in **Figure 12.3**, the architecture is built on the Host Operating System (Linux) and relies on core kernel features like **Namespaces** and **cgroups**. Above these kernel features sit the **Container Runtime** and the **Docker Engine**, topped by the **REST API** which allows for automation.

Let's break down these two major components in detail.

### 1. Container Runtime
The **Container Runtime** is the low-level component responsible for actually running containers. It adheres to the standards set by the **Open Container Initiative (OCI)**, which defines the open industry standards for container formats and runtimes.

The runtime is split into two key pieces:
* **`containerd`**: A high-level daemon that manages the complete container lifecycle of its host system: image transfer and storage, container execution and supervision, and networking. It is responsible for downloading Docker images and then running them.
* **`runc`**: A low-level CLI tool for spawning and running containers according to the OCI specification. It is directly responsible for interacting with the kernel to set up namespaces and cgroups for each container.

> **Note:** Docker donated `runc` and `containerd` to the **Cloud Native Computing Foundation (CNCF)**. This move was crucial as it allowed broader industry collaboration and standardization, ensuring these core components weren't controlled by a single company.

<div align="center">
  <img src="./images/03.png" width="500"/>
</div>

**Figure 12.4** provides a more detailed view of this hierarchy. You can see how the API Interface communicates with `dockerd`, which talks to `containerd`, which finally uses `runc` to spawn the actual container processes.

### 2. The Docker Engine
The **Docker Engine** is the higher-level component that users interact with. It is split into:
* **The `dockerd` daemon:** A long-running process that listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes.
* **The API Interface:** A REST API that specifies interfaces that programs can use to talk to the daemon and instruct it what to do.
* **The Command-Line Interface (CLI):** The `docker` command you type in your terminal. It uses the Docker API to control or interact with the Docker daemon.

---

## 🔄 Docker Workflow

Docker uses a **client-server architecture**. The workflow involves three main players: the Client, the Host (Server Daemon), and the Registry.

<div align="center">
  <img src="./images/04.png" width="500"/>
</div>

**Figure 12.5** illustrates this workflow clearly:

1.  **The Client:** This is how users interact with Docker (e.g., running `docker build`, `docker pull`, `docker run`). The client sends these commands to the Docker daemon via the API.
2.  **The Docker Host (Server):** This is the machine running the `dockerd` daemon. The daemon listens for API requests and manages the heavy lifting of building, running, and distributing your containers. It manages **Images** and **Containers**.
3.  **The Registry:** This is where Docker images are stored.
    * **Docker Hub** is the default public registry that anyone can use.
    * You can also host your own **Private Registries**.

**Typical Workflow:**
* When you run `docker pull ubuntu`, the daemon checks if it has the image locally. If not, it downloads it from the configured **Registry** (like Docker Hub).
* When you run `docker run ubuntu`, the daemon takes that **Image** and creates a runnable **Container** from it.

# 🐳 Working with Docker

This section introduces the practical side of working with Docker. Before running commands, it is essential to understand the environment we are working in, the licensing model of Docker, and the best strategy for installation to ensure stability and security.

## 💻 The Lab Environment

For the exercises in this section, the text specifies a specific setup to ensure consistency.

* **Operating System:** Debian GNU/Linux 12 (Bookworm).
* **Hardware Resources:**
    * **vCPUs:** 2
    * **RAM:** 2 GB
* **Role:** This machine acts as the **Docker Host**.

---

## 🏷️ Choosing the Right Docker Version

Docker, Inc. has evolved its business model over time. Understanding this evolution helps clarify which version you should install.

### 1. The History: CE vs. EE
In the past, Docker offered two distinct products:
* **Docker CE (Community Edition):** The free, open-source version.
* **Docker EE (Enterprise Edition):** The paid version with support and advanced features, which generated the company's revenue.

### 2. The Current Model: Subscription Tiers
Recently, Docker restructured its portfolio into four main tiers. This change affects how users access tools like Docker Desktop.

| Product Tier | Cost | Target Audience |
| :--- | :--- | :--- |
| **Docker Personal** | **Free** | Individual developers, education, open-source communities, and small businesses. |
| **Docker Pro** | Paid | Individual professional developers requiring advanced tools. |
| **Docker Team** | Paid | Development teams requiring collaboration features. |
| **Docker Business** | Paid | Large organizations requiring centralized management and security. |

### 3. Our Choice: Docker Personal
For the purpose of learning and general server administration in this guide, we will use **Docker Personal**.
* **Cost:** Free.
* **Includes:** Docker Engine (for servers) and Docker Desktop (for Linux/Mac/Windows).
* **Requirement:** It usually implies the existence of a Docker user account (Docker ID).

> **Note on Architecture:** Docker Engine is highly versatile. It supports both **x86** (Intel/AMD) and **ARM** architectures and provides native packages (`.deb` for Debian/Ubuntu and `.rpm` for Fedora/CentOS/RHEL).

---

## 📥 Installation Strategy: Repo vs. Repo

When installing software on Linux, you generally have two choices. The text advises on the best path for Docker.

### Option A: The OS Repository (Not Recommended)
You can install Docker directly from Debian's default repositories (e.g., `sudo apt install docker.io`).
* **Pros:** Easy, no setup required.
* **Cons:** The version is often **out of date**. Linux distributions freeze package versions for stability, meaning you might miss out on the latest Docker features and security patches.

### Option B: The Official Docker Repository (Recommended)
This method involves adding Docker's official package repository to your system's package manager (`apt`).
* **Pros:** You always get the **latest stable version** directly from the source.
* **Cons:** Requires a few extra setup steps.

### 🏆 The Decision
Since we are using a fresh Debian system with no prior Docker installation, we do not need to worry about conflicts with older versions.

We will proceed using **Option B (Docker's official `apt` repository)**. This ensures our environment is current, secure, and compatible with modern container standards.

# 🐳 Installing Docker on Debian 12

This guide details the step-by-step procedure to install Docker Personal (Docker Engine) on Debian GNU/Linux 12 (Bookworm) using the official Docker repository. This method ensures you always have the latest stable version.

-----

## 🛠️ Step 1: Install Prerequisites

Before installing Docker, we need to update the system and install packages that allow `apt` to use a repository over HTTPS.

### The Command

```bash
sudo apt update -y && sudo apt install ca-certificates curl gnupg
```

### 👨‍💻 Code Explanation

  * **`sudo apt update -y`**: Refreshes the local package index to ensure we are aware of the latest software versions.
  * **`&&`**: A logical operator that runs the second command only if the first one succeeds.
  * **`sudo apt install ...`**: Installs the following necessary tools:
      * **`ca-certificates`**: Allows the system to check the validity of SSL/TLS certificates (needed for secure connections).
      * **`curl`**: A tool for transferring data; we will use it to download the Docker security key.
      * **`gnupg`**: The GNU Privacy Guard, used for encryption and signing keys.

-----

## 🔑 Step 2: Add the Docker GPG Key

To ensure the software we download is authentic and hasn't been tampered with, we need to add Docker's official GPG (GNU Privacy Guard) key to our system.

### The Commands

```bash
# 1. Create the directory for keys with specific permissions
sudo install -m 0755 -d /etc/apt/keyrings

# 2. Download and process the key
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 3. Set read permissions for the key
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### 👨‍💻 Code Explanation

1.  **`install -m 0755 -d`**: Creates the directory `/etc/apt/keyrings` and sets the mode (`-m`) to `0755` (readable/executable by everyone, writable only by root).
2.  **`curl -fsSL ...`**: Downloads the key from the official Docker website.
      * **`|` (Pipe)**: Sends the downloaded key directly to the next command.
      * **`gpg --dearmor`**: Converts the key from a standard text format (ASCII) into a binary format that `apt` can use securely.
      * **`-o ...`**: Saves the resulting binary key to `/etc/apt/keyrings/docker.gpg`.
3.  **`chmod a+r`**: Ensures that **a**ll users have **r**ead permissions for the key, so the update process can read it.

-----

## 📦 Step 3: Set Up the Repository

Now we need to tell `apt` where to look for the Docker packages. We will construct the repository link dynamically so it matches our specific CPU architecture and Debian version.

### The Command

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 👨‍💻 Code Explanation

  * **`echo "..."`**: We are constructing a text string that acts as the repository configuration.
  * **`[arch=$(dpkg --print-architecture)]`**: Automatically detects if you are on Intel (`amd64`), ARM (`arm64`), etc.
  * **`signed-by=...`**: Tells `apt` to use the GPG key we downloaded in Step 2 to verify this specific repository.
  * **`$(. /etc/os-release && echo "$VERSION_CODENAME")`**: This command reads your system info and automatically inserts your Debian version codename (e.g., `bookworm`).
  * **`| sudo tee ...`**: Takes the echoed text and writes it to the file `/etc/apt/sources.list.d/docker.list` with root privileges.
  * **`> /dev/null`**: Discards the output so it doesn't clutter the terminal.

-----

## 🔄 Step 4: Update Repository List

Now that we have added a new repository, we must update our package index so the system "sees" the new Docker packages.

### The Command

```bash
sudo apt update -y
```

### 👨‍💻 Detailed Explanation

You will notice in the output that the system is now hitting `https://download.docker.com`. This confirms the previous steps were successful.

-----

## 📥 Step 5: Install Docker Packages

We are now ready to install the Docker Engine and its associated components.

### The Command

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 👨‍💻 Code Explanation

We are installing five specific packages:

1.  **`docker-ce`**: The Docker **C**ommunity **E**dition daemon (the engine itself).
2.  **`docker-ce-cli`**: The Command Line Interface (the tool you use to type `docker` commands).
3.  **`containerd.io`**: The container runtime that manages the lifecycle of a container.
4.  **`docker-buildx-plugin`**: A plugin that extends Docker's build capabilities.
5.  **`docker-compose-plugin`**: A plugin that allows you to use `docker compose` to manage multi-container applications.

-----

## 🧐 Step 6: Verify Source

Before proceeding, it is good practice to verify that `apt` installed Docker from the official Docker repository, not the default Debian one (which might be outdated).

### The Command

```bash
apt-cache policy docker-ce
```

### 🖥️ Expected Output

Look for the URL `https://download.docker.com`.

```text
hashim@debian:~$ apt-cache policy docker-ce
docker-ce:
  Installed: 5:23.0.6-1~debian.12~bookworm
  Candidate: 5:23.0.6-1~debian.12~bookworm
  Version table:
 *** 5:23.0.6-1~debian.12~bookworm 500
        500 https://download.docker.com/linux/debian bookworm/stable amd64 Packages
        100 /var/lib/dpkg/status
     5:23.0.5-1~debian.12~bookworm 500
        500 https://download.docker.com/linux/debian bookworm/stable amd64 Packages
```

-----

## 🟢 Step 7: Check Service Status

Docker should start automatically after installation. Let's confirm the daemon is active.

### The Command

```bash
sudo systemctl status docker
```

### 👨‍💻 Detailed Explanation

You should see `Active: active (running)` in green text. Press `q` to exit the status view.

-----

## 👤 Step 8: Configure User Permissions

By default, running Docker commands requires `root` privileges (using `sudo`). To allow your regular user to run Docker commands, you must add them to the `docker` group.

### The Commands

```bash
# Optional: Verify the group exists
tail /etc/group

# Add your current user to the docker group
sudo usermod -aG docker ${USER}
```

### 👨‍💻 Code Explanation

  * **`tail /etc/group`**: Shows the last few lines of the groups file. You should see `docker:` at the bottom.
  * **`usermod`**: Modifies a user account.
      * **`-aG`**: **A**ppends the user to a **G**roup.
      * **`docker`**: The name of the group.
      * **`${USER}`**: A variable representing your current logged-in username (e.g., `hashim`).

-----

## 🔄 Step 9: Apply Group Changes

Group membership changes do not take effect immediately. You must log out and log back in, or refresh your session.

### The Verification Command

After logging back in, run:

```bash
groups
```

### 👨‍💻 Detailed Explanation

The output should list `docker` among your groups. If it does, you can now run commands like `docker run hello-world` without using `sudo`.

-----

## 🚀 Step 10: Enable Docker on Boot

Finally, ensure that the Docker service starts automatically whenever the server is rebooted.

### The Command

```bash
sudo systemctl enable docker
```

### 👨‍💻 Detailed Explanation

This creates a symbolic link in the system's initialization directory, ensuring the Docker daemon (`dockerd`) launches as soon as the operating system boots up. Docker is now fully installed and configured\!

# 🛠️ Using Docker Commands

Working with Docker means using its Command Line Interface (CLI). It has a significant number of sub-commands available. To see them all, you can run `docker --help`.

We will not discuss all commands here, but we will focus on the essential ones needed to get started with Docker.

-----

## 1\. Verifying the Installation (`docker run`)

Before learning anything about the commands, let’s first perform a test to see whether the installation is working. We will use the `docker run` command to check whether we can access **Docker Hub** (the public registry) and run containers.

### The Command

```bash
sudo docker run hello-world
```

### 🖥️ Expected Output

```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete
Digest: sha256:9eabfcf6034695c4f6208296be9090b0a3487e20fb6a5cb0565242621cf73d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### 👨‍💻 What just happened?

This message confirms your installation is working. Behind the scenes, the Docker Daemon took the following steps:

1.  **Check Local Cache:** It checked if the `hello-world` image existed locally. It didn't ("Unable to find image...").
2.  **Pull from Registry:** It contacted Docker Hub and downloaded the image.
3.  **Create Container:** It created a new container from that image.
4.  **Run & Output:** It ran the executable inside the container, which printed the "Hello from Docker\!" message, and streamed that output to your terminal.

-----

## 2\. Searching for Images (`docker search`)

Let’s now dig deeper and search for other images available on Docker Hub. For example, let's search for an **Ubuntu** image.

### The Command

```bash
docker search ubuntu
```

### 🖥️ Expected Output

The output displays a list of images that match your search term.

```text
NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
ubuntu                           Ubuntu is a Debian-based Linux operating sys…   15927     [OK]
websphere-liberty                WebSphere Liberty multi-architecture images …   293       [OK]
open-liberty                     Open Liberty multi-architecture images based…   59        [OK]
...
```

### 📊 Understanding the Output Columns

  * **NAME:** The name of the image (e.g., `ubuntu`, `ubuntu/nginx`).
  * **DESCRIPTION:** A short text describing what the image does.
  * **STARS:** A popularity metric based on user likes (similar to GitHub stars). High star counts usually indicate reliability.
  * **OFFICIAL:** If marked `[OK]`, this image is officially supported and maintained by the company behind the software (e.g., Canonical for Ubuntu). These are the safest images to use.
  * **AUTOMATED:** Shows whether the image is built automatically from a source repository.

-----

## 3\. Downloading Images (`docker pull`)

Once you find the image you are looking for, you can download it onto your system without running it immediately. Let's download the official **Ubuntu** image.

### The Command

```bash
docker pull ubuntu
```

### 👨‍💻 Explanation

With this command, the `ubuntu` image is downloaded locally onto your computer. It is now stored in your local Docker cache, ready to spawn containers instantly.

-----

## 4\. Listing Local Images (`docker images`)

To see what images are currently stored on your computer, use the `docker images` command.

### The Command

```bash
docker images
```

### 🖥️ Expected Output

```text
REPOSITORY    TAG       IMAGE ID       CREATED       SIZE
hello-world   latest    9c7a54a9a43c   4 days ago    13.3kB
ubuntu        latest    3b418d7b466a   13 days ago   77.8MB
```

### 🧐 Note on Image Size

Notice the small size of the Ubuntu image (\~78MB). A full Ubuntu Server install is usually much larger.

  * **Why?** Docker images contain *only* the base minimum packages and libraries needed to run the OS userspace. They do not include a kernel (since they share the host's kernel) or unnecessary tools like UI or documentation. This makes containers extremely efficient.

# 🏗️ Managing Docker Containers

In this section, we will learn the essential lifecycle commands for Docker containers: running, listing, starting, stopping, and removing them.

-----

## 🚀 Running Containers

We will use the Ubuntu image that we previously downloaded. To launch it interactively, we use the `docker run` command with two critical arguments:

  * **`-i` (interactive):** Keeps STDIN open even if not attached.
  * **`-t` (tty):** Allocates a pseudo-TTY (terminal). This is what makes it feel like a real terminal session.

### The Command

```bash
docker run -it ubuntu
```

### 👨‍💻 What happens next?

Once you run this, your command prompt will change immediately.

  * **Before:** `hashim@debian:~$` (You are on your host machine).
  * **After:** `root@f02e14e61e2b:/#` (You are inside the container).

You are now the **root** user inside a fresh Ubuntu environment. You can run commands like `apt update`, install software, create files, etc. Just remember: **containers are ephemeral**. Any changes you make stay *inside* that specific container instance. If you delete the container, your changes are gone (unless you use volumes or commit the image).

To exit the container and return to your host machine, simply type:

```bash
exit
```

*(Note: Exiting the shell usually stops the container unless it was started in detached mode).*

-----

## 📋 Listing Containers

You can check the status of your containers using the `docker ps` command.

### 1\. List Running Containers

Open a **new terminal window** (while keeping your Ubuntu container running in the first one) and run:

```bash
docker ps
```

**Output:**

```text
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
f02e14e61e2b   ubuntu    "/bin/bash"   3 minutes ago   Up 3 minutes             amazing_hopper
```

This shows only containers that are currently **Up** (running).

### 2\. List All Containers (Active and Inactive)

To see containers that have stopped (like the "hello-world" one from earlier), add the `-a` flag.

```bash
docker ps -a
```

**Output:**

```text
CONTAINER ID   IMAGE         STATUS                     NAMES
f02e14e61e2b   ubuntu        Up 3 minutes               amazing_hopper
3a340db56d48   hello-world   Exited (0) 1 hour ago      recursing_murdock
```

### 3\. List the Latest Container

To see just the most recently created container (running or not), use `-l`.

```bash
docker ps -l
```

### 🏷️ A Note on Names

Notice the names `amazing_hopper` and `recursing_murdock`. Docker automatically assigns a random name (an adjective + a famous scientist/hacker) to every container if you don't specify one yourself using `--name`.

-----

## 🚦 Starting, Stopping, and Removing

You can manage containers using either their **Container ID** (e.g., `f02e14e61e2b`) or their **Name** (e.g., `amazing_hopper`).

### Stopping a Container

If a container is running, you can stop it gracefully.

```bash
docker stop amazing_hopper
```

After running this, `docker ps` will show an empty list because the container is no longer running.

### Starting a Container

To start a stopped container again (preserving its state from when it stopped):

```bash
docker start amazing_hopper
```

It will now appear in `docker ps` again.

### Removing a Container

Containers take up disk space even when stopped. To permanently delete a container, use `rm`.

```bash
docker rm recursing_murdock
```

*(Note: You generally cannot remove a running container. You must `stop` it first, or use `docker rm -f` to force removal).*

> **Important Note:**
> Removing a container deletes the *instance* (the writable layer where your changes lived). It **does not** delete the underlying **Image** (Ubuntu, Hello-World) from your system. You can always spawn new containers from that image.


# 🌐 Docker Networking

In this section, we will perform a practical exercise: modifying a running Ubuntu container by installing Python packages and then saving that modified state as a new, custom Docker image.

## 🐍 The Scenario: Installing Python 3

We start by running our Ubuntu container and checking if Python is pre-installed.

### 1\. Launch and Check

```bash
docker run -it ubuntu
```

Once inside the container (`root@...:/#`), check for Python:

```bash
python -v
# Output: bash: python: command not found

python3 -v
# Output: bash: python3: command not found
```

### 2\. Attempting Installation (and Troubleshooting Networking)

Since Python is missing, we try to install it using the container's package manager (`apt`).

```bash
apt install python3
```

**Potential Error:**
You might see an error like `E: Unable to locate package python3`. This usually means the container cannot reach the internet (the Ubuntu repositories).

**Troubleshooting with `docker network`:**
To investigate, we use the `docker network` command family. Open a **new terminal** on your host machine to run these.

  * **List Networks:**
    ```bash
    docker network ls
    ```
    **Output:**
    ```text
    hashim@debian:~$ docker network ls
    NETWORK ID     NAME      DRIVER    SCOPE
    f8b50039d02e   bridge    bridge    local
    2d35dbd91efc   host      host      local
    4ae4f24c96be   none      null      local
    ```
      * **`bridge`**: The default network. Containers usually attach here to talk to the outside world.
      * **`host`**: The container shares the host's networking stack directly.
      * **`none`**: The container has no networking.

### 3\. Connecting a Running Container

If your container isn't connected, you can attach it to a network on the fly.

1.  **Find Container Name/ID:**

    ```bash
    docker ps
    ```

    Let's assume the name is `peaceful_euler`.

2.  **Connect to Bridge:**

    ```bash
    docker network connect bridge peaceful_euler
    ```

3.  **Test Connectivity (Inside the Container):**
    Go back to your container terminal and try updating again.

    ```bash
    apt update -y
    ```

    **Output:**

    ```text
    Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
    Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
    ...
    ```

    If you see lines like `Get:1 ...`, success\! You are online.

**Pro Tip:** To avoid this manual step, you can specify the network when you *start* the container:

```bash
docker run -itd --network=bridge ubuntu
```

### 4\. Installing Python 3

Now that we are online, run the installation command inside the container again:

```bash
apt install python3
```

**Output:**

```text
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libexpat1 libmpdec3 libpython3-stdlib ...
0 upgraded, 13 newly installed, 0 to remove and 0 not upgraded.
```

This time, it will download the packages and install Python 3.

-----

## 💾 Committing the New Image

We now have a container that is different from the base `ubuntu` image—it has Python 3 installed. If we delete this container, that work is lost. To save this state, we **commit** it as a new image.

### 1\. Get Container ID

Open a terminal on your host and find the ID of your modified container.

```bash
docker ps
```

**Output:**

```text
hashim@debian:~$ docker ps
CONTAINER ID    IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
260ead99f7ca    ubuntu    "/bin/bash"   35 seconds ago   Up 34 seconds             peaceful_euler
```

  * **Container ID:** `260ead99f7ca`

### 2\. Run `docker commit`

We will save this container as a new image named `packt/ubuntu-python3`.

```bash
docker commit -m "added python3 to ubuntu" -a "packt user" 260ead99f7ca packt/ubuntu-python3
```

**Command Breakdown:**

  * **`-m "..."`**: A commit **m**essage (like in Git) describing what you changed.
  * **`-a "..."`**: The **a**uthor of the change (e.g., your name).
  * **`260ead99f7ca`**: The **Source Container ID** you are saving.
  * **`packt/ubuntu-python3`**: The **Target Image Name** (Repository/Tag).

**Output:**

```text
sha256:d02e16cc3a1f3d976cd97347d57c9ec7869a303c4a8b14d014e8c744b9720a90
```

This long string is the ID of the new image you just created.

### 3\. Verify the New Image

Check your local image list to see the result.

```bash
docker images
```

**Output:**

```text
hashim@debian:~$ docker images
REPOSITORY             TAG       IMAGE ID       CREATED         SIZE
packt/ubuntu-python3   latest    d02e16cc3a1f   4 seconds ago   149MB
hello-world            latest    9c7a54a9a43c   6 days ago      13.3kB
ubuntu                 latest    3b418d7b466a   2 weeks ago     77.8MB
```

**Notice the Size:**
The base `ubuntu` image was \~78MB. Our new `packt/ubuntu-python3` image is \~149MB. This confirms that our changes (Python 3 and its dependencies) have been saved into the new image layer. You can now spawn new containers from `packt/ubuntu-python3` and they will have Python pre-installed\!

# 🐳 Working with Dockerfiles

Before starting to work with Dockerfiles, let’s see what a **Dockerfile** is. It is a text file that consists of instructions defined by the user for Docker to execute. It respects a basic structure:

```dockerfile
INSTRUCTION arguments
```

The Dockerfile is mainly used for **creating new container images**. Docker reads this file and automatically builds images based on the instructions inside.

### Key Dockerfile Instructions

  * **`FROM`**: This **must** be the first instruction. It tells Docker which existing image to build upon (the "Base Image").
  * **`LABEL`**: Adds metadata, such as the author's name, version, or description. (Note: `MAINTAINER` is the older, deprecated version of this).
  * **`RUN`**: Executes commands *inside* the image during the build process (e.g., `apt install python`). Each `RUN` instruction adds a new layer to the image.
  * **`ADD`**: Copies files/directories from your computer into the image. It can also handle URLs and automatically extract tarballs.
  * **`COPY`**: Similar to `ADD`, but simpler. It copies local files into the image without the extra "magic" features of `ADD`. This is preferred for simple file copying.
  * **`CMD`**: Specifies the *default* command to run when a container starts. There can only be one `CMD`.
  * **`USER`**: Sets the username (or UID) to use when running the image.
  * **`WORKDIR`**: Sets the working directory for subsequent instructions (`RUN`, `CMD`, `COPY`, etc.). It acts like `cd` inside the build process.
  * **`ENTRYPOINT`**: Configures the container to run as an executable.

For a complete list, visit the [official Docker documentation](https://docs.docker.com/engine/reference/builder/).

-----

## 🛠️ Building Container Images from Dockerfiles

In this section, we will create a Dockerfile to build a custom Docker image.

**Scenario:** We want to create a lightweight image based on **Debian** that comes pre-installed with a **Python 3** programming environment.

### Step 1: Create a Workspace

First, create a clean directory to work in.

```bash
mkdir ~/my_docker_images && cd ~/my_docker_images
```

### Step 2: Create the File

Create an empty file named `py_env_dockerfile`.

```bash
touch py_env_dockerfile
```

### Step 3: Edit the Dockerfile

Open the file in your text editor and add the following content.

```dockerfile
# Use the official Debian image as a base
FROM debian

# Add metadata about the author
MAINTAINER packt

# Update repositories
RUN apt update -y

# Upgrade installed packages
RUN apt upgrade -y

# Install Python 3
RUN apt install python3 -y
```

**Code Explanation:**

  * `FROM debian`: We start with the official Debian Linux image.
  * `MAINTAINER packt`: Identifies the author.
  * `RUN ...`: These commands execute standard Linux shell commands inside the image to update the system and install Python 3. The `-y` flag is crucial because the build process cannot answer "yes/no" prompts interactively.

### Step 4: Build the Image

Now, run the `docker build` command to process the Dockerfile and create the image.

```bash
docker build ~/my_docker_images -f py_env_dockerfile -t pydeb
```

  * **`~/my_docker_images`**: The "context" directory (where to look for files).
  * **`-f py_env_dockerfile`**: Specifies the filename of our Dockerfile.
  * **`-t pydeb`**: Tags (names) the resulting image `pydeb`.

**Command Output:**

```text
packt@debian:~/my_docker_images$ docker build ~/my_docker_images -f py_env_dockerfile -t pydeb
[+] Building 8.9s (8/8) FINISHED
 => [internal] load .dockerignore                                      0.0s
 => => transferring context: 2B                                        0.0s
 => [internal] load build definition from py_env_dockerfile            0.0s
 => => transferring dockerfile: 140B                                   0.0s
 => [internal] load metadata for docker.io/library/debian:latest       1.5s
 => [1/4] FROM docker.io/library/debian@sha256:63d6...                 0.0s
 => CACHED [2/4] RUN apt update -y                                     0.0s
 => CACHED [3/4] RUN apt upgrade -y                                    0.0s
 => [4/4] RUN apt install python3 -y                                   7.1s
 => exporting to image                                                 0.2s
 => => exporting layers                                                0.2s
 => => writing image sha256:5f925117827e...                            0.0s
 => => naming to docker.io/library/pydeb                               0.0s
```

**Success\!** The output shows that Docker executed each step (`[1/4]`, `[2/4]`, etc.) and successfully exported the final image `pydeb`.

### Step 5: Verify and Run

We can verify the image exists and run a container from it.

**Check Image List:**

```bash
docker images
```

You should see `pydeb` in the list.

**Run a Container:**

```bash
docker run -it pydeb
```

This launches a container based on your new image. Since we didn't specify a `CMD` in our Dockerfile, it defaults to running `bash`.

### Step 6: Verify Running Container

Open a new terminal and run `docker ps` to see your active container.

```bash
hashim@debian:~$ docker ps
CONTAINER ID   IMAGE    COMMAND   CREATED          STATUS          PORTS     NAMES
8d133fd5f7f8   pydeb    "bash"    29 seconds ago   Up 28 seconds             gifted_chatelet
```

You now have a running container named `gifted_chatelet` (randomly assigned) built from your custom `pydeb` image\!