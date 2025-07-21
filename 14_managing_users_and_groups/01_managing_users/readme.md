#  **Managing Users in Linux** 📋

A **user** is anyone using a computer or a system resource. In Linux, every user account is identified by:

- **Name** (e.g. `alice`, `bob`)
- **UID** (User Identifier, a unique number)

## 🎯 Purpose

- Organize access to files and applications  
- Isolate processes for security  
- Delegate limited or full privileges to different accounts  

## 🔍 Types of Users

1. ### 👤 Normal (Regular) Users  
   - **Who:** You, your friends, colleagues  
   - **Use case:** Personal work, running common applications, file management  
   - **Features:**  
     - Login shell (e.g. `/bin/bash`)  
     - Home directory (`/home/username`)  
     - Limited access to system-wide resources  

2. ### 🛠️ System Users  
   - **Who:** Background services (web servers, database daemons)  
   - **Use case:** Run daemons or services with minimal privileges  
   - **Features:**  
     - May **lack** a login shell  
     - May **lack** a home directory  
     - Isolated privileges to reduce attack surface  

3. ### 🚀 Superusers  
   - **Who:** Administrators (e.g. `root`)  
   - **Use case:** Full system control—install software, manage users, change configurations  
   - **Features:**  
     - UID 0  
     - Unlimited access to **all** resources  

## 🔐 Privileges & Administration

- Only **root** or users in the **`sudoers`** file can:
  - Create new user accounts  
  - Modify existing user accounts  
  - Delete user accounts  

> **Pro tip:** Always use `sudo` for administrative tasks rather than logging in directly as `root`, to keep an audit trail and reduce risk.  

# 🔑 Understanding `sudo` in Linux

In Linux, the **root** user is the default superuser account with unrestricted privileges—able to read, write, install, remove, and modify **any** file or setting. However, working as root all the time is dangerous: a single typo can break your system or open security holes. 

`sudo` (“substitute user do” or more commonly “superuser do”) offers a safer way:  
- **Promotes** a regular user to superuser (or another user) privileges  
- Adds an **extra layer** of security by requiring your own password and caching your credentials  
- **Limits** the time window during which elevated privileges remain active  


## 🛡️ Why Avoid Direct `root` Logins?

- **Accidental damage:** One misplaced command (`rm -rf /`) can wipe your system.  
- **Audit trail:** Actions under `sudo` are logged (e.g. `/var/log/auth.log`), whereas root actions are less traceable.  
- **Least privilege principle:** Work as a normal user by default; elevate privileges **only** when needed.  

## ⚙️ What `sudo` Does

1. **Authentication**  
   - Prompts you for your **own** password (not root’s) to confirm identity.  
2. **Authorization**  
   - Checks `/etc/sudoers` (and any files in `/etc/sudoers.d/`) to see which commands you’re allowed to run.  
3. **Execution**  
   - Runs the requested command with root (UID 0) or another specified user’s privileges.  
4. **Credential Caching**  
   - Temporarily remembers your successful authentication for a short time (default 15 minutes on Ubuntu).  

## 🔍 Checking Your `sudo` Privileges

While logged in as your regular user (e.g. `julian` on host `neptune`), run:

```bash
sudo -v
````

* **`-v` (validate):**

  * Updates your cached credentials if they’re still valid
  * Prompts for your password if the cache has expired
* **Success:** No output (silently refreshed)
* **Failure:**

  ```text
  Sorry, user julian may not run sudo on neptune.
  ```

  This means your account is **not** in the sudoers list.


## ⏳ `sudo` Timeout Behavior

* After authenticating once, you can run additional `sudo` commands **without re-entering** your password for a limited span.
* **Ubuntu’s default:** 15 minutes
* After the timeout, the next `sudo` command will prompt you to authenticate again.

---


#  **Creating, Modifying, and Deleting Users** 👥

In this section, we explore the command-line tools and workflows for **creating**, **modifying**, and **deleting** user accounts on Linux. Examples are shown for Ubuntu and Fedora, but the same concepts apply to most distributions (check your distro’s docs for equivalents).

## 🎯 Why User Management Matters

- **Security & Isolation**  
  Each user gets their own UID/GID, home directory, and permissions—so processes and files remain isolated.  
- **Least-Privilege Principle**  
  Grant only the access each account needs: normal users for day-to-day work, system users for daemons, and sudoers for administrative tasks.  
- **Audit & Accountability**  
  Track who did what! User actions can be logged, and sudoers logs stamp commands with usernames and timestamps.

## 🛠️ Creating Users

Linux provides two main CLI tools:

1. **`useradd`** – A low-level utility that creates a user record (more granular control).  
2. **`adduser`** – A friendlier Perl wrapper around `useradd` (guided prompts).

Both are installed by default on Ubuntu and Fedora. On Alpine Linux, use `adduser` instead of `useradd`.

### 1. Creating Users with `useradd`

#### Syntax
```bash
sudo useradd [OPTIONS] USERNAME
````

#### Basic Usage

```bash
sudo useradd julian
```

* **What it does:**

  * Adds a record for `julian` in `/etc/passwd`
  * **Does not** create the home directory by default
  * Leaves GECOS/comment, password, and shell at defaults

#### Inspecting the Entry

```bash
sudo cat /etc/passwd | grep julian
# julian:x:1003:1003::/home/julian:/bin/sh
```

Each colon-separated field means:

1. `julian` → **Username**
2. `x` → **Password placeholder** (actual hash lives in `/etc/shadow`)
3. `1003` → **UID**
4. `1003` → **GID**
5. (empty) → **GECOS/comment** (full name, etc.)
6. `/home/julian` → **Home directory** (not created yet!)
7. `/bin/sh` → **Login shell**

Alternatively, use `getent` and `id`:

```bash
getent passwd julian
# julian:x:1003:1003::/home/julian:/bin/sh

id julian
# uid=1003(julian) gid=1003(julian) groups=1003(julian)
```

#### Create Home Directory

By default, `useradd` does *not* make `/home/julian`. To create it:

```bash
sudo useradd -m julian
```

* `-m` / `--create-home` → create the home directory with default skeleton files from `/etc/skel`

#### Specify Full Name (GECOS Field)

Add a “comment” (GECOS) to store the full name:

```bash
sudo useradd -m -c "Julian" julian
```

* `-c` / `--comment "TEXT"` → populates the GECOS field (e.g. display name)

#### Set User Password

Until you set a password, the user cannot log in (SSH/GUI). Do:

```bash
sudo passwd julian
# New password: 
# Retype new password: 
# passwd: password updated successfully
```

* Updates `/etc/shadow` with the user’s hashed password
* Only root/sudoers can read `/etc/shadow`

```bash
sudo getent shadow julian
# julian:$6$…hashed…:18295:0:99999:7:::
```

### 2. Creating Users with `adduser`

`adduser` is a higher-level script that wraps `useradd` and walks you through each step.

#### Syntax

```bash
sudo adduser USERNAME
```

#### Interactive Example

```bash
sudo adduser alex
```

* Prompts you for:

  1. **Password** (and confirmation)
  2. **Full Name**, **Room Number**, **Work Phone**, **Home Phone**, **Other**
  3. Confirmation of details
* Automatically:

  * Creates `/home/alex` and copies `/etc/skel` files
  * Creates a private group `alex`
  * Adds `alex` to the standard `users` group

Inspect with:

```bash
getent passwd alex
# alex:x:1006:1006:,,,:/home/alex:/bin/bash
```

> **Note:** On Fedora, `adduser` may run non-interactively—check your distro’s behavior.

---



