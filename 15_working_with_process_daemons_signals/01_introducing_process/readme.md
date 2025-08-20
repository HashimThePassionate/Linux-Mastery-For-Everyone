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

