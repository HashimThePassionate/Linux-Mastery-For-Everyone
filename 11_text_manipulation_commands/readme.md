#  **Commands for Text Manipulation** 🛠️

Linux offers a rich set of **text-manipulation** utilities that let you process, filter, and transform text directly from the command line. These tools are indispensable for system administration, scripting, and data processing, enabling you to work efficiently without leaving your terminal.


##  **`grep` — Text Search Tool** 🔍

**Definition:**
`grep` (Global Regular Expression Print) searches files for lines matching a specified pattern. It supports literal strings, basic and extended regular expressions, and an array of options to control output.


# Examples

Below is a step-by-step demonstration of each tool, complete with **hands-on examples** and **real-world analogies**. ✨

## 🗂️ Preparing Sample Files

```bash
# Start your Ubuntu container
docker start -ai my-ubuntu

# Create a demo directory and files
mkdir ~/text_tools_demo && cd ~/text_tools_demo

# users.txt
cat <<EOF > users.txt
alice:x:1000:1000:Alice:/home/alice:/bin/bash
bob:x:1001:1001:Bob:/home/bob:/bin/zsh
charlie:x:1002:1002:Charlie:/home/charlie:/bin/bash
EOF

# services.log
cat <<EOF > services.log
[INFO] 2025-07-08 09:00 sshd started
[WARN] 2025-07-08 09:05 high CPU usage
[ERROR] 2025-07-08 09:10 failed login for bob
[INFO] 2025-07-08 09:15 cron job executed
EOF

# data.csv
cat <<EOF > data.csv
name,age,city
Alice,30,Karachi
Bob,25,Lahore
Charlie,35,Islamabad
EOF
```

## 🔍 1. `grep` — Simple but Powerful Search

**Analogy:** Like using **Ctrl + F** across multiple files at once! 📄🔎

### Key Options

| Flag | Description                                |
| ---- | ------------------------------------------ |
| `-v` | Invert match (show lines **not** matching) |
| `-l` | List only filenames with a match           |
| `-L` | List only filenames **without** a match    |
| `-c` | Count matching lines                       |
| `-n` | Show line numbers                          |
| `-i` | Case-insensitive search                    |
| `-r` | Recursive search in directories            |
| `-E` | Use extended regular expressions           |
| `-F` | Fixed-string search (literal, no regex)    |

### Examples

1. **Find users with Bash shell**

   ```bash
   cat users.txt
   grep '/bin/bash' users.txt
   ```

`Output`
```bash
alice:x:1000:1000:Alice:/home/alice:/bin/bash
bob:x:1001:1001:Bob:/home/bob:/bin/zsh

root@7e56c80bcd77:~/text_tools_demo# grep '/bin/bash' users.txt
alice:x:1000:1000:Alice:/home/alice:/bin/bash
charlie:x:1002:1002:Charlie:/home/charlie:/bin/bash
```


2. **Count error lines in the log**

   ```bash
   cat services.log 
   grep -c 'ERROR' services.log
   ```

`Output`
```bash
[INFO] 2025-07-08 09:00 sshd started
[WARN] 2025-07-08 09:05 high CPU usage       
[ERROR] 2025-07-08 09:10 failed login for bob
[INFO] 2025-07-08 09:15 cron job executed 

1
```

3. **Show non-INFO lines**

   ```bash
   grep -v '\[INFO\]' services.log
   ```

`Output`
```bash
[WARN] 2025-07-08 09:05 high CPU usage
[ERROR] 2025-07-08 09:10 failed login for bob
```

4. **Recursive, case-insensitive search for “bob”**

   ```bash
   grep -Rin 'bob' *.txt
   ```

`Output`
```bash
2:bob:x:1001:1001:Bob:/home/bob:/bin/zsh
```

5. **List files mentioning “cron”**

   ```bash
   grep -l 'cron' services.log
   ```

`Output`
```bash
services.log
```

---

##  **The `tee` Command** 📬

## 🔍 Definition

The `tee` command reads data from **standard input** (stdin) and writes it **simultaneously** to **standard output** (stdout) and to one or more specified files. By default, `tee` **overwrites** the target file(s); with the `-a` option, it **appends** instead of replacing.

**Key Points:**

* Splits a data stream like a plumbing **T-junction** 🚰
* Enables you to **view** output on-screen and **save** it to file(s) in one step ✏️
* Supports writing to **multiple files** at once 📂
* Offers an **append** mode (`-a`) to preserve existing file contents ➕

## 🛠️ Prerequisites: Create Sample Files

Let’s set up a demo environment:

```bash
# 1. Start Ubuntu container
docker start -ai my-ubuntu

# 2. Create a demo directory and enter it
mkdir -p ~/tee_demo && cd ~/tee_demo

# 3. Create a simple text file
cat <<EOF > message.txt
Hello, world!
This is line two.
EOF

# 4. Create a log file
cat <<EOF > system.log
[INFO] 2025-07-08 System initialized
[WARN] Disk usage at 90%
[ERROR] Service X failed to start
EOF
```

## 🎯 Hands-On Examples

### 1. Capture and Display Line Count

Count lines in `message.txt`, save result to `linecount.txt`, and view it:

```bash
wc -l message.txt | tee linecount.txt
```

* **On-screen output**

  ```
  2 message.txt
  ```
* **Verify file**

  ```bash
  cat linecount.txt
  # → 2 message.txt
  ```

---

### 2. Append New Content to a File

Append a new line to `message.txt` while displaying it:

```bash
echo "This is line three." | tee -a message.txt
```

* **Printed**

  ```
  This is line three.
  ```
* **Verify file contents**

  ```bash
  cat message.txt
  # → Hello, world!
  #   This is line two.
  #   This is line three.
  ```

### 3. Log Warnings from a System File

Filter warnings from `system.log`, save to `warnings.log`, and display:

```bash
grep '\[WARN\]' system.log | tee warnings.log
```

* **Output & file**

  ```
  [WARN] Disk usage at 90%
  ```

### 4. Broadcast Output to Multiple Files

Generate disk usage report and write to three files at once:

```bash
df -h | tee disk_a.txt disk_b.txt disk_c.txt
```

* **Screen**: Shows human-readable disk usage
* **Files**: `disk_a.txt`, `disk_b.txt`, `disk_c.txt` each contain the same report


### 5. Using `sudo tee` for Protected Files

Append a configuration line to a root-owned file:

```bash
echo "PermitRootLogin no" | sudo tee -a /etc/ssh/sshd_config
```

* **Why `sudo tee`?**

  * `sudo echo … > file` fails due to redirection permissions
  * `sudo tee -a file` writes successfully under elevated privileges

---


