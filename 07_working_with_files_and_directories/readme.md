# **Working with Files and Directories** 📁

## Introduction to Files and Directories 📝

In Linux, everything is treated as a file—even directories! 🗂️ Understanding how to work with files and directories is a fundamental skill for navigating and managing a Linux system. This involves using various commands for basic operations like creating, viewing, locating, and linking files, as well as understanding file properties.

---

## Understanding File Paths 🛤️

Every file in the Linux **File Hierarchy System (FHS)** has a *path*—a readable representation of its location. In Linux, all files are organized under the root directory (`/`) according to the FHS. Paths use the forward-slash (`/`) to express relationships between files and directories. For example:

- `/home/packt/poem` indicates a file named `poem` located inside the `packt` directory, which is inside the `home` directory, starting from the root (`/`).

### Types of Paths

There are two types of paths in Linux:

1. **Absolute Path** 🌍: Starts from the root directory (`/`) and provides the full path to the file.
   - Example: `/home/packt/poem`
2. **Relative Path** 📍: Refers to a file relative to your current working directory (checked using the `pwd` command).
   - Example: If your current directory is `/home/packt`, the relative path to `poem` is simply `poem`.

---

## Practical Example: Working with Paths in a Docker Container 🐳

Let’s explore these concepts inside a Docker container running Ubuntu. We’ll assume we’re working as the `root` user, where the default home directory is `/root`.

### Step 1: Check Your Current Directory 🔎

Use the `pwd` command to see your current working directory:

```bash
pwd
```

**Output:**

```
/root
```

This confirms we’re in the `/root` directory.

### Step 2: Create a File ✍️

Let’s create a file named `poem` in the `/root` directory using the `touch` command:

```bash
touch poem
```

Verify the file’s creation with `ls`:

```bash
ls
```

**Output:**

```
poem
```

### Step 3: Add Content to the File 📜

Add some text to the `poem` file using the `echo` command:

```bash
echo "Hello, this is my poem!" > poem
```

### Step 4: View the File Using Absolute and Relative Paths 👀

- **Using Absolute Path**: The absolute path to the file is `/root/poem`. View its contents:

  ```bash
  cat /root/poem
  ```

  **Output:**

  ```
  Hello, this is my poem!
  ```

- **Using Relative Path**: Since we’re already in `/root`, the relative path is just `poem`:

  ```bash
  cat poem
  ```

  **Output:**

  ```
  Hello, this is my poem!
  ```

Both commands produce the same output because the relative path is based on the current directory.

---

## Working with Relative Paths: Special Characters 🎯

Relative paths often use two special characters:

- `.` **(single dot)**: Refers to the current directory.
- `..` **(double dots)**: Refers to the parent directory of the current directory.

### Example: Accessing `/etc/passwd` Using a Relative Path 📊

The `/etc/passwd` file stores user account information and has the absolute path `/etc/passwd`. Let’s access it from `/root` using a relative path.

1. **Check Current Directory**:

   ```bash
   pwd
   ```

   **Output:**

   ```
   /root
   ```

2. **Try Accessing** `passwd` **Directly**:

   ```bash
   cat passwd
   ```

   **Output:**

   ```
   cat: passwd: No such file or directory
   ```

   This error occurs because there’s no `passwd` file in `/root`. The relative path looks for the file in the current directory.

3. **Use Relative Path with** `..`: To reach `/etc/passwd` from `/root`:

   - `..` moves to the parent directory of `/root`, which is `/`.
   - Then, navigate to `/etc/passwd`. The command is:

   ```bash
   cat ../etc/passwd
   ```

   **Output:**

   ```
   root:x:0:0:root:/root:/bin/bash
   bin:x:1:1:bin:/bin:/sbin/nologin
   daemon:x:2:2:daemon:/sbin:/sbin/nologin
   ...
   ```

---

## Understanding the Example in Figure 2.7 📈

The figure demonstrates relative path usage:

- **Command:** `pwd`
  - **Output:** `/home/packt`
  - The user is in the `/home/packt` directory (in our Docker case, we’re in `/root`).
