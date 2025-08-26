# 👨‍💻 **Introducing Processes** in Linux

A **process** represents a **running instance of a program**.

🔍 **Definition:**

> A process is a program in action — the actual execution of instructions from a compiled executable file.

🧠 In simple terms:

* A **program** is a passive collection of code and data.
* A **process** is that program actively running on the system.

📌 Every time you run a **command**, **script**, or **application**, a new **process** is created.

---

## 🛠️ How Are Processes Created?

In **Linux**, **every command you run** in the terminal starts a new process.

✅ These can be:

* 🧑‍💻 User-initiated commands (in terminal)
* 📜 Scripts (like Bash or Python)
* ⚙️ Executable programs (run manually or automatically)

The **method of creation** and **how it interacts** with the system or user determines the process **type**.

---

# 🔎 Understanding Process Types in Linux

At a high level, there are **two major categories**:

| Type              | Description                                 | Interaction |
| ----------------- | ------------------------------------------- | ----------- |
| 🟢 **Foreground** | Interactive processes (needs user input)    | Yes         |
| ⚫ **Background**  | Non-interactive or automated (run silently) | No          |

---

## 🧑‍💼 Foreground (Interactive) Processes

🔹 These processes **require user interaction** during their execution.

📌 Examples:

* Running `vim` in the terminal
* Executing `python` and typing commands live

---

## 🤖 Background (Non-Interactive) Processes

🔹 These processes run **without any user interaction**.

🔁 They are:

* **Automatically started** at boot time
* **Scheduled** to run at specific times (via `cron` or `at`)

---

# 🧩 Extended Classifications

While most process types can be grouped as **foreground** or **background**, there are some special kinds:

## 🧾 Batch Processes

* These are **automated tasks** scheduled by the system.
* Not manually initiated by users.
* Examples: Cron jobs, system backups, scheduled log rotations.

> 📌 **Batch = Background + Scheduled**

---

## 🧙‍♂️ Daemons

* Special background processes.
* Started **automatically at system boot**.
* Run **indefinitely** in the background (until system shutdown).

📌 Common examples:

* `sshd` (SSH server)
* `httpd` (Apache web server)
* `cron` (scheduler daemon)

---

## 👨‍👦 Parent and Child Processes

In Linux, processes can **create other processes**.

* The **creator** is called the **Parent Process**.
* The newly created one is the **Child Process**.

📌 Example:

```bash
bash -> python script.py
```

Here:

* `bash` is the parent
* `python` process is the child

---

# 🟢 **Foreground Processes** in Linux

A **Foreground Process** (also known as an **Interactive Process**) is a process that:

* 🧑‍💻 Starts **through a Terminal session**
* 📥 Accepts **user input**
* 📤 Sends **output to the screen** (stdout/stderr)
* 🔄 **Blocks** the terminal until the task finishes or is interrupted

---

## 🧠 Key Characteristics

| Feature               | Description                                           |
| --------------------- | ----------------------------------------------------- |
| 🎬 Started by         | User via Terminal                                     |
| 🧾 Output goes to     | Terminal (stdout or stderr)                           |
| 🧍 Needs user input   | Often yes                                             |
| 🔗 Linked to terminal | Tied to the current Terminal session (parent process) |
| 🔚 If terminal exits  | Process ends immediately (via `SIGHUP` signal)        |
| ✋ Interrupting        | `Ctrl + C` sends `SIGINT` to stop the process         |

---

## 🔍 Simple Foreground Example

```bash
man ps
```

📘 This opens the manual for the `ps` command (used to view current processes).

### ⚠️ Behavior:

* Terminal is captured by the `man` interface.
* You can’t type any other command until you **exit** the manual by pressing `q`.

---

## 🔁 Foreground Example – Infinite Loop

Here’s a **long-running task** that loops infinitely and prints "Wait..." every 5 seconds:

```bash
while true; do echo "Wait..."; sleep 5; done
```

### 📟 Sample Terminal Output:

```bash
root@abb64eb599dc:/# while true; do echo "Wait..."; sleep 5; done
Wait...
Wait...
Wait...
Wait...
^C
root@abb64eb599dc:/#
```

### ⚙️ Explanation of the Command:

