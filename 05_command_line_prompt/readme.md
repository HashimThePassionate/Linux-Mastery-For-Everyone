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

# 📖 **Linux Command Structure**🐧

## 🛠️ Linux Command Structure

Linux commands follow a consistent structure, making them flexible and powerful. The general format is:

```
command [-option(s)] [argument(s)]
```

### Components

- **Command Name**: The name of the command, e.g., `ls` (list directory contents).
- **Options**: Modify how the command behaves, typically prefixed with a hyphen (`-`), e.g., `-l` for long listing. Options are optional.
- **Arguments**: Specify what the command operates on, such as a file or directory name, e.g., `/var/`. Arguments are also optional.

### Example

The `ls` command can be used in various ways:

- Without options or arguments: Lists the current directory's contents.
- With options: Provides additional details or customizes output.
- With arguments: Targets a specific directory or file.

## 📋 The `ls` Command

The `ls` command (short for "list") displays the contents of a directory. It's a fundamental tool for exploring the filesystem.

### Basic Usage

Running `ls` without options or arguments lists the files and directories in the current working directory (check with `pwd`).

**Command**:

```bash
ls
```

**Sample Output** (in a Docker container's root directory):

```
bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```

This shows all files and directories in the current directory (`/` in the container).

### Using Options: `-l` for Long Listing

The `-l` option (lowercase L) displays a detailed, long listing format, including permissions, ownership, size, and modification date.

**Command**:

```bash
ls -l
```

**Sample Output** (shortened for brevity):

```
lrwxrwxrwx   1 root root    7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root 4096 Apr 22  2024 boot
drwxr-xr-x   5 root root  360 Apr 21 09:02 dev
drwxr-xr-x   1 root root 4096 Apr 21 08:03 etc
...
```

**Output Breakdown**:

- `lrwxrwxrwx`: File permissions (e.g., `l` for link, `drwxr-xr-x` for directory).
- `root root`: Owner and group.
- `4096`: Size in bytes.
- `Apr 22 2024`: Last modified date.
- `bin -> usr/bin`: File or directory name (with links indicated by `->`).

### Using Arguments: Targeting a Directory

Arguments let you list the contents of a specific directory without changing your current working directory.

**Command**:

```bash
ls -l /var/
```

**Sample Output** (shortened):

```
drwxr-xr-x 2 root root 4096 Apr 22 2024 backups
drwxr-xr-x 5 root root 4096 Apr  4 02:09 cache
drwxr-xr-x 7 root root 4096 Apr  4 02:04 lib
...
```

Here, `-l` provides detailed output, and `/var/` specifies the target directory.

## 🐳 Docker Context

In a Dockerized Ubuntu container:

- **Filesystem**: The container's filesystem is isolated from the host, based on the `ubuntu:latest` image. The `ls` command shows the container's directory contents.
- **Prompt**: The prompt (e.g., `root@f69c4124e2e7:/#`) indicates you're in the root directory (`/`) as the root user, with `f69c4124e2e7` being the container ID.
- **Minimal Environment**: Containers are lightweight, so some directories (e.g., `/var/`) may have fewer contents than a full Ubuntu system.

---