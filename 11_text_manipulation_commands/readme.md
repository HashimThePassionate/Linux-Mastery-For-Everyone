#  **Commands for Text Manipulation** 🛠️

Linux offers a rich set of **text-manipulation** utilities that let you process, filter, and transform text directly from the command line. These tools are indispensable for system administration, scripting, and data processing, enabling you to work efficiently without leaving your terminal.


## 🔍 `grep` — Text Search Tool

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