| Part             | Meaning                                                                 |
| ---------------- | ----------------------------------------------------------------------- |
| `while true`     | Start an infinite loop                                                  |
| `do ... done`    | Commands inside the loop                                                |
| `echo "Wait..."` | Print the message “Wait...”                                             |
| `sleep 5`        | Pause execution for 5 seconds before repeating                          |
| `^C` (Ctrl + C)  | Sends `SIGINT` signal to stop the process and return to Terminal prompt |

---

## 📢 Important Signal: `SIGINT`

When you press **Ctrl + C**:

* 🧠 A **SIGINT (Signal Interrupt)** is sent to the **foreground process**.
* ✂️ The process is **interrupted** and **stopped immediately**.
* 🖥️ The Terminal becomes **interactive again**.

For deeper info, refer to the **Signals section** in the *Inter-process Communication* topic.

---

# ⚫ **Background Processes** in Linux

A **Background Process** (also known as a **non-interactive** or **automated process**) is a process that:

* 🏃 Runs **independently** of user input
* 💻 Is usually launched from the **Terminal** but **does not block it**
* 📂 Often writes output to **log files** instead of the terminal
* 🕰️ Is typically **long-running** and doesn’t require supervision

---

## 🧠 Key Characteristics

| Feature                | Description                                                            |
| ---------------------- | ---------------------------------------------------------------------- |
| 🧑‍💻 Started from     | Terminal, script, or auto-triggered                                    |
| 👤 Needs user input?   | ❌ No                                                                   |
| 🕐 Duration            | Often long-lived                                                       |
| 🖥️ Terminal blocked?  | ❌ No — you can keep using the terminal while it runs                   |
| 🗂️ Output Location    | Often written to files (e.g. log files) instead of displaying onscreen |
| 🔗 Linked to terminal? | Yes, but doesn’t stop terminal interaction                             |

---

## ✅ How to Start a Background Process

Append an `&` at the end of any command:

```bash
while true; do echo "Wait..."; sleep 10; done &
```

### 🧾 Breakdown of the Command:

| Part             | Meaning                                  |
| ---------------- | ---------------------------------------- |
| `while true`     | Infinite loop                            |
| `do ... done`    | Commands to repeat                       |
| `echo "Wait..."` | Print message                            |
| `sleep 10`       | Wait 10 seconds between messages         |
| `&`              | Run in background (don’t block terminal) |

---

## 👨‍💻 Example: Interactive Use While Background Process is Running

```bash
root@abb64eb599dc:/# while true; do echo "Wait..."; sleep 10; done &
[1] 2866   # <- Process ID (PID) shown after launch
root@abb64eb599dc:/# echo "Interactive prompt..."
Interactive prompt...
```

> ⚠️ Even though the process is running, you can **continue typing** and running other commands.

---

## 🔪 Terminating a Background Process

To stop a background process, use the `kill` command along with its PID:

```bash
kill -9 2866
```

### 💡 Explanation:

| Command | Description                                      |
| ------- | ------------------------------------------------ |
| `kill`  | Sends a termination signal to a process          |
| `-9`    | Stands for `SIGKILL`, a **forceful** termination |
| `2866`  | The PID (process ID) of the background process   |

✅ Once the process is killed, you’ll see:

```bash
[1]+  Killed                  while true; do echo "Wait..."; sleep 10; done
```

---

## 🧬 Background vs. Foreground Processes

| Feature                  | Foreground           | Background                    |
| ------------------------ | -------------------- | ----------------------------- |
| Terminal Blocked?        | ✅ Yes                | ❌ No                          |
| Needs User Input?        | ✅ Often              | ❌ Usually not                 |
| Ends if Terminal Exits?  | ✅ Yes (via `SIGHUP`) | ✅ Usually (unless daemonized) |
| Can Output to Log Files? | ❌ Not usually        | ✅ Common practice             |

---

## 🔁 Automated Background Tasks

There are two special types:

### 📅 **Batch Processes**

* Scheduled via tools like `cron` or `at`
* Not triggered manually by the user

### 🧙‍♂️ **Daemons**

* Start automatically during **system boot**
* Stop automatically at **shutdown**
* Examples: `sshd`, `cron`, `nginx`, etc.

> 💡 These are background processes too, but they **don't need terminal** interaction at all.

There’s also a select category of background processes that are automatically started during system
boot and terminated at shutdown without user supervision. These background processes are also
known as daemons.

---

# 🧿 **Introduction to Daemons** in Linux

## 🌀 What is a Daemon?

A **Daemon** is a special type of **background process** in Linux that:

