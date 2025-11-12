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

Here is a comprehensive, detailed explanation of the provided text.

# 🛡️ Introducing AppArmor

**AppArmor** (Application Armor) is a Linux security module based on the **Mandatory Access Control (MAC)** model that confines applications to a limited set of resources.

AppArmor uses an access control mechanism based on **security profiles** that have been loaded into the Linux kernel. Each profile contains a collection of rules for accessing various system resources. AppArmor can be configured to either **enforce** access control (block violations) or just **complain** about access control violations (log them but allow them).

AppArmor proactively protects applications and operating system resources from internal and external threats, including **zero-day attacks**, by preventing both known and unknown vulnerabilities from being exploited.

AppArmor has been built into the mainline Linux kernel since version 2.6.36 and is currently shipped with **Ubuntu, Debian, openSUSE**, and similar distributions.

In the following sections, we’ll use an **Ubuntu Server 22.04 LTS** environment to showcase a few practical examples with AppArmor. Most of the related command-line utilities will work the same on any platform with AppArmor installed.


## 🚀 Working with AppArmor

AppArmor command-line utilities usually require superuser privileges. The following command checks the current status of AppArmor:

```bash
sudo aa-status
```

Here’s an excerpt from the command’s output:

```bash
hashim@Hashim:~/Repo$ sudo aa-status
apparmor module is loaded.
215 profiles are loaded.
132 profiles are in enforce mode.
   /snap/snapd/25202/usr/lib/snapd/snap-confine
   /snap/snapd/25202/usr/lib/snapd/snap-confine//mount-namespace-capture-helper
...
```

The `aa-status` (or `apparmor_status`) command provides a full list of the currently loaded AppArmor profiles. We’ll examine AppArmor profiles next.

### 📜 Introducing AppArmor Profiles