- **Command:** `cat passwd`
  - **Output:** `cat: passwd: No such file or directory`
  - This fails because `passwd` doesn’t exist in `/home/packt` (or `/root` in our case).
- **Command:** `cat ../../etc/passwd`
  - From `/home/packt`:
    - First `..` goes to `/home`.
    - Second `..` goes to `/`.
    - Then `/etc/passwd` accesses the file.
  - In our Docker case, from `/root`, we only needed `../etc/passwd` because `/root` is directly under `/`.

---

## Pro Tip: Use Tab for Autocompletion ✅

When working with paths, use the `Tab` key for autocompletion:

- Example: Type `cat ../etc/pa` and press `Tab`. It will show files in `/etc` starting with `pa` (like `passwd`).
- This helps confirm your path and saves time! ⏩

---

# **Creating Files** 📄✨

## Introduction to File Creation 🖥️

In Linux, creating files is a common task, and there are several ways to do it. This section explores two methods: using the `touch` command to create empty files and using the `echo` command with redirection to create files with content. These examples are executed inside a Docker container running Ubuntu, with the root user in the `/home/packt` directory.

---

## Method 1: Creating Files with `touch` 📁

The `touch` command is a simple way to create an empty file. It also sets you as the file owner, and the file size will be zero since it’s empty.

### Step 1: Set Up the Directory 🌐

First, navigate to the `/home` directory, create a `packt` directory, and move into it:

```bash
cd /home
mkdir packt
cd packt
pwd
```

**Output:**

```
/home/packt
```

### Step 2: Create a File with `touch` ✍️

Create a file named `new-report`:

```bash
touch new-report
```

Verify its creation:

```bash
ls
```

**Output:**

```
new-report
```

### Step 3: Check File Details 🔍

Use `ls -l` to view the file’s details:

```bash
ls -l new-report
```

**Output:**

```
-rw-r--r-- 1 root root 0 Apr 28 11:43 new-report
```

- `-rw-r--r--`: Permissions (read/write for root, read for others).
- `1`: Number of hard links.
- `root root`: Owner and group.
- `0`: File size (empty file).
- `Apr 28 11:43`: Modification time.
- `new-report`: File name.

---

## Updating Timestamps with `touch` ⏰

The `touch` command can also update a file’s timestamps without modifying its content. Linux tracks three timestamps:

- **Modification Time (mtime)**: When the file’s content was last changed.
- **Access Time (atime)**: When the file was last accessed.
- **Creation Time**: When the file was created (not always displayed).

### Step 1: Check Initial Modification Time 📅

```bash
ls -l new-report
```

**Output:**

```
-rw-r--r-- 1 root root 0 Apr 28 11:43 new-report
```

### Step 2: Update Access Time with `-a` 🔄

Use `touch -a` to update the access time:

```bash
touch -a new-report
```

Check the access time using `ls -l --time=atime`:

```bash
ls -l --time=atime new-report
```

**Output:**

```
-rw-r--r-- 1 root root 0 Apr 28 11:45 new-report
```

The access time has updated to `11:45`.

**Use Case:** Timestamps are useful with commands like `find` for more precise file searches.

---

## Method 2: Creating Files with `echo` and Redirection 📜

You can create files and add content using the `echo` command combined with redirection operators (`>` and `>>`).

### Step 1: Print Text with `echo` 🖨️

The `echo` command prints text to the screen:

```bash
echo this is a presentation
```

**Output:**

```
this is a presentation
```

### Step 2: Redirect Output to a File with `>` 💾

Redirect the text to a new file named `art-file`:

```bash
echo this is a presentation > art-file
```

View the file’s content:

```bash
cat art-file
```

**Output:**

```
this is a presentation
```

**Note:** The `>` operator creates the file if it doesn’t exist. If the file already exists, `>` overwrites it.

### Step 3: Overwrite the File 🔄

Use `>` again to overwrite the file with new content:

```bash
echo this is a new presentation > art-file
```

Check the content:

```bash
cat art-file
```

**Output:**

```
this is a new presentation
```

The original content is replaced.

### Step 4: Append to the File with `>>` 📋

