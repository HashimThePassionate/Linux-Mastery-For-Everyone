# **The Command-Line Prompt** 🖥️

# Table of Contents 🐧

- [The Command-Line Prompt](#the-command-line-prompt) 🖥️
  - [Examples of Prompts](#examples-of-prompts)
- [Shell Command Types](#-shell-command-types) 🛠️
  - [Internal Commands](#1-internal-commands)
  - [External Commands](#2-external-commands)
  - [Checking Command Types](#checking-command-types)
- [Docker Context](#-docker-context) 🐳
- [Checking Prompt and Command Types in Docker](#-checking-prompt-and-command-types-in-docker) 🚀
- [Linux Command Structure](#-linux-command-structure) 📖
  - [Components](#components)
  - [Example](#example)
- [The `ls` Command](#-the-ls-command) 📋
  - [Basic Usage](#basic-usage)
  - [Using Options: `-l` for Long Listing](#using-options--l-for-long-listing)
  - [Using Arguments: Targeting a Directory](#using-arguments-targeting-a-directory)
- [Docker Context](#-docker-context-1) 🐳
- [Consulting Linux Manual Pages](#-consulting-linux-manual-pages) 📖
  - [What Are Manual Pages?](#-what-are-manual-pages) 📚
  - [Manual Page Structure](#-manual-page-structure) 🗂️
  - [Quick Reference with `--help`](#-quick-reference-with---help) 🚀
  - [Docker Context](#-docker-context-2) 🐳
  - [Installing Man Pages in Docker](#installing-man-pages-in-docker)
  - [Exploring a Manual Page: Example with `pwd`](#-exploring-a-manual-page-example-with-pwd) 🔍
  - [Practical Usage in Docker](#-practical-usage-in-docker) 🛠️
  - [Why Manual Pages Matter](#-why-manual-pages-matter) 🌟
  - [Tips for Using Man Pages](#-tips-for-using-man-pages) 💡

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

# 📖 **Consulting Linux Manual Pages** 🐧

The **manual pages** (man pages) are a Linux system administrator's best friend, providing detailed documentation for every command. 

## 📚 What Are Manual Pages?

- **Definition**: Each Linux command has a manual page that details its usage, options, arguments, and more.
- **Access**: Use the `man` command followed by the command name to view its manual. For example:

  ```bash
  man ls
  ```
- **Purpose**: Man pages are technical documentation, offering comprehensive information about commands, their behavior, and related metadata.

## 🗂️ Manual Page Structure

Manual pages are organized into standardized sections, consistent across Linux distributions (e.g., Ubuntu, Fedora). Key sections include:

- **NAME**: Command name and a brief description.
- **SYNOPSIS**: Command syntax, showing how to use it with options and arguments.
- **DESCRIPTION**: Detailed explanation of what the command does.
- **OPTIONS**: Available options and their effects.
- **EXAMPLES**: Usage examples (sometimes included).
- **AUTHOR**: Who wrote the command.
- **COPYRIGHT**: Licensing information.
- **SEE ALSO**: Related commands or resources.

## 🚀 Quick Reference with `--help`

For a concise overview of a command, use the `--help` option, available for most commands:

```bash
ls --help
```

This displays a brief summary of the command's usage and options, ideal for quick reference.

## 🐳 Docker Context

In a Dockerized Ubuntu container (e.g., `ubuntu:latest`):

- **Minimal Environment**: Man pages are not installed by default to keep the image lightweight.
- **Installation**: You need to install the `man-db` and `manpages` packages to enable manual pages.
- **Prompt**: The container's prompt (e.g., `root@f69c4124e2e7:/#`) indicates you're working in the container's root directory as the root user.

### Installing Man Pages in Docker

To use man pages in your Ubuntu container:

1. **Update the Package List**:

   ```bash
   apt update
   ```

2. **Install Man Pages**:

   ```bash
   apt install man-db manpages
   ```

3. **Optional: Unminimize the Container**: If the container is minimized (common in `ubuntu:latest`), run:

   ```bash
   unminimize
   ```

   This installs additional documentation and tools, including man pages.

## 🔍 Exploring a Manual Page: Example with `pwd`

The `pwd` command (print working directory) is a great example to explore via its manual page.

### Command

```bash
man pwd
```

### Sample Output (Summarized)

```
PWD(1)                       User Commands                       PWD(1)

NAME
       pwd - print name of current/working directory

SYNOPSIS
       pwd [OPTION]...

DESCRIPTION
       Print the full filename of the current working directory.

       -L, --logical
              use PWD from environment, even if it contains symlinks

       -P, --physical
              avoid all symlinks

       --help
              display this help and exit

       --version
              output version information and exit

AUTHOR
       Written by Jim Meyering.

COPYRIGHT
       Copyright (C) 2023 Free Software Foundation, Inc. License GPLv3+.

SEE ALSO
       getcwd(3)
```

### Output Breakdown

- **NAME**: `pwd` prints the current directory's full path.
- **SYNOPSIS**: `pwd [OPTION]...` shows optional arguments.
- **DESCRIPTION**: Explains that `pwd` outputs the working directory's path.
- **OPTIONS**:
  - `-L`: Uses the environment's path, including symlinks.
  - `-P`: Shows the physical path, resolving symlinks.
  - `--help`: Displays help.
  - `--version`: Shows version info.
- **NOTE**: The shell may have its own `pwd` version, overriding the default.
- **SEE ALSO**: References related functions like `getcwd(3)`.

## 🛠️ Practical Usage in Docker

### 1. Access the Container

Start or access your Ubuntu container:

```bash
docker start -ai my-ubuntu
```

Or open a new shell session:

```bash
docker exec -it my-ubuntu bash
```

### 2. Install Man Pages

If not already installed:

```bash
apt update
apt install man-db manpages
```

### 3. View a Manual Page

Check the manual for `pwd`:

```bash
man pwd
```

Navigate the manual using:

- **Arrow keys**: Move up/down.
- **/**: Search for text.
- **q**: Quit the manual.

### 4. Use `--help` for Quick Reference

For a quick overview:

```bash
pwd --help
```

## 🌟 Why Manual Pages Matter

- **Reliable Source**: Man pages are the most authoritative documentation for Linux commands.
- **Offline Access**: Ideal for environments with limited or no internet access.
- **Skill Development**: Regular use of man pages improves your Linux proficiency, as they provide firsthand, technical insights.
- **Not a Tutorial**: Man pages are technical, not step-by-step guides. Practice reading them to become comfortable with their format.

## 💡 Tips for Using Man Pages

- **Start with** `--help`: Use it for quick command summaries.
- **Practice Regularly**: Check man pages before searching online to build familiarity.
- **Explore Sections**: Focus on **DESCRIPTION** and **OPTIONS** for practical usage.
- **Docker-Specific**: Always ensure man pages are installed in containers, as they’re not included by default.

---
