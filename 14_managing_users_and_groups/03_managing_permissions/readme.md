# 📁 **Linux File and Directory Permissions**

## Table of Contents

- [📁 **Linux File and Directory Permissions**](#-linux-file-and-directory-permissions)
  - [Table of Contents](#table-of-contents)
  - [🔐 Managing Permissions in Linux](#-managing-permissions-in-linux)
    - [🧰 What You’ll Learn:](#-what-youll-learn)
  - [📊 Figure 4.31 — The File Permission Attributes](#-figure-431--the-file-permission-attributes)
  - [📂 File and Directory Permissions Explained](#-file-and-directory-permissions-explained)
    - [🟢 **Read (r)**](#-read-r)
    - [🟡 **Write (w)**](#-write-w)
    - [🔵 **Execute (x)**](#-execute-x)
  - [🔎 Viewing Permissions](#-viewing-permissions)
    - [🔍 Breakdown:](#-breakdown)
  - [🗂 File Type Identifiers](#-file-type-identifiers)
  - [🧮 Octal Representation](#-octal-representation)
    - [✅ Verify using `stat`:](#-verify-using-stat)
  - [📋 Common Permission Examples](#-common-permission-examples)
- [🔧 **Changing Permissions**](#-changing-permissions)
  - [📌 Why Change Permissions?](#-why-change-permissions)
  - [🛠️ Using `chmod` — *Change Mode*](#️-using-chmod--change-mode)
    - [🔹 Syntax:](#-syntax)
  - [🔄 Modes of Changing Permissions](#-modes-of-changing-permissions)
  - [🔤 Permission Flags in Relative Mode](#-permission-flags-in-relative-mode)
  - [🧪 Example 1 — Add Write Permission to Others](#-example-1--add-write-permission-to-others)
    - [🎯 Goal: Allow all **other users (o)** to write to `myfile`.](#-goal-allow-all-other-users-o-to-write-to-myfile)
    - [✅ Command:](#-command)
    - [📟 Terminal Walkthrough:](#-terminal-walkthrough)
  - [🧪 Example 2 — Remove Read and Write from Owner](#-example-2--remove-read-and-write-from-owner)
    - [🎯 Goal: Remove **read and write** permission for the **owner (u)**](#-goal-remove-read-and-write-permission-for-the-owner-u)
    - [✅ Command:](#-command-1)
  - [🧪 Example 3 — Combined Multiple Changes](#-example-3--combined-multiple-changes)
    - [🎯 Goal:](#-goal)
    - [✅ Command:](#-command-2)
    - [📟 Output:](#-output)
  - [🔢 Permissions in **Absolute Mode**](#-permissions-in-absolute-mode)
  - [🧠 What Is Absolute Mode?](#-what-is-absolute-mode)
  - [🔢 Octal Value Reference Table](#-octal-value-reference-table)
  - [🔄 Structure of Octal Permissions](#-structure-of-octal-permissions)
  - [🧪 Example — Give Full Permissions to Everyone](#-example--give-full-permissions-to-everyone)
    - [🎯 Goal: Assign **read, write, and execute** permission to owner, group, and others for `myfile`.](#-goal-assign-read-write-and-execute-permission-to-owner-group-and-others-for-myfile)
    - [✅ Command:](#-command-3)
    - [📟 Terminal Session:](#-terminal-session)
    - [🧾 Explanation:](#-explanation)
  - [📌 More Common Octal Combinations](#-more-common-octal-combinations)
- [👤 **Changing File Ownership with `chown`**](#-changing-file-ownership-with-chown)
  - [📌 Why Change Ownership?](#-why-change-ownership)
  - [🔐 Who Can Use `chown`?](#-who-can-use-chown)
  - [🧰 `chown` Command Syntax](#-chown-command-syntax)
  - [🧪 Example — Change Owner and Group of a File](#-example--change-owner-and-group-of-a-file)
    - [🎯 Goal: Change file ownership of `myfile` to:](#-goal-change-file-ownership-of-myfile-to)
    - [✅ Command:](#-command-4)
    - [📟 Terminal Output:](#-terminal-output)
    - [🧾 Explanation:](#-explanation-1)
  - [🔄 Recursive Ownership Change](#-recursive-ownership-change)
    - [🎯 Goal: Make `julian` the owner of all files in `mydir/`, including subdirectories](#-goal-make-julian-the-owner-of-all-files-in-mydir-including-subdirectories)
    - [✅ Command:](#-command-5)
  - [📟 Full Recursive Example:](#-full-recursive-example)
    - [🔍 Output:](#-output-1)
- [👥 **Changing Group Ownership with `chgrp`**](#-changing-group-ownership-with-chgrp)
  - [🧠 Why Change Group Ownership?](#-why-change-group-ownership)
  - [🧰 `chgrp` Command Syntax](#-chgrp-command-syntax)
  - [🧪 Example — Change Group Ownership of a File](#-example--change-group-ownership-of-a-file)
    - [🎯 Goal: Set the group ownership of `myfile` to `developers`.](#-goal-set-the-group-ownership-of-myfile-to-developers)
    - [✅ Command:](#-command-6)
    - [📟 Terminal Session:](#-terminal-session-1)
  - [➕ Add User to a Group](#-add-user-to-a-group)
    - [🎯 Goal: Add user `julian` to the `developers` group.](#-goal-add-user-julian-to-the-developers-group)
    - [✅ Command:](#-command-7)
    - [🔍 Verify Group Membership:](#-verify-group-membership)
  - [⚠️ Why Use `sudo`?](#️-why-use-sudo)
- [🧮 **Understanding `umask` and Special Permissions**](#-understanding-umask-and-special-permissions)
  - [📌 What is `umask`?](#-what-is-umask)
    - [🔐 Default Permission Formulas:](#-default-permission-formulas)
  - [⚙️ Default `umask` Values](#️-default-umask-values)
    - [📊 Example: File \& Directory Permissions for Regular User](#-example-file--directory-permissions-for-regular-user)
  - [🔍 Verify with Terminal](#-verify-with-terminal)
    - [📂 File Creation Example](#-file-creation-example)
    - [📁 Directory Creation Example](#-directory-creation-example)
  - [🚨 Special Permissions in Linux](#-special-permissions-in-linux)
  - [🔑 `setuid` (Set User ID on Execution)](#-setuid-set-user-id-on-execution)
    - [✅ Command:](#-command-8)
    - [🧪 Terminal Example:](#-terminal-example)
  - [🔒 `setgid` (Set Group ID on Execution)](#-setgid-set-group-id-on-execution)
    - [✅ Command:](#-command-9)
    - [🧪 Terminal Example:](#-terminal-example-1)
  - [📌 `sticky` Bit (Directory Protection)](#-sticky-bit-directory-protection)
    - [✅ Command:](#-command-10)
    - [🧪 Terminal Example:](#-terminal-example-2)
  - [✅ Summary](#-summary)
  - [📚 References:](#-references)

---

Linux is a **multiuser, multitasking operating system**. One of the key pillars of its security model is **permission management**, which ensures that only authorized users can access or modify specific files or directories.

---

## 🔐 Managing Permissions in Linux

> Linux provides a robust framework to **allow multiple users** to access and operate independently, all while keeping operations secure through file and directory permissions.

### 🧰 What You’ll Learn:

* The three basic permission types: `read`, `write`, and `execute`
* How to **view** and **change** permissions
* Octal (numeric) and symbolic representations
* Real-world examples and interpretations

---

## 📊 Figure 4.31 — The File Permission Attributes

| **Entity**   | **Read (r)** = 4 | **Write (w)** = 2 | **Execute (x)** = 1 |
| ------------ | ---------------- | ----------------- | ------------------- |
| Owner/User   | ✅                | ✅                 | ✅                   |
| Group        | ✅                | ✅                 | ✅                   |
| Others/World | ✅                | ✅                 | ✅                   |

📌 Each permission is represented as a **bit** and its equivalent **octal value**:

* `r (read)` → 2² = **4**
* `w (write)` → 2¹ = **2**
* `x (execute)` → 2⁰ = **1**
* `- (no permission)` → 0

---

## 📂 File and Directory Permissions Explained

### 🟢 **Read (r)**

* **Files:** View contents.
* **Directories:** List contents using `ls`.

### 🟡 **Write (w)**

* **Files:** Modify or overwrite contents.
* **Directories:** Add, delete, or rename files inside.

### 🔵 **Execute (x)**

* **Files:** Run a script or binary file.
* **Directories:** Enter the directory with `cd`.

---

## 🔎 Viewing Permissions

Use the `ls` command with the `-l` option:

```bash
ls -l /etc/passwd
```

Example Output:

```
-rw-r--r-- 1 root root 2010 Mar  9 08:57 /etc/passwd
```

### 🔍 Breakdown:

| Field         | Meaning                     |
| ------------- | --------------------------- |
| `-rw-r--r--`  | **File access permissions** |
| `1`           | Number of hard links        |
| `root`        | Owner (user)                |
| `root`        | Group owner                 |
| `2010`        | File size                   |
| `Mar 9 08:57` | Last modified date          |
| `/etc/passwd` | File name                   |

---

## 🗂 File Type Identifiers

| Symbol | Meaning          |
| ------ | ---------------- |
| `-`    | Regular file     |
| `d`    | Directory        |
| `l`    | Symbolic link    |
| `p`    | Named pipe       |
| `s`    | Socket           |
| `b`    | Block device     |
| `c`    | Character device |

---

## 🧮 Octal Representation

The permissions string `-rw-r--r--` is broken down as:

| Entity | Symbolic | Octal           |
| ------ | -------- | --------------- |
| User   | `rw-`    | 4 + 2 = **6**   |
| Group  | `r--`    | 4       = **4** |
| Others | `r--`    | 4       = **4** |

➡️ **Total Octal Permission:** `644`

### ✅ Verify using `stat`:

```bash
stat --format '%a' /etc/passwd
```

Output:

```
644
```

---

## 📋 Common Permission Examples

| Symbolic    | Octal | Description                          |
| ----------- | ----- | ------------------------------------ |
| `rwxrwxrwx` | 777   | All permissions for everyone         |
| `rwxr-xr-x` | 755   | Owner full; others: read & execute   |
| `rwxr-x---` | 750   | Owner full; group: read & execute    |
| `rwx------` | 700   | Owner full access only               |
| `rw-rw-rw-` | 666   | Read & write for all                 |
| `rw-rw-r--` | 664   | Owner & group: rw; others: r         |
| `rw-rw----` | 660   | Owner & group: rw; others: no access |
| `rw-r--r--` | 644   | Owner: rw; group & others: r         |
| `rw-r-----` | 640   | Owner: rw; group: r; others: none    |
| `rw-------` | 600   | Owner only                           |
| `r--------` | 400   | Read only by owner                   |

---

# 🔧 **Changing Permissions**

Managing file and directory permissions is one of the **core administrative tasks** in Linux. This guide will help you understand how to change permissions using the `chmod` command in **relative mode**, with full breakdowns and real terminal examples.

---

## 📌 Why Change Permissions?

Linux is a **multi-user operating system**. Proper permission management ensures:

* 🔒 File security
* 👥 Multi-user access control
* 🧩 System stability

To control access, you’ll use tools like:

* **`chmod`** — Change file permission (mode)
* **`chown`** — Change ownership (explained in another topic)

---

## 🛠️ Using `chmod` — *Change Mode*

### 🔹 Syntax:

```bash
chmod [OPTIONS] [PERMISSION_CHANGE] FILE
```

---

## 🔄 Modes of Changing Permissions

There are **two modes** of using `chmod`:

| Mode        | Description                         |
| ----------- | ----------------------------------- |
| 🔄 Relative | Symbolic, easier, human-readable    |
| 🔢 Absolute | Uses octal numbers (e.g., 755, 644) |

In this README, we focus on both **relative mode** and **absolute mode**.

---

## 🔤 Permission Flags in Relative Mode

| Symbol | Meaning                 |
| ------ | ----------------------- |
| `u`    | User (owner)            |
| `g`    | Group                   |
| `o`    | Others                  |
| `a`    | All (`u`, `g`, `o`)     |
| `+`    | Add permission          |
| `-`    | Remove permission       |
| `=`    | Set exactly (overwrite) |
| `r`    | Read                    |
| `w`    | Write                   |
| `x`    | Execute                 |

---

## 🧪 Example 1 — Add Write Permission to Others

### 🎯 Goal: Allow all **other users (o)** to write to `myfile`.

### ✅ Command:

```bash
chmod o+w myfile
```

### 📟 Terminal Walkthrough:

```bash
# Create the file
touch myfile

# Check permissions
ls -l myfile
# -rw-r--r-- 1 root root 12 Aug 19 04:13 myfile

# Try writing to it as another user
su alex
echo "new" >> myfile
# ❌ bash: myfile: Permission denied

# Back to root
exit

# Add write permission for others
chmod o+w myfile

# Confirm permission change
ls -l myfile
# -rw-r--rw- 1 root root 12 Aug 19 04:13 myfile

# Switch back to alex
su alex
cat myfile
# Hello World

# Append new content
echo "New text" >> myfile

# Verify
cat myfile
# Hello World
# New text
```

---

## 🧪 Example 2 — Remove Read and Write from Owner

### 🎯 Goal: Remove **read and write** permission for the **owner (u)**

### ✅ Command:

```bash
chmod u-rw myfile
```

📌 No `sudo` was used because we were already logged in as the **owner (`root`)**.

---

## 🧪 Example 3 — Combined Multiple Changes

Assume `myfile` has `rwxrwxrwx` (777) permissions.

### 🎯 Goal:

* ❌ Remove **read** from owner (`u-r`)
* ❌ Remove **write** from owner & group (`ug-w`)
* ❌ Remove **all permissions** from others (`o-rwx`)

### ✅ Command:

```bash
chmod u-r,ug-w,o-rwx myfile
```

### 📟 Output:

```bash
ls -l myfile
# ----r----- 1 root root 21 Aug 19 04:18 myfile
```

🧠 Interpretation:

* No permissions for owner
* Only read for group
* No permissions for others

---

## 🔢 Permissions in **Absolute Mode**

## 🧠 What Is Absolute Mode?

Unlike relative mode (which **adds or removes** specific permissions), **absolute mode**:

* Replaces **all** permission bits at once
* Uses **numeric (octal)** representation
* Is ideal for scripting or setting exact permission states

---

## 🔢 Octal Value Reference Table

Here’s how each permission maps to an octal value:

| Octal | Symbolic | Meaning              |
| ----- | -------- | -------------------- |
| `7`   | `rwx`    | Read, Write, Execute |
| `6`   | `rw-`    | Read, Write          |
| `5`   | `r-x`    | Read, Execute        |
| `4`   | `r--`    | Read only            |
| `3`   | `-wx`    | Write, Execute       |
| `2`   | `-w-`    | Write only           |
| `1`   | `--x`    | Execute only         |
| `0`   | `---`    | No permissions       |

---

## 🔄 Structure of Octal Permissions

Absolute mode sets permissions in **three-digit octal format**:

```bash
chmod XYZ filename
```

| Position | Represents     | Example (7) |
| -------- | -------------- | ----------- |
| X        | User/Owner     | `rwx`       |
| Y        | Group          | `rwx`       |
| Z        | Others (World) | `rwx`       |

---

## 🧪 Example — Give Full Permissions to Everyone

### 🎯 Goal: Assign **read, write, and execute** permission to owner, group, and others for `myfile`.

### ✅ Command:

```bash
chmod 777 myfile
```

---

### 📟 Terminal Session:

```bash
# Initial state — very limited permissions
ls -l myfile
# ----r----- 1 root root 21 Aug 19 04:18 myfile

# Change all permissions to full access
chmod 777 myfile

# Verify the change
ls -l myfile
# -rwxrwxrwx 1 root root 21 Aug 19 04:18 myfile
```

### 🧾 Explanation:

* `7` = `rwx` for **owner**
* `7` = `rwx` for **group**
* `7` = `rwx` for **others**

---

## 📌 More Common Octal Combinations

| Octal | Symbolic    | Description                                |
| ----- | ----------- | ------------------------------------------ |
| `777` | `rwxrwxrwx` | Full access to everyone                    |
| `755` | `rwxr-xr-x` | Full for owner, read/execute for others    |
| `700` | `rwx------` | Full for owner, no access for others       |
| `644` | `rw-r--r--` | Read/write for owner, read-only for others |
| `600` | `rw-------` | Read/write for owner, no access for others |

---

# 👤 **Changing File Ownership with `chown`**

In Linux, **ownership** determines who can read, write, or execute a file. The `chown` command (short for **change owner**) is used to **change the owner and/or group** associated with files and directories.

---

## 📌 Why Change Ownership?

Sometimes, files are created or copied with incorrect ownership. To ensure the right user or group has access, Linux administrators use `chown`.

---

## 🔐 Who Can Use `chown`?

| User Type             | Allowed To...                                               |
| --------------------- | ----------------------------------------------------------- |
| 👑 Superuser (`root`) | Change owner and group of any file                          |
| 👤 Regular User       | Can change group **only if** they're a member of that group |
| ❌ Regular User        | Cannot change **owner** without `sudo`                      |

---

## 🧰 `chown` Command Syntax

```bash
chown [OPTIONS] [OWNER][:[GROUP]] FILE
```

* **OWNER** → Username of new file owner
* **GROUP** → Group name to assign (optional)
* Use `:` to separate owner and group

---

## 🧪 Example — Change Owner and Group of a File

### 🎯 Goal: Change file ownership of `myfile` to:

* **Owner:** `julian`
* **Group:** `developers`

### ✅ Command:

```bash
sudo chown julian:developers myfile
```

### 📟 Terminal Output:

```bash
root@dcd680177ef5:/home# sudo chown julian:developers myfile
root@dcd680177ef5:/home# ls -l myfile
-rw-r--r-- 1 julian developers 7 Aug 19 04:35 myfile
```

### 🧾 Explanation:

* `julian` is now the **owner**
* `developers` is now the **group**

---

## 🔄 Recursive Ownership Change

To **change ownership of all files** and **subdirectories** inside a folder, use the `-R` (or `--recursive`) option.

### 🎯 Goal: Make `julian` the owner of all files in `mydir/`, including subdirectories

### ✅ Command:

```bash
sudo chown -R julian:julian mydir/
```

---

## 📟 Full Recursive Example:

```bash
# Create directory structure
mkdir mydir
mkdir mydir/subdir
touch mydir/subdir/file1
touch mydir/subdir/file2
touch mydir/subdir/file3

# Apply recursive chown
sudo chown -R julian:julian mydir/

# View permissions
ls -lR mydir/
```

### 🔍 Output:

```bash
mydir/:
total 4
drwxr-xr-x 2 julian julian 4096 Aug 19 04:40 subdir

mydir/subdir:
total 0
-rw-r--r-- 1 julian julian 0 Aug 19 04:40 file1
-rw-r--r-- 1 julian julian 0 Aug 19 04:40 file2
-rw-r--r-- 1 julian julian 0 Aug 19 04:40 file3
```

📌 Now, **every file and directory** inside `mydir/` is owned by **julian\:julian**

---

# 👥 **Changing Group Ownership with `chgrp`**

In Linux, each file or directory has two types of ownership:

* **User (owner)** — managed via `chown`
* **Group** — managed via `chgrp`

---

## 🧠 Why Change Group Ownership?

* Useful for team collaboration
* Ensures group-level access control
* Helps organize file access by roles (e.g. `developers`, `admins`, `users`)

---

## 🧰 `chgrp` Command Syntax

```bash
chgrp [OPTIONS] GROUP FILE
```

* `GROUP`: The target group to assign ownership to
* `FILE`: The file or directory to update

---

## 🧪 Example — Change Group Ownership of a File

### 🎯 Goal: Set the group ownership of `myfile` to `developers`.

### ✅ Command:

```bash
sudo chgrp developers myfile
```

---

### 📟 Terminal Session:

```bash
root@dcd680177ef5:/home# sudo chgrp developers myfile
root@dcd680177ef5:/home# ls -l myfile
-rwxrwxrwx 1 julian developers 7 Aug 19 04:35 myfile
```

📌 Now, the group `developers` has ownership of `myfile`.

---

## ➕ Add User to a Group

If a user needs to access group-owned files, they must be a member of that group.

### 🎯 Goal: Add user `julian` to the `developers` group.

### ✅ Command:

```bash
sudo usermod -aG developers julian
```

### 🔍 Verify Group Membership:

```bash
groups julian
# Output:
julian : julian users developers
```

👑 Check root’s groups:

```bash
groups root
# Output:
root : root
```

---

## ⚠️ Why Use `sudo`?

Since the current user (`root`) is **not part of** the `developers` group, we must use `sudo` to modify group ownership or group memberships.

---

# 🧮 **Understanding `umask` and Special Permissions**

In Linux, **default permissions** for newly created files and directories are calculated using a **permission mask** called `umask`. Additionally, **special permission bits** like `setuid`, `setgid`, and `sticky bit` are used to control ownership and access in specific contexts.

---

## 📌 What is `umask`?

The `umask` command defines the **default permission restrictions** for newly created files and directories.

### 🔐 Default Permission Formulas:

| Type     | Formula        | Max Permission |
| -------- | -------------- | -------------- |
| **File** | `0666 - umask` | `rw-rw-rw-`    |
| **Dir**  | `0777 - umask` | `rwxrwxrwx`    |

---

## ⚙️ Default `umask` Values

* **Regular users**: `0002`
* **Root user**: `0022`

### 📊 Example: File & Directory Permissions for Regular User

| Type      | Formula       | Result |
| --------- | ------------- | ------ |
| File      | `0666 - 0002` | `0664` |
| Directory | `0777 - 0002` | `0775` |

---

## 🔍 Verify with Terminal

### 📂 File Creation Example

```bash
touch myfile2
stat --format '%a' myfile2
```

✅ Output:

```
664
```

---

### 📁 Directory Creation Example

```bash
mkdir mydir2
stat --format '%a' mydir2
```

✅ Output:

```
775
```

---


## 🚨 Special Permissions in Linux

Some Linux use-cases require **special access behavior**, which is controlled using **special permission bits**:

| Special Permission | Bit | Octal Value | Purpose                         |
| ------------------ | --- | ----------- | ------------------------------- |
| `setuid`           | s   | 4           | Execute file with owner's UID   |
| `setgid`           | s   | 2           | Execute file with group's GID   |
| `sticky`           | t   | 1           | Restrict deletion to file owner |

These bits appear as an **extra leading digit** in the octal representation (e.g., `4755`, `2755`, `1775`).

---

## 🔑 `setuid` (Set User ID on Execution)

Runs an executable **with the file owner's privileges**.

### ✅ Command:

```bash
chmod u+s myscript.sh
```

### 🧪 Terminal Example:

```bash
touch myscript.sh
chmod +x myscript.sh
chmod u+s myscript.sh
ls -l myscript.sh
# -rwsr-xr-x 1 root root 0 Aug 19 05:04 myscript.sh
stat --format '%a' myscript.sh
# 4755
```

📌 Notice the `s` replacing the user's `x` in the permission string.

---

## 🔒 `setgid` (Set Group ID on Execution)

Runs an executable **with the file group’s privileges**.
Also affects **group inheritance in directories**.

### ✅ Command:

```bash
chmod g+s myscript.sh
```

### 🧪 Terminal Example:

```bash
chmod g+s myscript.sh
ls -l myscript.sh
# -rwsr-sr-x 1 root root 0 Aug 19 05:04 myscript.sh
stat --format '%a' myscript.sh
# 2755
```

---

## 📌 `sticky` Bit (Directory Protection)

Ensures that **only the file owner** can delete/rename files inside a shared directory.

### ✅ Command:

```bash
chmod +t mydir/
```

### 🧪 Terminal Example:

```bash
mkdir mydir
chmod +t mydir
ls -ld mydir/
# drwxr-xr-t 2 root root 4096 Aug 19 05:09 mydir/
stat --format '%a' mydir/
# 1755
```

---

## ✅ Summary

| Concept           | Command Example        | Octal        |
| ----------------- | ---------------------- | ------------ |
| Default File Mode | `umask` + `touch file` | 0666 - umask |
| Default Dir Mode  | `umask` + `mkdir dir`  | 0777 - umask |
| Set `setuid`      | `chmod u+s file`       | +4000        |
| Set `setgid`      | `chmod g+s file/dir`   | +2000        |
| Set `sticky`      | `chmod +t dir`         | +1000        |

---

## 📚 References:

* 📄 [`man chmod`](https://man7.org/linux/man-pages/man1/chmod.1.html)
* 📄 [`man umask`](https://man7.org/linux/man-pages/man1/umask.1.html)
* 🌐 [Oracle Docs on `setuid`](https://docs.oracle.com/cd/E19683-01/816-4883/secfile-69/index.html)

---
