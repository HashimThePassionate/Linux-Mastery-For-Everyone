#  **The Linux Shell with Docker** 🚀

1. 📥 Pull and run an Ubuntu container from docker
2. 👤 Check your username inside the container
3. 🐚 Verify your default shell

Let's dive in! 🎉

---

## 🐳 1. Pull & Run Ubuntu Container

Now we’ll fetch the latest Ubuntu image and launch it:

1. **Open PowerShell or Windows Terminal or vs code command prompt as a normal user.**

2. **Pull the Ubuntu image**:

   ```bash
   docker pull ubuntu:latest
   ```

   - This downloads the official Ubuntu Docker image.

3. **Run the container interactively**:

   ```bash
   docker run -it --name my-ubuntu ubuntu:latest bash
   ```

   - `-it`: Interactive terminal mode
   - `--name my-ubuntu`: Assigns the name `my-ubuntu` to your container
   - `bash`: Launches the Bash shell inside the container

4. **You’ll see a prompt** like:

   ```bash
   root@<container-id>:/#
   ```

   - This indicates you’re inside the Ubuntu container!

---

## 👤 2. Check Your Username

Inside the container, find out which user you’re logged in as:

- **Method A**: Using `whoami`

  ```bash
  whoami
  ```

  - Prints your current username (usually `root` by default).

🔍 **Why this matters:** Knowing your user helps you manage permissions and understand which account you’re using for commands.

---

## 🐚 3. Verify Your Default Shell

To confirm which shell binary you’re running:

- **Quick check**:

  ```bash
  echo $0
  ```

  - Displays the name of the shell process (e.g., `-bash`).

- **Deep dive**: Inspect your `/etc/passwd` entry using `cat` and `grep`
  ```bash
  cat /etc/passwd | grep root
  ```
  - This streams the entire `/etc/passwd` file into `grep`, which filters and displays only the lines containing “root.”
  - You should see an entry like:
    ```text
    root:x:0:0:root:/root:/bin/bash
    ```
  - The **last field** (`/bin/bash`) shows your default shell path.
  - Using `cat /etc/passwd | grep root` is a simple way to verify your root user’s shell without needing to construct dynamic patterns.

> 🔑 **Key point:** The default shell determines how commands are interpreted. Bash is the most common on Linux.

---

## 🎯 Next Steps

- Practice basic Linux commands: `ls`, `cd`, `mkdir`, `apt update`, etc.
- Explore file permissions and editing files with `nano` or `vi`.
- When ready, exit the container:
  ```bash
  exit
  ```
- To restart later:
  ```bash
  docker start my-ubuntu
  docker attach my-ubuntu
  ```

---