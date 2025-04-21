#  **The Linux Shell with Docker** 🚀

1. 📥 Pull and run an Ubuntu container from docker
2. 👤 Check your username inside the container
3. 🐚 Verify your default shell

Let's dive in! 🎉

---

## 🐳 1. Pull & Run Ubuntu Container

Now we’ll fetch the latest Ubuntu image and launch it:

1. **Open PowerShell or Windows Terminal or vs code command prompt as a normal user.**

2. **Pull the Ubuntu image**:

   ```bash
   docker pull ubuntu:latest
   ```

   - This downloads the official Ubuntu Docker image.

3. **Run the container interactively**:

   ```bash
   docker run -it --name my-ubuntu ubuntu:latest bash
   ```

   - `-it`: Interactive terminal mode
   - `--name my-ubuntu`: Assigns the name `my-ubuntu` to your container
   - `bash`: Launches the Bash shell inside the container

4. **You’ll see a prompt** like:

   ```bash
   root@<container-id>:/#
   ```

   - This indicates you’re inside the Ubuntu container!

---

## 👤 2. Check Your Username

Inside the container, find out which user you’re logged in as:

- **Method A**: Using `whoami`

  ```bash
  whoami
  ```

  - Prints your current username (usually `root` by default).

🔍 **Why this matters:** Knowing your user helps you manage permissions and understand which account you’re using for commands.

---

## 🐚 3. Verify Your Default Shell

To confirm which shell binary you’re running:

- **Quick check**:

  ```bash
  echo $0
  ```

  - Displays the name of the shell process (e.g., `-bash`).

- **Deep dive**: Inspect your `/etc/passwd` entry using `cat` and `grep`
  ```bash
  cat /etc/passwd | grep root
  ```
  - This streams the entire `/etc/passwd` file into `grep`, which filters and displays only the lines containing “root.”
  - You should see an entry like:
    ```text
    root:x:0:0:root:/root:/bin/bash
    ```
  - The **last field** (`/bin/bash`) shows your default shell path.
  - Using `cat /etc/passwd | grep root` is a simple way to verify your root user’s shell without needing to construct dynamic patterns.

> 🔑 **Key point:** The default shell determines how commands are interpreted. Bash is the most common on Linux.

---

## 🎯 Next Steps

- Practice basic Linux commands: `ls`, `cd`, `mkdir`, `apt update`, etc.
- Explore file permissions and editing files with `nano` or `vi`.
- When ready, exit the container:
  ```bash
  exit
  ```
- To restart later:
  ```bash
  docker start my-ubuntu
  docker attach my-ubuntu
  ```

---

# 🚀 Starting or Accessing the Ubuntu Container

To work with the `my-ubuntu` container, use one of these commands depending on its state:

### 1. Start an Existing Container

If the `my-ubuntu` container is stopped, start it in interactive mode:

```bash
docker start -ai my-ubuntu
```

#### 🔍 Command Breakdown

- `docker start`: Starts a stopped container.
- `-ai`: Attaches the terminal in interactive mode (`-i`) with a pseudo-TTY (`-a`).
- `my-ubuntu`: The name of the container.

### 2. Access a Running Container

If the `my-ubuntu` container is already running, open a new Bash session inside it:

```bash
docker exec -it my-ubuntu bash
```

#### 🔍 Command Breakdown

- `docker exec`: Runs a command inside a running container.
- `-it`: Enables interactive mode with a terminal.
- `my-ubuntu`: The name of the container.
- `bash`: Launches the Bash shell.

Upon starting or accessing, you'll see a prompt like:

```bash
root@f69c4124e2e7:/#
```

This indicates you're the `root` user in the container's root directory (`/`). The string `f69c4124e2e7` is part of the container's unique ID.

## 📋 Checking Installed Shells

To list all valid login shells installed in the container, run:

```bash
cat /etc/shells
```

### 📜 Sample Output

The output will resemble:

```plaintext
# /etc/shells: valid login shells
/bin/sh
/usr/bin/sh
/bin/bash
/usr/bin/bash
/bin/rbash
/usr/bin/rbash
/usr/bin/dash
```

### 🧠 Understanding the Output

The `/etc/shells` file lists shells that can be used as login shells for users. Here's what each shell means:

- `/bin/sh`: The Bourne shell, a lightweight shell often used for scripting. 🛠️
- `/bin/bash`: The Bourne Again Shell, a feature-rich shell and the default in most Ubuntu systems. 🌟
- `/bin/rbash`: Restricted Bash, a limited version of Bash for enhanced security. 🔒
- `/usr/bin/dash`: The Debian Almquist Shell, a fast and lightweight shell for system scripts. ⚡
- Paths like `/usr/bin/sh` may be symbolic links to other shells (e.g., `/bin/sh`).

---

# 🔍 **Shell Connections: TTY vs. PTS**

Shell connections link users to the system’s shell. There are two main types:

### 1. TTY (Teletypewriter) 🖨️

- **Definition**: A native connection directly tied to the computer’s hardware, such as physical or virtual consoles.
- **Characteristics**: Historically linked to teletypewriters, TTYs are used for direct terminal access via keyboards or serial ports.
- **Example**: Physical console access or virtual terminals (e.g., `Ctrl+Alt+F1` on Linux).

### 2. PTS (Pseudo Terminal Slave) 🌐

- **Definition**: An emulated connection created by software like SSH, Telnet, or terminal emulators (e.g., xterm).
- **Characteristics**: PTS is the "slave" part of a pseudo-terminal pair (PTY), where a program (e.g., SSH client) acts as the master to manage input/output.
- **Example**: SSH sessions or GUI terminal windows.

### Key Difference

- **TTY**: Native, hardware-based, used for direct console access.
- **PTS**: Emulated, software-based, used for remote or virtual terminal sessions.

## 🐳 Docker Context

In Docker containers:

- **PTS Dominates**: Containers typically use PTS connections because they run in virtualized environments and rely on pseudo-terminals allocated by flags like `-it`.
- **TTY Rare**: True TTY connections (hardware-based) are uncommon in containers due to their isolated nature.

## 🚀 Checking Shell Connections in Docker

To identify whether you're using a TTY or PTS connection in your Ubuntu container, follow these steps:

### 1. Start or Access the Container

- **Start a Stopped Container**:

  ```bash
  docker start -ai my-ubuntu
  ```

  - `-ai`: Attaches the terminal in interactive mode.

- **Access a Running Container**:

  ```bash
  docker exec -it my-ubuntu bash
  ```

  - `-it`: Allocates a pseudo-terminal (PTS) and opens a Bash shell.

### 2. Identify the Terminal

Inside the container, run:

```bash
tty
```

**Sample Output**:

```
/dev/pts/0
```

This indicates a **PTS** connection, as Docker’s `-t` flag allocates a pseudo-terminal.

### 3. List Terminal Devices

To see available terminal devices:

```bash
ls /dev
```

Look for `/dev/pts/` entries (e.g., `pts/0`, `pts/1`), which represent PTS devices. A `/dev/tty` may appear but is typically symbolic in containers.