* Starts automatically **during system boot**
* **Runs indefinitely** or until it's stopped (usually at shutdown)
* **Doesn’t interact directly** with the user or a terminal
* Is typically associated with a **system user account** (like `root`) and runs with **elevated privileges**

> ⚙️ Daemons are essential for handling various system services and tasks silently in the background.

---

## 📡 What Do Daemons Do?

Daemons usually:

* Serve **client requests** (e.g., HTTP, FTP)
* Communicate with other **foreground or background processes**

---

## 🔥 Common Examples of Daemons in Linux

| Daemon    | Description                                                   |
| --------- | ------------------------------------------------------------- |
| `systemd` | 🔁 The **parent of all processes** (replaces older `init`)    |
| `crond`   | ⏰ A **scheduler** that runs automated tasks in the background |
| `ftpd`    | 🌐 An **FTP server** that handles file transfer requests      |
| `httpd`   | 🌍 A **web server** (like Apache) for serving HTTP requests   |
| `sshd`    | 🔒 A **Secure Shell** server that handles remote SSH logins   |

📝 **Naming Convention:** Most daemons in Linux end with **`d`**, which stands for "daemon".

---

## 📂 Where Are Daemon Scripts Stored?

Depending on your **Linux distribution**, the location of daemon scripts varies:

| Distro | Daemon Script Directory |
| ------ | ----------------------- |
| Ubuntu | `/etc/init.d/`          |
| Fedora | `/lib/systemd/`         |

These scripts are used by the **init system** to start, stop, or manage services.

---

## ⚙️ How Are Daemons Controlled?

System daemons are typically controlled through **shell scripts**. These scripts:

* Are **executed during system boot** (automatically)
* Can also be **manually invoked** by administrators using service control commands

### 🧑‍💻 Example: Starting/Stopping Daemons

Commands like `systemctl start sshd` or `service sshd stop` allow privileged users to:

* Manage daemon lifecycle
* Perform actions in the **background** while keeping the **terminal free**

---

# 🧬 Understanding the init Process

## 🪜 What is init?

`init` (short for initialization) is:

* A **system process** and **service manager**
* The **first process** that starts when Linux boots
* The **root (parent)** of all other processes

🎯 All Linux processes are **direct or indirect children** of `init`.

---

## 🧠 Evolution of init Systems

Over time, various `init` implementations have evolved:

* **SysVinit**
* **Upstart**
* **OpenRC**
* **systemd** (most widely used today)
* **runit**

Each has pros and cons, but we treat all of them under the generic term "**init**" here.

---

## 🌳 Viewing the Process Tree with `pstree`

The `pstree` command gives a **tree-like view** of all processes, starting from `init` (often `systemd`):

### 🔍 Example Output:

```bash
root@5ca0168c9be1:/# pstree
systemd-+-cron
        |-dbus-daemon
        |-rsyslogd---3*[{rsyslogd}]
        |-2*[sleep]
        |-sshd
        |-systemd-journal
        |-systemd-logind
        |-systemd-network
        |-systemd-resolve
        |-systemd-udevd
        `-udisksd---5*[{udisksd}]
