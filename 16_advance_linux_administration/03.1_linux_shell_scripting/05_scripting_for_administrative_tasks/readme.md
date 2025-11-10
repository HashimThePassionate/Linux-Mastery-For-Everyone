# ⚙️ Creating Scripts for System Administrative Tasks

<details>
<summary>Table of Contents</summary>

- [⚙️ Creating Scripts for System Administrative Tasks](#️-creating-scripts-for-system-administrative-tasks)
  - [🚀 An Updating Script Example](#-an-updating-script-example)
    - [Code Snapshot: `update_script.sh`](#code-snapshot-update_scriptsh)
    - [👨‍💻 Code Explanation](#-code-explanation)
    - [🔑 Handling the `sudo` Password](#-handling-the-sudo-password)
      - [1. Add User `hashim` to the `sudo` Group](#1-add-user-hashim-to-the-sudo-group)
      - [2. Configure Password-less `sudo`](#2-configure-password-less-sudo)
  - [⏰ Scheduling Scripts in Linux with `cron`](#-scheduling-scripts-in-linux-with-cron)
    - [Understanding `crontab` Syntax](#understanding-crontab-syntax)
    - [Setting Up Our Cron Job](#setting-up-our-cron-job)
    - [Managing Cron Jobs](#managing-cron-jobs)
    - [Silencing Cron Job Output](#silencing-cron-job-output)
  - [🗄️ A Backup Script Example](#️-a-backup-script-example)
    - [Code Snapshot: `backup_script.sh`](#code-snapshot-backup_scriptsh)
    - [👨‍💻 Code Explanation](#-code-explanation-1)
  - [🔐 A Random Password Generator Script](#-a-random-password-generator-script)
    - [Code Snapshot: `passgen_script.sh`](#code-snapshot-passgen_scriptsh)
    - [👨‍💻 Code Explanation](#-code-explanation-2)
    - [🖥️ Example Output](#️-example-output)
  - [📦 Packaging Scripts](#-packaging-scripts)
    - [Creating Debian (`.deb`) Packages from Source](#creating-debian-deb-packages-from-source)
    - [📝 Steps for Creating a Debian (`.deb`) Package](#-steps-for-creating-a-debian-deb-package)
      - [1. Create a Directory](#1-create-a-directory)
      - [2. Create a License File](#2-create-a-license-file)
      - [3. Move the Bash Script Inside the Directory](#3-move-the-bash-script-inside-the-directory)
      - [4. Add the Script to the System (Optional Test)](#4-add-the-script-to-the-system-optional-test)
      - [5. Create the Tarball](#5-create-the-tarball)
      - [6. Install Specific Debian Packaging Utilities](#6-install-specific-debian-packaging-utilities)
      - [7. Set Up the Workspace](#7-set-up-the-workspace)
      - [8. Add the Tarball...](#8-add-the-tarball)
      - [9. Configure the `debian/` Files](#9-configure-the-debian-files)
      - [10. Build the `.deb` Package](#10-build-the-deb-package)

</details>

---

In this section, we will show you a couple of scripts for administrative tasks. As we stated at the beginning of this chapter, a shell script is a sequence of Bash commands that are executed when the file is running. In the following subsections, we will create two scripts that will do two different administrative tasks.


## 🚀 An Updating Script Example

This script will use the `apt` command to update the system at a specified time. It is a very basic script that runs just a simple command and shows some messages to standard output.

The simplest way would be to run the following command inside the script:

```bash
# This command first updates the package lists,
# then upgrades all installed packages
sudo apt update && sudo apt upgrade -y
```

### Code Snapshot: `update_script.sh`

Here is the full code for the script, which we'll assume is saved as `/home/hashim/update_script.sh`.

```bash
#!/bin/bash
# a simple update script
echo "Starting the update process..."

# First, update the package lists
sudo apt update

# Then, perform the upgrade
sudo apt upgrade -y

echo "System update is finished! Enjoy!"
```

### 👨‍💻 Code Explanation

  * `#!/bin/bash`: This is the "shebang." It tells the system to execute this script using the Bash shell.
  * `# a simple update script`: This is a comment. The system ignores it, but it's helpful for humans.
  * `echo "Starting the update process..."`: This prints a status message to the terminal so the user knows the script has started.
  * `sudo apt update`: This is the first core command.
      * `sudo`: (Super User Do) This executes the command with administrative (root) privileges, which are required for system-wide updates.
      * `apt`: (Advanced Package Tool) This is the default package manager for modern Debian-based distributions.
      * `update`: This command **refreshes the local list of available packages** from the server. It *does not* install anything.
  * `sudo apt upgrade -y`: This is the second core command.
      * `upgrade`: This command compares the new package lists (from `apt update`) to the packages currently installed and upgrades any that have new versions.
      * `-y`: This flag automatically answers "yes" to any prompts, such as "Are you sure you want to install these updates?" This is essential for automation.
  * `echo "System update is finished! Enjoy!"`: This prints a final message to the terminal, letting the user know the process is complete.


### 🔑 Handling the `sudo` Password

The issue with the script above is that it requires the `sudo` password. This defies the purpose of automation because the user (`hashim`) must provide the password manually. Let’s learn how to overcome this issue.

#### 1\. Add User `hashim` to the `sudo` Group

First, your user must have `sudo` privileges. The standard way to do this on Ubuntu is to add the user to the `sudo` group.

```bash
# Run this command as a user who *already* has sudo privileges, or as root
sudo usermod -aG sudo hashim
```

  * **`sudo`**: Runs the command as root.
  * **`usermod`**: A utility to **mod**ify a **user** account.
  * **`-aG`**: This is a combination of two flags:
      * `-a`: **A**ppend the user to a group. This is critical. Without `-a`, you would *replace* all current groups with just `sudo`.
      * `-G`: Specifies the **G**roup to append to (in this case, `sudo`).
  * **`hashim`**: The user you are adding to the group.

After running this, the user `hashim` will need to **log out and log back in** for the new group membership to take effect.

#### 2\. Configure Password-less `sudo`

Once `hashim` is in the `sudo` group, you *still* need to enter a password. To make the script fully automatic, you can disable the password requirement.

You will have to edit the `/etc/sudoers` file. (It is highly recommended to use the `sudo visudo` command to edit this file, as it prevents syntax errors).

> **Important Note**
>
> Please take into consideration that this method will have **system-wide effects**, not only on the script you want to run. It is **not considered a safe measure** and we advise you to use it with extreme care and consideration on any production system.

To allow users in the `sudo` group to run `sudo` commands without a password, you will modify the following line:

This is the original line in `/etc/sudoers`:

```
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL
```

...change it to...

```
# Allow members of group sudo to execute any command without a password
%sudo   ALL=(ALL:ALL) NOPASSWD: ALL
```

This will provide a password-less action for every user that is in the `sudo` group, which now includes `hashim`.

Now, we will have to schedule the script to run at a certain time. Don’t forget to make your script file executable to be able to run it.

```bash
chmod +x /home/hashim/update_script.sh
```

As we are using a server distribution, we assume that this machine is running 24/7 without interruptions, so running at startup would make no sense. Furthermore, we want to ensure that the system is always up to date. To schedule the script, we will use `cron` and `crontab`.


## ⏰ Scheduling Scripts in Linux with `cron`

Using `crontab` might look scary at first, but once you get to know it, you will find it very useful. Perhaps the most intimidating aspect of using `cron` jobs is the definition process, especially the schedule setup.

### Understanding `crontab` Syntax

A very useful example of how to set the cron date is provided inside the `/etc/crontab` file. We believe that Figure 8.54 is self-explanatory.

Here is a breakdown of the fields shown in the example:

1.  **Minute** (0 - 59)
2.  **Hour** (0 - 23)
3.  **Day of Month** (1 - 31)
4.  **Month** (1 - 12) OR (jan, feb, mar, ...)
5.  **Day of Week** (0 - 6) (Sunday=0 or 7) OR (sun, mon, ...)
6.  **User-name** (The user account that will run the command)
7.  **Command to be executed**

### Setting Up Our Cron Job

Now, let’s set up a new `cron` job for our updating script. We will use the `crontab -e` command. This command will use the default shell text editor (often **Nano**).

Running the `crontab -e` command will start a new editor instance where the job should be written. **Note:** When using `crontab -e`, you are editing the file for your *own user* (`hashim`), so you *omit* the `user-name` field (field 6).

Here is our code:

```bash
00 23 * * 0 /home/hashim/update_script.sh
```

Let’s explain this. In the preceding example, we set the new update script file to run at **23:00** (11:00 PM) every **Sunday** (Day of Week = 0).

*If you were editing the system-wide `/etc/crontab` file directly, you would include the username as shown in the original text:*

```bash
00 23 * * 0 hashim /home/hashim/update_script.sh
```

### Managing Cron Jobs

Once the line of code is created, you can check if the `cron` job was created by using the `crontab -l` command.

```bash
hashim@Hashim:~$ crontab -e
# (Editor opens to add the line)

hashim@Hashim:~$ crontab -l
00 23 * * 0 /home/hashim/update_script.sh
```

If you want to see the `cron` jobs of *other* users, you can use the following command (as root):

```bash
hashim@Hashim:~$ sudo crontab -u hashim -l
```

### Silencing Cron Job Output

The following snippet shows the code, which can be enhanced inside `crontab` with an output and error message redirection to `/dev/null`. This is useful when we don’t want to see the command’s output or if any errors occur and we don’t need to see them. The line would be modified as follows:

```bash
00 23 * * 0 /home/hashim/update_script.sh > /dev/null 2>&1
```

  * `> /dev/null`: This redirects all standard output (STDOUT) to `/dev/null`, a "black hole" that discards all data.
  * `2>&1`: This redirects all standard error (STDERR, channel `2`) to the *same place* as standard output (STDOUT, channel `&1`), which is already pointed to `/dev/null`. This effectively silences all output from the script.

`crontab` is a powerful tool that will prove of great help whenever you need to schedule tasks in Linux. However, it is not the only tool available for the job. Feel free to explore other tools available, such as the `at` command.


## 🗄️ A Backup Script Example

Another common use for scripting is for backing up files. We will create a new script for this task and we will schedule it according to our needs.

### Code Snapshot: `backup_script.sh`

The script’s code is shown in Figure 8.55. The code is backing up the `/home/hashim` directory to the `/mnt/backup` directory.

```bash
#!/bin/bash
#which directories to backup
backup_dirs="/home/hashim"
#destination
destination="/mnt/backup"
#the archive file
date_backup=$(date +"%Y-%m-%d")
hostname=$(hostname)
file="$date_backup-$hostname.tgz"
printf "%s\n" "Backing up $backup_dirs to $destination/$file"
#backing up with tar
tar czf $destination/$file $backup_dirs
printf "%s\n" "Backup finished..."
#listing the backup destination
ls -lh $destination
```

### 👨‍💻 Code Explanation

  * `#!/bin/bash`: The shebang, telling the system to use Bash.
  * `backup_dirs="/home/hashim"`: A variable holding the directory we want to back up (our user's home directory).
  * `destination="/mnt/backup"`: A variable holding the location where the backup will be saved.
  * `date_backup=$(date +"%Y-%m-%d")`:
      * `$(...)`: This is command substitution. It runs the command inside and captures its output.
      * `date +"%Y-%m-%d"`: This gets the current date and formats it as `YYYY-MM-DD`.
  * `hostname=$(hostname)`: This runs the `hostname` command and saves the computer's name (e.g., `Hashim`) to the `hostname` variable.
  * `file="$date_backup-$hostname.tgz"`: This combines the variables to create a unique filename, like `2025-11-10-Hashim.tgz`.
  * `printf "%s\n" "..."`: This is a more robust way to print a formatted string with a newline (`\n`). It tells the user what is about to happen.
  * `tar czf $destination/$file $backup_dirs`: This is the main backup command.
      * `tar`: The **T**ape **AR**chive utility, used for bundling files.
      * `c`: **C**reates a new archive.
      * `z`: Compresses the archive using g**z**ip.
      * `f`: Specifies the **f**ilename for the archive (which is `$destination/$file`).
      * `$backup_dirs`: The source directory to add to the archive.
  * `ls -lh $destination`: After the backup, this command lists the destination directory so you can confirm the file was created.
      * `-l`: Shows a **l**ong list format.
      * `-h`: Shows file sizes in **h**uman-readable format (e.g., `2.5M` instead of `2516582`).

> **Important Note**
>
> Using the `date` format can be daunting and intimidating at first, but knowing how to use it in your scripts will prove invaluable. Use the `date --help` command for more information on all the available formatting options.


## 🔐 A Random Password Generator Script

Keeping your online or local accounts secure is extremely important, so secure passwords should be used as a general rule. We will provide you with an example of a script that generates random passwords.

There are many password generators available to use in Linux (such as `pwmake`), but we thought that it would be fun if you created such a small script by yourself.

We will use **`openssl`** and an encoding mechanism called **`base64`**. For more information on `base64`, [visit the MDN docs](https://developer.mozilla.org/en-US/docs/Glossary/Base64). Keep in mind that there are many other ways to generate random characters in Linux, one highly used being the `/dev/urandom` pseudo-random number generator. Feel free to explore as many ways as you like.

### Code Snapshot: `passgen_script.sh`

Here is our code for the password-generating script, as shown in Figure 8.56.

```bash
#!/bin/bash
printf "%s\n" "*** Random password generator ***"
printf "\n"
printf "%s" "Number of characters (we recommend >8): "
read n
printf "%s" "How many passwords you want to generate? "
read p
printf "\n"
for (( i=0; i<$p; i++ ))
do
    if [ $n -le 8 ]
    then
        echo "The password is too short. Try again please!"
        exit 1
    else
        openssl rand -base64 $n
    fi
done
printf "\n"
```

### 👨‍💻 Code Explanation

1.  `#!/bin/bash`: The shebang.
2.  `printf ...` / `read ...`: These commands print the title and prompts, then `read` the user's answers into the `n` (number) and `p` (passwords) variables.
3.  `for (( i=0; i<$p; i++ ))`: A C-style loop that runs `$p` times.
4.  `if [ $n -le 8 ]`: A test to see if the requested length (`$n`) is **l**ess than or **e**qual to 8.
5.  `echo "..."` / `exit 1`: If the password is too short, print an error and exit the script with an error code.
6.  `openssl rand -base64 $n`: This is the core command. It uses `openssl` to generate `$n` random bytes and then encodes them using `-base64` to make them printable.
7.  `done`: Ends the `for` loop.

### 🖥️ Example Output

When you run the script, it will look just like Figure 8.57.

```
*** Random password generator ***

Number of characters (we recommend >8): 12
How many passwords you want to generate? 2

f1YT+q3LpxmdYhsn
iXIQkZ+pLug1P8K2
```

Scripting is an invaluable task you should master. We barely scratched the surface of shell scripting, but the information in this chapter should give you a good start so that you can start developing sysadmin scripts.


## 📦 Packaging Scripts

In the next subsection, we will show you how to package your scripts into full-fledged apps that can be installed on the Linux command line. This is an addition to *Chapter 3*, where you learned about Linux package management.

Bash scripts are pieces of software like any other, so they can be deployed with Linux distributions as platform-specific packages. Just like you would package any other piece of software code, you can package a piece of shell scripting code.

In this section, as an add-on to the knowledge you gathered in *Chapter 3*, we will show you how to package a Bash script. As stated before, the information provided here can easily be used for other software sources (usually, in Linux, there is C/C++, Python, Rust, Java, or Go).


### Creating Debian (`.deb`) Packages from Source

To create a `.deb` package, we will use the password generator script we developed in the previous section.

> **Important Note: Programming Language Types**
>
> Usually, software programs are developed using human-readable source code. This code needs to be translated into machine code so that the computer will understand it. There are several large generic types of programming languages:
>
>   * **Interpreted** (such as Python or Bash): The source code is executed line by line by another program (the interpreter), without a prior compilation step.
>   * **Compiled** (such as C/C++, Java, and Go): The source code is first translated into a machine-code binary, which is then run.
>
> For more information, check out the [Wikipedia article on programming language types](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type).

Often, the software source code is distributed in the form of compressed archives (**`.tar.gz`** files or **tarballs**) that are then packaged into `.deb` packages. The archive usually consists of the source code file and a **license file**.

> **Important Note: FOSS Licenses**
>
> This license file offers information about the type of license the software is distributed with. In the case of free and open source software (FOSS), the license is usually **GPLv3**, but other types such as the **MIT**, **Apache**, and **BSD** licenses are also used.
>
> For detailed information on FOSS license types, [check out the Wikipedia comparison](https://en.wikipedia.org/wiki/Comparison_of_free_and_open-source_software_licenses).


### 📝 Steps for Creating a Debian (`.deb`) Package

Here are the necessary steps for creating a `.deb` package from our Bash script.

#### 1\. Create a Directory

First, we will create a new directory. The Debian convention is `packagename-version`. We'll do this in our home directory (`/home/hashim`).

```bash
hashim@Hashim:~$ cd ~
hashim@Hashim:~$ mkdir passgen-0.1
```

#### 2\. Create a License File

For our Bash script, we will create a license file to use with the package.

```bash
hashim@Hashim:~$ cd passgen-0.1
hashim@Hashim:~/passgen-0.1$ nano LICENSE
```

*(You can find the text to add to your file at [https://www.gnu.org/licenses under the "How to Apply These Terms" section](https://www.gnu.org/licenses).)*

#### 3\. Move the Bash Script Inside the Directory

We will copy the `passgen_script.sh` file (which we'll assume is in `/home/hashim`) into our new directory and give it the name it will be installed with.

```bash
hashim@Hashim:~/passgen-0.1$ cp ~/passgen_script.sh ./passgen
```

#### 4\. Add the Script to the System (Optional Test)

We will add the `passgen` script to the Filesystem Hierarchy Standard `$PATH` using the `install` command.

```bash
hashim@Hashim:~/passgen-0.1$ sudo install -m 0755 passgen /usr/bin/passgen
```

This ensures that we can run the `passgen` script from anywhere without using the full path.

#### 5\. Create the Tarball

Debian packaging tools expect an "original source tarball" named in a specific way: `packagename_version.orig.tar.gz`.

Go *outside* your `passgen-0.1` directory (back to `/home/hashim`) and create the tarball:

```bash
hashim@Hashim:~/passgen-0.1$ cd ..
hashim@Hashim:~$ tar -cvzf passgen_0.1.orig.tar.gz passgen-0.1
```

  * `c`: **C**reate an archive.
  * `v`: **V**erbosely list files processed.
  * `z`: Compress the archive with g**z**ip.
  * `f`: Use the following **f**ilename.

#### 6\. Install Specific Debian Packaging Utilities

To create `.deb` packages, we need some specific utilities.

```bash
hashim@Hashim:~$ sudo apt install build-essential dh-make
```

#### 7\. Set Up the Workspace

The Debian workflow happens *inside* the source directory.

First, unpack the tarball you just made (this seems redundant, but it's the standard workflow):

```bash
hashim@Hashim:~$ tar -xzf passgen_0.1.orig.tar.gz
hashim@Hashim:~$ cd passgen-0.1
```

Now, run `dh_make` to create the packaging "scaffolding":

```bash
hashim@Hashim:~/passgen-0.1$ dh_make -s --createorig
```

  * `-s`: for a **s**ingle binary package.
  * This command will ask you a few questions (like package type and email). After it finishes, it creates a new `debian/` directory inside `passgen-0.1`.

This `debian/` directory contains all the build information, structured as follows:

  * `debian/control`: Contains metadata like name, version, and dependencies.
  * `debian/rules`: A Makefile that automates the build.
  * `debian/changelog`: A mandatory file tracking changes.
  * `debian/install`: A file *we will create* to list what to install and where.

#### 8\. Add the Tarball...

This step (adding the tarball to a `SOURCES` directory) is not needed in the Debian workflow. `dh_make` has already located the `passgen_0.1.orig.tar.gz` in the parent directory.

#### 9\. Configure the `debian/` Files

This is the most important part. We must edit/create a few files in the `debian/` directory.

**A. `debian/control`**
Edit this file to look like the following. We fill in the `Description`, update the `Maintainer`, and change `Depends` to just `bash`. `Architecture: all` is the equivalent of `BuildArch: noarch`.

```spec
Source: passgen
Section: utils
Priority: optional
Maintainer: Hashim <hashim@Hashim>
Build-Depends: debhelper-compat (= 13)
Standards-Version: 4.6.0
Homepage: https://www.example.com/passgen

Package: passgen
Architecture: all
Depends: bash, ${misc:Depends}
Description: A bash password generator script
 This is a longer description for the password
 generator implemented in bash.
```

**B. `debian/install`**
This file does not exist by default. We must create it.

```bash
hashim@Hashim:~/passgen-0.1$ nano debian/install
```

Add this single line, which tells the builder to take the file `passgen` (from our source) and install it into `/usr/bin/`.

```
passgen usr/bin
```

**C. `debian/changelog`**
This file was created by `dh_make`. We just need to add a note for our initial release.

```
passgen (0.1-1) UNRELEASED; urgency=medium

  * Initial release.

 -- Hashim <hashim@Hashim>  Mon, 10 Nov 2025 05:17:35 +0500
```

#### 10\. Build the `.deb` Package

Finally, we build the package. The command to use is `dpkg-buildpackage`. We run this from *inside* the `passgen-0.1` directory.

```bash
hashim@Hashim:~/passgen-0.1$ dpkg-buildpackage -b -us -uc
```

  * `-b`: binary package
  * `-us -uc`: do not sign the source/changelog (for this test)

The build will run, and if successful, you can find the new `.deb` file in the directory *above* (`..`), in `/home/hashim`.

```bash
hashim@Hashim:~/passgen-0.1$ cd ..
hashim@Hashim:~$ ls
passgen-0.1/
passgen_0.1-1_all.deb
passgen_0.1-1.debian.tar.xz
passgen_0.1-1.dsc
passgen_0.1.orig.tar.gz
```

The file you want is **`passgen_0.1-1_all.deb`**. (This replaces the `.rpm` file shown in Figure 8.60).

Here you are, building your very first Debian binary package from a Bash script file\! You can do this for any type of source file you might develop. Building binary packages is not a very difficult task, and with good documentation about the specifics of the `debian/` files, you are good to go.

---