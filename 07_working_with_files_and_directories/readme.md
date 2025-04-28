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