```

### 🧾 Explanation:

* `systemd` is at the **top** of the tree (parent process)
* `cron`, `sshd`, `rsyslogd`, etc., are **child processes**
* Threads (e.g., `[{rsyslogd}]`) are shown with asterisk (`*`) and count

This tree helps visualize the **hierarchical relationship** between processes and their **parent-child** structure.

> 💡 **Pro Tip:** Use `ps aux | grep daemon-name` or `systemctl status daemon-name` to check the status of any daemon.

---

# 👨‍👦 **Parent and Child Processes** in Linux

## 🧬 What is a Parent Process?

In Linux, a **Parent Process** is one that creates other processes using system calls like `fork()` or similar. The newly created processes are known as **Child Processes**.

### 🔁 Relationship:

* A **Parent Process** ➝ spawns ➝ **Child Processes**
* Each child is **linked** to its parent.
* If the parent process stops or exits, the child is usually affected—unless special instructions are given.

---

## 👶 What is a Child Process?

A **Child Process** is:

* Spawned by a **parent process**
* Usually **inherits** environment and permissions from the parent
* Normally **terminates** when the parent exits

However...

> 🛡️ If the child process is told to **ignore the SIGHUP signal** (using tools like `nohup`), it can continue to run even after the parent dies.

---

## 🧠 Key Concept: `SIGHUP` Signal

* **SIGHUP** (Signal Hang Up) is sent to child processes **when a parent process exits**
* If **not handled**, the child process will **terminate**
* You can **suppress** this behavior using the `nohup` command

```bash
nohup myscript.sh &
```

> 📌 This keeps the child process running **even after logout or parent termination**

---

## 🌲 Linux Process Tree Hierarchy

* **All Linux processes**, except the very first (`init` or `systemd`), are **child processes of some parent**.
* This creates a **process hierarchy tree** in the system.

> 🧮 You can view it with:

```bash
pstree
```

---

## 🔥 Important Behaviors

| Behavior                  | Description                                                                 |
| ------------------------- | --------------------------------------------------------------------------- |
| 🛑 Killing child process  | Will **not stop** the parent                                                |
| 🛑 Killing parent process | Will **usually stop** child unless `nohup` or similar is used               |
| ✅ Best Practice           | Let parent process **gracefully exit** after child completes                |
| 🧩 Signals                | Parent sends signals to child (like `SIGTERM`, `SIGHUP`) to manage behavior |

---


🧠 **Pro Tip**: Use `ps -ef` or `pstree -p` to inspect running processes and parent-child relationships.

Look:
```bash
root@5ca0168c9be1:/# ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 00:12 ?        00:00:06 /sbin/init
root          25       1  0 00:12 ?        00:00:00 /usr/lib/systemd/systemd-journald
root          66       1  0 00:12 ?        00:00:00 /usr/sbin/cron -f -P
root          70       1  0 00:12 ?        00:00:00 /usr/lib/systemd/systemd-logind
syslog        74       1  0 00:12 ?        00:00:00 /usr/sbin/rsyslogd -n -iNONE
root          77       1  0 00:12 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 
message+      78       1  0 00:12 ?        00:00:01 @dbus-daemon --system --address=syste
root         105       0  0 00:12 pts/0    00:00:00 bash
systemd+    2601       1  0 00:24 ?        00:00:00 /usr/lib/systemd/systemd-networkd
systemd+    2627       1  0 00:24 ?        00:00:00 /usr/lib/systemd/systemd-resolved
root        3331       1  0 00:29 ?        00:00:00 /usr/libexec/udisks2/udisksd
root        3396       1  0 00:30 ?        00:00:00 /usr/lib/systemd/systemd-udevd
root        3447       0  0 00:30 pts/1    00:00:00 bash
root        3574       0  0 00:44 pts/2    00:00:00 bash
root        3600    3447  0 00:53 pts/1    00:00:00 bash -c set -o huponexit; sleep 1000 
root        3601    3600  0 00:53 pts/1    00:00:00 sleep 1000
root        3602    3600  0 00:53 pts/1    00:00:00 sleep 9999
root        3609    3574  0 01:00 pts/2    00:00:00 ps -ef
```

---
# 🔄 **Parent & Child Process Practical Example**

## 🧪 Case 1 — Parent killed → Child also dies

### ▶️ Step 1: Run a parent with a child in the foreground

```bash
bash -c "sleep 200" &
```

Output:

```
[1] 3615
```

* `3615` = PID of the parent (`bash -c`)
* It creates a child `sleep 200`

---

### ▶️ Step 2: Check processes

```bash
ps -o pid,ppid,cmd | grep '[s]leep'
```

Output:

```
3615  3574  sleep 200
```

Here:

* `PPID=3574` → Parent shell that spawned it
* `CMD=sleep 200`

---

### ▶️ Step 3: Kill the parent

```bash
kill -TERM 3615
```

---

### ▶️ Step 4: Verify

```bash
ps -o pid,ppid,cmd | grep '[s]leep'
```

Output:

```
[1]+  Terminated              bash -c "sleep 200"
```

✅ Proof: Parent is terminated → Child process (`sleep 200`) is also gone.

---

## 🧪 Case 2 — Child survives even if parent dies

### ▶️ Step 1: Run parent with `nohup` child

```bash
bash -c "nohup sleep 1000 >/tmp/nohup.out 2>&1 & sleep 9999" &
```

* Parent → `bash -c`
* Child1 → `sleep 1000` (background)
* Child2 → `sleep 9999`

---

### ▶️ Step 2: Check processes

```bash
ps -o pid,ppid,cmd | grep '[s]leep'
```

Sample Output:

```
3601  3600  sleep 1000
3602  3600  sleep 9999
```

---

### ▶️ Step 3: Kill the parent

```bash
kill -TERM 3600
```

---

### ▶️ Step 4: Verify

```bash
ps -o pid,ppid,cmd | grep '[s]leep'
```

Sample Output:

```
3601     1 sleep 1000
```

👉 Notice:

* Parent (`bash -c`) is gone
* Child (`sleep 1000`) survived
* **PPID = 1** → It was adopted by `systemd`

✅ Proof: Child continues even after parent termination.


## 🗂️ What is Batch Processing?

Sometimes, Linux processes are run:

* **Without user interaction**
* On a **predetermined schedule**
* For long-running or automated tasks

🎯 This is called **Batch Processing**

> Example: Running a backup script every night via `cron`
---

# 🧾 **Batch Processes**

Batch processing allows us to run **background jobs** automatically at a **specific time**, often on a **recurring schedule**. These are commonly used to automate repetitive or resource-heavy tasks during off-peak hours.

---

## 📌 What is a Batch Process?

> A **batch process** is a script or command that runs **automatically** without user interaction. It is typically launched by a **job scheduler** like `cron` or `at`.

These jobs can include:

* 📥 Backing up databases
* 📦 Compressing or uploading files
* 📊 Generating reports
* 🔄 Auto-updating system packages

---

## ⚙️ Tools for Scheduling Batch Jobs

### 🔁 `cron` – Recurring Jobs

* Designed for **recurring tasks** (e.g., every 2 minutes, every Monday).
* Uses a file called `crontab` to schedule jobs.

### ☑️ `at` – One-Time Jobs

* Used for **single-use**, scheduled tasks (e.g., "run this once at 4 PM").
* Not suitable for repeat schedules.

---

## 🔧 Step-by-Step: Install and Use `cron`

### ✅ Step 1: Install `cron` (If Not Already Installed)

```bash
sudo apt update
sudo apt install cron nano
```

### 🔁 Step 2: Start and Enable cron Service

```bash
sudo systemctl start cron
sudo systemctl enable cron
```

✅ This ensures `cron` starts at boot and runs in the background.

---

## ✍️ Step 3: Make nano as a default editor and Create the Batch Script

Let’s create a simple script at `/root/log_time.sh` that logs the current date and time every 2 minutes.

```bash
nano ~/.bashrc
export EDITOR=nano
source ~/.bashrc
nano /root/log_time.sh
```

Paste the following content into the file:

```bash
#!/bin/bash
echo "Job ran at: $(date)" >> /root/cron_job_log.txt
```

Make the script executable:

```bash
chmod +x /root/log_time.sh
```

---

## 🛠️ Step 4: Schedule the Job in Crontab

Edit the root user's crontab:

```bash
crontab -e
```

Then add the following line at the bottom:

```cron
*/2 * * * * /root/log_time.sh
```

📌 **This tells cron to run `/root/log_time.sh` every 2 minutes.**

---
Absolutely! Let's break down this line from the crontab:

```cron
*/2 * * * * /root/log_time.sh
```

This line tells `cron` **when** and **what** to run.

---

## 🧠 Crontab Format Breakdown

A typical crontab line has **5 time fields** followed by the **command/script**:

```cron
┌───────────── Minute (0 - 59)
│ ┌───────────── Hour (0 - 23)
│ │ ┌───────────── Day of month (1 - 31)
│ │ │ ┌───────────── Month (1 - 12)
│ │ │ │ ┌───────────── Day of week (0 - 7) (Sunday is 0 or 7)
│ │ │ │ │
│ │ │ │ │
* * * * *  command to run
```

---

## 📌 What Does `*/2 * * * *` Mean?

Let's break down **`*/2 * * * *`**:

| Field       | Value | Meaning               |
| ----------- | ----- | --------------------- |
| Minute      | `*/2` | Every **2 minutes**   |
| Hour        | `*`   | Every hour            |
| Day (Month) | `*`   | Every day             |
| Month       | `*`   | Every month           |
| Day (Week)  | `*`   | Every day of the week |

## ✅ Examples of Time Schedules

Here are some other examples of crontab time fields:

| Cron Expression   | Meaning                                   |
| ----------------- | ----------------------------------------- |
| `0 * * * *`       | Every hour at minute 0 (e.g., 1:00, 2:00) |
| `30 9 * * 1-5`    | 9:30 AM on weekdays (Mon–Fri)             |
| `0 0 * * 1`       | Every Monday at midnight                  |
| `*/15 8-17 * * *` | Every 15 minutes from 8 AM to 5 PM        |

---

## ✅ Step 5: Verify That cron Is Working

Wait a few minutes and check the log file:

```bash
cat /root/cron_job_log.txt
```

You should see something like:

```
Job ran at: Tue Aug 26 01:02:00 PKT 2025
Job ran at: Tue Aug 26 01:04:00 PKT 2025
```

🎉 Your batch job is successfully running every 2 minutes!

---

# 🧠 **Orphan, Zombie & Anatomy of a Process**

## ⚰️ Orphan and Zombie Processes

In Linux, understanding how processes behave — especially when they become **orphan** or **zombie** — is crucial for efficient system administration.

### 👶 What is an **Orphan Process**?

An **orphan process** occurs when:

* A child process is **still running**, but its **parent process terminates** before it finishes.
* In this case, the Linux kernel reassigns the orphan process to the **`init` process (PID 1)**, which becomes its new parent.
* This is a **normal behavior** and **handled gracefully** by Linux.

> 📌 **Remember:** Orphan processes are still **alive and running**.

---

### 🧟 What is a **Zombie Process**?

A **zombie process** (also known as a **defunct process**) occurs when:

* A child process **has completed execution** (terminated) **but still has an entry** in the **process table**.
* It stays there **until the parent process reads its exit status** using `wait()` system call.
* These entries can be seen using the `ps` command but the process **doesn't exist anymore**.

> 📌 **Remember:** Zombie processes are **terminated**, but **still linger** in memory tables.

---

### 🔍 Difference Between Orphan and Zombie

| Attribute       | Orphan Process 🍼  | Zombie Process 🧟                |
| --------------- | ------------------ | -------------------------------- |
| Alive?          | ✅ Yes              | ❌ No                             |
| Parent Exists?  | ❌ No (init adopts) | ✅ Yes (didn’t clean up)          |
| Cleanup Needed? | ❌ No (handled)     | ✅ Yes (manually if persistent)   |
| System Impact   | Minimal            | Can be dangerous if many pile up |

---

## 🧬 Anatomy of a Process in Linux

To inspect and analyze processes, Linux offers powerful tools like `ps` and `top`.

---

### 📌 `ps` Command Basics

```bash
ps [OPTIONS]
```

* The `ps` command shows a **snapshot of current system processes**.
* Most commonly used without any arguments to show processes in the current terminal.

```bash
ps
```

📸 **Sample Output:**

```bash
root@5ca0168c9be1:/# ps
  PID TTY          TIME CMD
  240 pts/0    00:00:00 bash
  262 pts/0    00:00:00 ps