Use `>>` to append new content without overwriting:

```bash
echo this is a future presentation >> art-file
```

Check the content:

```bash
cat art-file
```

**Output:**

```
this is a new presentation
this is a future presentation
```

The new text is appended to the end of the file.

---

# **Listing Files** 📋🔎

## Introduction to the `ls` Command 📜

The `ls` command is essential for listing files and directories in Linux. We’ve previously used `ls -l` for long-format listing. This section explores additional options for `ls`, demonstrating their usage inside a Docker container in the `/home/packt` directory, which contains two files: `art-file` (57 bytes) and `new-report` (0 bytes).

---

## Exploring `ls` Command Options 🛠️

### 1. `ls -lh`: Human-Readable File Sizes 📏

The `-l` option provides a long-format listing, and `-h` displays file sizes in a human-readable format (e.g., kilobytes or megabytes instead of bytes).

**Command:**

```bash
ls -lh
```

**Output:**

```
total 4.0K
-rw-r--r-- 1 root root 57 Apr 28 11:49 art-file  
-rw-r--r-- 1 root root  0 Apr 28 11:43 new-report
```

**Explanation:**

- `total 4.0K`: Total size of the directory (4 kilobytes, excluding hidden files).
- `57` and `0`: File sizes in bytes (human-readable format via `-h`).
- Other fields (permissions, owner, group, etc.) are in long format due to `-l`.

---

### 2. `ls -la`: Show All Files (Including Hidden) 🕵️

The `-a` option includes hidden files (those starting with `.`), and `-l` provides a long-format listing.

**Command:**

```bash
ls -la
```

**Output:**

```
total 12
drwxr-xr-x 2 root root 4096 Apr 28 11:48 .
drwxr-xr-x 1 root root 4096 Apr 28 11:42 ..
-rw-r--r-- 1 root root   57 Apr 28 11:49 art-file
-rw-r--r-- 1 root root    0 Apr 28 11:43 new-report
```

**Explanation:**

- `total 12`: Total size (12 kilobytes), including hidden directories (`.` and `..`).
- `drwxr-xr-x`: Permissions for directories (`.` is the current directory, `..` is the parent).
- `4096`: Default size of directories in Linux.
- `.` and `..`: Hidden entries representing the current (`/home/packt`) and parent (`/home`) directories.

---

### 3. `ls -ltr`: Sort by Modification Time (Reverse Order) ⏰

The `-t` option sorts files by modification time (newest first), and `-r` reverses the order (oldest first). The `-l` option provides a long-format listing.

**Command:**

```bash
ls -ltr
```

**Output:**

```
total 4
-rw-r--r-- 1 root root  0 Apr 28 11:43 new-report
-rw-r--r-- 1 root root 57 Apr 28 11:49 art-file
```

**Explanation:**

- `new-report` (11:43) is older than `art-file` (11:49).
- Normally, `-t` shows newest first, but `-r` reverses it, so `new-report` appears first.

---

### 4. `ls -lS`: Sort by File Size 📦

The `-S` option sorts files by size (largest first), and `-l` provides a long-format listing.

**Command:**

```bash
ls -lS
```

**Output:**

```
total 4
-rw-r--r-- 1 root root 57 Apr 28 11:49 art-file
-rw-r--r-- 1 root root  0 Apr 28 11:43 new-report
```

**Explanation:**

- `art-file` (57 bytes) is larger than `new-report` (0 bytes), so it appears first.

---

### 5. `ls -R`: Recursive Listing 🌳

The `-R` option lists the contents of the current directory and its subdirectories recursively.

**Command:**

```bash
ls -R
```

**Output:**

```
.:
art-file  new-report
```

**Explanation:**

- `.:` represents the current directory (`/home/packt`).
- Only `art-file` and `new-report` are present since there are no subdirectories.

---

## Long Listing with `ls -la` 📋

The `ls -la` command, often called "long listing," is a widely used method to list files. It shows detailed information (via `-l`) and includes hidden files (via `-a`). The output above for `ls -la` is an example of long listing, showing permissions, ownership, sizes, timestamps, and hidden entries like `.` and `..`.

---