# 🛡️ Understanding Linux Security

One significant consideration for securing a computer system or network is the means for system administrators to control how users and processes can access various resources, such as files, devices, and interfaces, across systems. The Linux kernel provides a handful of such mechanisms, collectively referred to as **Access Control Mechanisms (ACMs)**.

Let’s describe them in detail.


## 🔑 Discretionary Access Control (DAC)

**Discretionary Access Control (DAC)** is the standard, most common ACM related to filesystem objects (files, directories, and devices).

This model is called "discretionary" because access to an object is at the **discretion of the object’s owner**. Access is controlled based on the identity of users and groups (subjects). The owner of a file can decide who gets read, write, and execute permissions.

Depending on a subject’s access permissions, they could also pass those permissions on to other subjects (for example, an administrator managing regular users).

### Practical Example (Standard `chmod` Permissions)

DAC is the `rwx` permission model you use every day. When you run `ls -l`, you are looking at DAC.

```bash
$ ls -l /data/report.txt
-rw-r--r-- 1 hashim dev_team 4096 Nov 11 09:15 /data/report.txt
```

  * **Owner (`hashim`):** Has `rw-` (read, write) permissions.
  * **Group (`dev_team`):** Has `r--` (read-only) permissions.
  * **Others:** Have `r--` (read-only) permissions.

Because this is **discretionary**, the owner (`hashim`) has the authority to change these permissions at any time:

```bash
# The owner, hashim, decides to remove all access for "Others"
$ chmod o-r /data/report.txt

# The new permissions are now at the owner's discretion
$ ls -l /data/report.txt
-rw- 1 hashim dev_team 4096 Nov 11 09:16 /data/report.txt
```


## 🗂️ Access Control Lists (ACLs)

**Access Control Lists (ACLs)** are an extension of DAC. They provide a more granular way to control which subjects (such as users and groups) have access to specific filesystem objects (such as files and directories).

DAC is limited: you only have *one* owner and *one* group. What if you want to give a *single, different user* (e.g., `auditor_bob`) read access, but you don't want to add him to the `dev_team` group? ACLs solve this.

### Practical Example (`setfacl` and `getfacl`)

Using the same file, let's give `auditor_bob` read access.

**1. Check the default ACL (it just shows the DAC permissions):**

```bash
$ getfacl /data/report.txt
# file: data/report.txt
# owner: hashim
# group: dev_team
user::rw-
group::r--
other::---
```

**2. Add a new ACL entry for `auditor_bob`:**

```bash
$ setfacl -m u:auditor_bob:r-- /data/report.txt
```

**3. Check the ACL again:**

```bash
$ getfacl /data/report.txt
# file: data/report.txt
# owner: hashim
# group: dev_team
user::rw-
user:auditor_bob:r--   <-- NEW, GRANULAR PERMISSION
group::r--
mask::r--
other::---
```

Now, `ls -l` will show a `+` symbol to indicate that an ACL is active:

```bash
$ ls -l /data/report.txt
-rw-+ 1 hashim dev_team 4096 Nov 11 09:17 /data/report.txt
```


## 🏷️ Mandatory Access Control (MAC)

**Mandatory Access Control (MAC)** provides different, system-wide access control levels to subjects over the objects they own.

Unlike DAC (where users have full control over the files they own), MAC adds **additional labels** or **categories** to *all* filesystem objects. This access is *not* discretionary; it is "mandatory" and enforced by the kernel based on a central policy.

Consequently, subjects (like a user or a web server process) must have the appropriate access to these categories to interact with the objects labeled as such.

> **Simple Analogy:**
>
>   * **DAC** is the **key to your house**. You own it, you can make copies, and you decide who gets a key.
>   * **MAC** is a **government security clearance**. Even if you have the key to a house (DAC), if that house is on a military base (MAC), you *also* need the correct security clearance to even get to the front door. The owner of the house cannot just "give" you their clearance.

MAC is enforced by:

  * **SELinux** (Security-Enhanced Linux) on RHEL/Fedora systems.
  * **AppArmor** on Ubuntu/Debian/openSUSE systems.

### Practical Example (SELinux Contexts)

A common example is a web server. Even if you `chmod 777` a file in your home directory, a web server process (like Apache) running under SELinux *cannot* read it.

Why? Because the MAC policy says the web server process (labeled `httpd_t`) can only read files labeled `httpd_sys_content_t`.

You can see these labels using the `-Z` flag:

```bash
# This file is in a web directory
$ ls -Z /var/www/html/index.html
-rw-r--r--. root root unconfined_u:object_r:httpd_sys_content_t:s0 /var/www/html/index.html

# This file is in your home directory
$ ls -Z /home/hashim/secret.txt
-rw-r--r--. hashim hashim unconfined_u:object_r:user_home_t:s0 /home/hashim/secret.txt
```

The web server process is forbidden from reading the `user_home_t` label, regardless of the `rw-r--r--` (DAC) permissions.


## 🧑‍💼 Role-Based Access Control (RBAC)

**Role-Based Access Control (RBAC)** is an alternative to the permission-based access control of filesystem objects.

Instead of assigning permissions directly to users, a system administrator assigns **roles** (which are based on business or functional criteria). These roles are then given access to specific filesystem objects.

This is a logical abstraction over MAC or DAC. To interact with an object, a subject must be a member of a specific group or role. This makes managing large numbers of users much simpler.

### Practical Example (The `sudoers` File)

The `/etc/sudoers` file is a perfect example of RBAC. Instead of giving 50 individual users permission to restart the database, you create a *role* (a group) and give that *role* the permission.

**1. Define the Role in `/etc/sudoers` (using `sudo visudo`):**

```
# This line creates a "role" (an alias) called DB_ADMINS
User_Alias DB_ADMINS = user1, user2, user3

# This line gives the DB_ADMINS role specific permissions
DB_ADMINS ALL = /usr/sbin/service postgresql restart, /usr/bin/psql
```

**2. Manage Users by Role:**
Now, when a new employee (`user4`) joins the database team, you don't need to touch the `sudoers` file. You simply add them to the `DB_ADMINS` role. This is a logical abstraction.


## 🔒 Multi-Level Security (MLS)

**Multi-Level Security (MLS)** is a specific and very strict **MAC scheme**. In this model, subjects (processes) and objects (files, sockets, etc.) are given security levels (e.g., "Top Secret," "Secret," "Confidential").

This model enforces strict rules about information flow, such as:

  * **"No Read Up":** A "Confidential" process can *never* read a "Top Secret" file.
  * **"No Write Down":** A "Top Secret" process can *never* write its data into a "Confidential" file (this would leak the secret).


## 🗃️ Multi-Category Security (MCS)

**Multi-Category Security (MCS)** is an improved version of SELinux that reuses much of the MLS framework.

MCS allows users to label files with **categories** in addition to levels. This is used heavily in virtualization and containers (like Docker) to isolate them from each other.

> **Simple Analogy:**
>
>   * **MLS Level:** Your security clearance is "Top Secret".
>   * **MCS Category:** Your project is "Project Alpha".
>
> You can read "Top Secret" files for "Project Alpha". You *cannot* read "Top D" files for "Project Bravo", even though you have the correct "Top Secret" *level*. You are not in the "Project Bravo" *category*.


> **A Note on Prior Concepts**
>
> Wrapping up our brief discussion on ACMs, we should note that the internals of **DAC** and **ACLs** were covered in *Section 4, Managing Users and Groups*, particularly in the *Managing permissions* section.

---