```

---

### 📊 Breaking Down `ps` Output

| Header   | Meaning                                                                                                            |
| -------- | ------------------------------------------------------------------------------------------------------------------ |
| **PID**  | **Process ID** – A unique number assigned by the kernel when the process is created. Example: `240`                |
| **TTY**  | **Teletype Terminal** – Shows which terminal is controlling the process. Example: `pts/0` means a pseudo-terminal. |
| **TIME** | **CPU Time** – Total CPU time the process has used. Shows as `00:00:00` if commands run too quickly.               |
| **CMD**  | **Command Name** – The command that started the process (like `bash`, `ps`).                                       |

---

## 🔍 TTY: Understanding the Terminal Control

* **TTY** = Teletype terminal → represents where a process is interacting with.
* **pts/0** → Pseudo Terminal Session 0 (usually SSH or console session).
* More sessions (e.g., new SSH connections) will appear as `pts/1`, `pts/2`, etc.

---

## 🧠 CPU TIME Explanation

Even if you run multiple commands in your terminal session, the **`bash` process** might show **`00:00:00`** under **TIME** because:

* The **parent shell (bash)** doesn’t consume much CPU itself.
* **Each command** you run is a **separate child process**, and their CPU time is not added to bash’s TIME.

---

## 💡 Summary

| Concept        | Key Idea                                                |
| -------------- | ------------------------------------------------------- |
| Orphan Process | Child still running, parent is gone. Adopted by `init`. |
| Zombie Process | Child exited, but parent didn’t clean up.               |
| `ps` Command   | Shows active processes and useful diagnostics.          |
| TTY            | Terminal interface connected to the process.            |
| CPU TIME       | Only accumulates when process actively uses CPU.        |

---


