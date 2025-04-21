# **The Command-Line Prompt** 🖥️

The **command-line prompt** (or shell prompt) is where you enter commands. It typically displays:

- **Username**: The current user (e.g., `packt`).
- **Hostname**: The system’s name (e.g., `saturn` or a container ID).
- **Current Directory**: The working directory (e.g., `~` for home).
- **User Indicator**: `$` for regular users, `#` for root users.

### Examples of Prompts

- **Ubuntu/Debian**: `packt@saturn:~$` (`$` for regular user, `#` for root).
- **Fedora/RHEL**: `[packt@localhost ~]$`.
- **openSUSE**: `packt@localhost:~>` (`>` for regular, `#` for root).
- **Docker (Ubuntu Container)**: `root@f69c4124e2e7:/#`
  - `root`: The user.
  - `f69c4124e2e7`: Container ID (acts as hostname).
  - `/`: Current directory (root directory).
  - `#`: Indicates root user.

## 🛠️ Shell Command Types

Shell commands are divided into two categories:

### 1. Internal Commands

- **Definition**: Built into the shell, no separate installation required.
- **Examples**: `cd` (change directory), `echo` (print text), `pwd` (print working directory).
- **Characteristics**: Fast and always available within the shell.

### 2. External Commands

- **Definition**: Separate programs installed on the system, located in directories like `/usr/bin`.
- **Examples**: `date` (display date), `touch` (create files), `man` (view manuals).
- **Characteristics**: Depend on system installation, more feature-rich.

### Checking Command Types

Use the `type` command to identify a command’s type:

```bash
type cd
```

**Output**: `cd is a shell builtin` (internal).

```bash
type date
```

**Output**: `date is /usr/bin/date` (external).

## 🐳 Docker Context

In a Dockerized Ubuntu container:

- **Prompt**: Shows `root@<container-id>:/#`, indicating the root user and container-specific ID.
- **Command Types**: Containers have a minimal environment, so some external commands may not be available unless installed. Internal commands are always present.
- **Use Case**: Understanding the prompt helps navigate the container’s environment, and checking command types ensures scripts or commands are supported.

## 🚀 Checking Prompt and Command Types in Docker

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

  - `-it`: Allocates a pseudo-terminal and opens a Bash shell.

### 2. Observe the Prompt

Upon entering, you’ll see:

```
root@f69c4124e2e7:/#
```

This confirms you’re the root user in the container’s root directory.

### 3. Check Command Types

Run the `type` command for various commands:

```bash
type cd
```

**Output**: `cd is a shell builtin`.

```bash
type ls
```

**Output**: `ls is aliased to 'ls --color=auto'` (points to an external command).

```bash
type date
```

**Output**: `date is /usr/bin/date`.

```bash
type echo
```

**Output**: `echo is a shell builtin`.

---