With AppArmor, processes are confined (or restricted) by **profiles**. AppArmor profiles are loaded upon system start and run in either `enforce` mode or `complain` mode. Let’s explore these modes in some detail:

  * **`enforce` mode:** AppArmor prevents applications running in enforce mode from performing restricted actions (e.g., writing to a file they shouldn't). Access violations are signaled with log entries in `syslog`. Ubuntu, by default, loads the application profiles in enforce mode.
  * **`complain` mode:** Applications running in complain mode *can* take restricted actions, while AppArmor creates a log entry for the related violation. `complain` mode is ideal for testing AppArmor profiles. Potential errors or access violations can be caught and fixed before switching the profiles to `enforce` mode.

With these introductory notes in mind, let’s create a simple application with an AppArmor profile.


## 🛠️ Creating a Profile

In this section, we’ll create a simple application guarded by AppArmor. We hope this exercise will help you get a reasonable idea of the inner workings of AppArmor. Let’s name this application `appackt`.

We’ll make it a simple script that creates a file, writes to it, and then deletes the file. The goal is to have AppArmor prevent our app from accessing any other paths in the local system. To try to make some sense of this, think of it as trivial log recycling.

### 1\. The `appackt` Script

Here’s the `appackt` script, and please pardon the thrifty implementation:

```bash
hashim@Hashim:~/Repo$ nano appackt
hashim@Hashim:~/Repo$ cat appackt 
#!/bin/bash
# Assuming ./log directory exists!
if [[ ! -d "./log" ]]
then
    echo "No log dir!"
    exit 1
fi
LOG_FILE="./log/appackt"
echo "Creating ${LOG_FILE}..."
touch ${LOG_FILE}
echo "Writing to ${LOG_FILE}..."
date +"%b %d %T ${HOSTNAME}: Hello from Packt!" >> ${LOG_FILE}
echo "Reading from ${LOG_FILE}..."
cat ${LOG_FILE}
echo "Deleting ${LOG_FILE}..."
rm ${LOG_FILE}
```

**Code Explanation:**

  * `if [[ ! -d "./log" ]]`: Checks if the directory named `log` does **not** (`!`) exist.
  * `LOG_FILE="./log/appackt"`: Defines a variable for the path where the script will work.
  * `touch ${LOG_FILE}`: Creates the empty log file.
  * `date ... >> ${LOG_FILE}`: Appends the current date, time, hostname, and a message to the log file.
  * `cat ${LOG_FILE}`: Reads the log file and prints its content.
  * `rm ${LOG_FILE}`: Deletes the log file.

### 2\. Initial Setup and Run

We are assuming that the `log` directory already exists at the same location as the script. Let’s make the script executable and run it to confirm it works.

```bash
hashim@Hashim:~/Repo$ mkdir ./log
hashim@Hashim:~/Repo$ ls
appackt  log
hashim@Hashim:~/Repo$ chmod a+x appackt
hashim@Hashim:~/Repo$ ./appackt 
Creating ./log/appackt...
Writing to ./log/appackt...
Reading from ./log/appackt...
Nov 12 12:38:41 Hashim: Hello from Packt!
Deleting ./log/appackt...
```

The script runs successfully without any AppArmor confinement.

### 3\. Install AppArmor Utilities

Now, let’s work on guarding and enforcing our script with AppArmor. Before we start, we need to install the `apparmor-utils` package—the AppArmor toolset:

```bash
sudo apt install -y apparmor-utils
```

We’ll use a couple of tools to help create the profile:

  * **`aa-genprof`**: **Gen**erates a new AppArmor security **prof**ile by "learning" what an application does.
  * **`aa-logprof`**: Updates an existing AppArmor security **prof**ile by scanning logs for violations.

### 4\. Generate the Profile with `aa-genprof`

We use `aa-genprof` to monitor our application at runtime and have AppArmor learn about it. This is a **two-terminal process**.

**In Terminal 1:** Run `aa-genprof`.

```bash
sudo aa-genprof ./appackt
```

There is a first prompt waiting for us. `aa-genprof` is now monitoring system logs for any actions performed by `./appackt`.

**In Terminal 2:** Run the application.
While the prompt in Terminal 1 is waiting, we will switch to Terminal 2 and run the following command:

```bash
./appackt
```

This run will generate the "events" (like `touch`, `date`, `cat`, `rm`) that `aa-genprof` is waiting to see.

**Back in Terminal 1:**
Now, we must go back to Terminal 1 and answer the prompts sent by `aa-genprof`.

  * **Prompt 1: Waiting to scan**

      * **Explanation:** This prompt asks to scan the system log for AppArmor events in order to detect possible complaints (violations).
      * **Answer: `S` (S)can**

  * **Prompts 2 to 5: Execute permissions (Inherit)**

      * **Explanation:** The tool will find that `appackt` (a bash script) executed other commands: `/usr/bin/touch`, `/usr/bin/date`, `/usr/bin/cat`, and `/usr/bin/rm`. It asks for "execute" permissions for these.
      * **Answer: `I` (I)nherit**
      * **What `(I)nherit` means:** This is a common choice for scripts. It means the child process (like `touch`) will run under the *same profile* as the parent (`appackt`).

  * **Prompts 6 to 9: Read/write permissions (Allow)**

      * **Explanation:** The tool will see the script trying to read/write to files like `/dev/tty` (the terminal), `/home/hashim/Repo/log/appackt` (our log file), and `/etc/ld.so.cache` (a system library cache). It asks for `r` (read) and `w` (write) permissions.
      * **Answer: `A` (A)llow**
      * **What `(A)llow` means:** This explicitly adds a rule to the profile to permit this specific action (e.g., "allow write access to `/home/hashim/Repo/log/appackt`").

  * **Prompt 10: Save changes**

      * **Explanation:** The prompt asks to save or review changes.
      * **Answer: `S` (S)ave**

At this point, we have finished scanning with `aa-genprof`, and we can answer the last prompt with **`F` (F)inish**.

Our app (`appackt`) is now enforced by AppArmor in `enforce` mode (by default).

### 5\. Refine the Profile with `aa-logprof`

For the rest of the steps, we only need one terminal window. Let’s run the `aa-logprof` command to further tune our `appackt` security profile if needed:

```bash
sudo aa-logprof
```

We’ll get several prompts again, similar to the previous ones, asking for further permissions needed by our script or by other applications. The prompts alternate between **(I)nherit** and **(A)llow** answers, where appropriate. It’s always recommended to ponder upon the permissions asked and act accordingly.

We may have to run the `aa-logprof` command a couple of times because, with each iteration, new permissions will be discovered and addressed, depending on the child processes that are spawned by our script, and so on.

### 6\. Clean Up Orphaned Profiles (Optional)

`aa-logprof` may discover old, orphaned profile entries in the kernel's cache. They will all be named according to the path of our application (e.g., `/home/hashim/Repo/appackt//null-/usr/bin/touch`). We can clean up these entries with the following command:

```bash
hashim@Hashim:~/Repo$ sudo aa-remove-unknown
```

**Example Output:**

```
Warning: found usr.sbin.sssd in /etc/apparmor.d/force-complain, forcing complain mode
Removing 'snap.snapd-desktop-integration.snapd-desktop-integration'
Removing 'snap.snapd-desktop-integration.hook.configure'
...
Removing '/snap/snapd/25202/usr/lib/snapd/snap-confine'
Removing '/home/hashim/Repo/appackt//null-/usr/bin/touch'
Removing '/home/hashim/Repo/appackt//null-/usr/bin/rm'
Removing '/home/hashim/Repo/appackt//null-/usr/bin/date'
Removing '/home/hashim/Repo/appackt//null-/usr/bin/cat'
```

**Explanation:** This command removes *all* unknown or orphaned profiles, including many from `snap` packages. At the bottom, you can see it cleaning up the temporary "child" profiles that `aa-genprof` created during its learning process.

### 7\. Verify the Profile is Enforcing

We can now verify that our app is indeed guarded with AppArmor:

```bash
sudo aa-status
```

The relevant excerpt from the output is as follows:

```bash
hashim@Hashim:~/Repo$ sudo aa-status
apparmor module is loaded.
185 profiles are loaded.
102 profiles are in enforce mode.
   /home/hashim/Repo/appackt
   /usr/bin/man
...
```

Our application (`/home/hashim/Repo/appackt`) is shown, as expected, in `enforce` mode.

### 8\. Test the Profile (Validation)

Next, we need to validate that our app complies with the security policies enforced by AppArmor. Let’s edit the `appackt` script and change the `LOG_FILE` path from `log` to `logs`.

```bash
# Change line 6 from ./log/appackt to ./logs/appackt
hashim@Hashim:~/Repo$ nano appackt
```

We have changed the output directory from `log` to `logs`. Let’s create this new `logs` directory and run our app:

```bash
hashim@Hashim:~/Repo$ mkdir logs
hashim@Hashim:~/Repo$ ./appackt
```

**Example Output (Permission Denied):**

```
Creating ./log/appackt...
touch: cannot touch './log/appackt': Permission denied
Writing to ./log/appackt...
./appackt: line 12: ./log/appackt: Permission denied
Reading from ./log/appackt...
./appackt: line 14: /usr/bin/cat: Permission denied
Deleting ./log/appackt...
./appackt: line 16: /usr/bin/rm: Permission denied
```

**Explanation of Failure:**
The preceding output suggests that `appackt` is attempting to access a path *outside the permitted boundaries* (our profile only allows access to `./log/`, not `./logs/`), thus validating our profile is working.

*(Note: The `echo` messages in the output still say `"./log/appackt"` because we only changed the `LOG_FILE` variable in the script, not the `echo` statements. The "Permission denied" errors confirm the script *tried* to access the new, un-profiled path.)*


## 🚦 Managing Profile States

Let’s revert the preceding changes (change `./logs/` back to `./log/`) and have the `appackt` script act normally.

### Set to Enforce Mode

We can explicitly set a profile to `enforce` mode with the following command:

```bash
hashim@Hashim:~/Repo$ sudo aa-enforce /home/hashim/Repo/appackt 
```

**Example Output:**

```
skipping unparseable profile /etc/apparmor.d/fusermount3 (Can't parse mount rule mount fstype=@{fuse_types} options=(nosuid,nodev) options in (ro,rw,noatime,dirsync,nodiratime,noexec,sync) -> @{HOME}/**/,)
Setting /home/hashim/Repo/appackt to enforce mode.
```

### Set to Complain Mode

If we wanted to make further adjustments to our application and then test it, we would change the profile mode to `complain` and then re-run `aa-logprof` to learn the new rules.

```bash
hashim@Hashim:~/Repo$ sudo aa-complain /home/hashim/Repo/appackt 
```

**Example Output:**

```
skipping unparseable profile /etc/apparmor.d/fusermount3 (Can't parse mount rule mount fstype=@{fuse_types} options=(nosuid,nodev) options in (ro,rw,noatime,dirsync,nodiratime,noexec,sync) -> @{HOME}/**/,)
Setting /home/hashim/Repo/appackt to complain mode.
```

AppArmor profiles are plain text files stored in the `/etc/apparmor.d/` directory. Creating or modifying them usually involves manually editing the files or using the `aa-genprof` and `aa-logprof` tools.

Next, let’s look at how to disable or enable AppArmor application profiles.


## 🚫 Disabling and Enabling Profiles

Sometimes, we may want to disable a problematic application profile while working on a better version. Here’s how we do this.

### 1\. Locate the Profile File

First, we need to locate the application profile we want to disable. The related file is in the `/etc/apparmor.d/` directory and it’s **named according to its full path, with dots (`.`) instead of slashes (`/`)**.

In our case, the file for `/home/hashim/Repo/appackt` is `home.hashim.Repo.appackt`.

```bash
hashim@Hashim:~/Repo$ cd /etc/apparmor.d/
hashim@Hashim:/etc/apparmor.d$ ls | grep "hashim"
home.hashim.Repo.appackt
```

### 2\. Disable the Profile

To disable the profile, we **create a symbolic link** to it inside the `/etc/apparmor.d/disable/` directory, and then **unload it from the kernel** using `apparmor_parser -R`.

```bash
hashim@Hashim:/etc/apparmor.d$ sudo ln -s /etc/apparmor.d/home.hashim.Repo.appackt /etc/apparmor.d/disable/ 
hashim@Hashim:/etc/apparmor.d$ sudo apparmor_parser -R /etc/apparmor.d/home.hashim.Repo.appackt
```

  * `ln -s`: Creates the symbolic link. AppArmor knows to ignore profiles linked here.
  * `apparmor_parser -R`: **R**emoves (unloads) the profile from the kernel's memory.

If we run the `sudo aa-status` command, we won’t see our `appackt` profile anymore. In this situation, the `appackt` script is not enforced by any restrictions.

### 3\. Re-enable the Profile

To re-enable the related security profile, we reverse the process: **remove the symbolic link** and **reload the profile into the kernel** with `apparmor_parser -r`.

```bash
sudo rm /etc/apparmor.d/disable/home.hashim.Repo.appackt
sudo apparmor_parser -r /etc/apparmor.d/home.hashim.Repo.appackt
```

  * `rm`: Removes the symbolic link.
  * `apparmor_parser -r`: **R**eplaces (loads) the profile back into the kernel.

### Deleting AppArmor Security Profiles

Deleting AppArmor security profiles is functionally equivalent to disabling them. We can also choose to remove the related file (`/etc/apparmor.d/home.hashim.Repo.appackt`) from the filesystem altogether.

If we delete a profile without removing it from the kernel first (with `apparmor_parser -R`), we can use the `aa-remove-unknown` command to clean up orphaned entries.


## 🏁 Final Considerations

Working with **AppArmor** is relatively easier than **SELinux**, especially when it comes to generating security policies or switching back and forth between permissive (`complain`) and non-permissive (`enforce`) mode. SELinux can only toggle the permissive context for the *entire system*, while AppArmor does it at the *application level*.

On the other hand, there might be no choice between the two, as some major Linux distributions either support one or the other.

  * **AppArmor** is used on Debian, Ubuntu, and openSUSE.
  * **SELinux** runs on RHEL, Fedora, and SLE.
    Theoretically, you can always try to port the related kernel modules across distros, but that’s not a trivial task.

As a final note, we should reiterate that in the big picture of Linux security, SELinux and AppArmor are **ACMs** that act *locally* on a system, at the application level. When it comes to securing applications and computer systems from the *outside world*, **firewalls** come into play.

---