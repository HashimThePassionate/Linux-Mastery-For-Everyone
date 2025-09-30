# **Exploring the Linux Shell** 🚀

Back in `Section, The Linux Shell and Filesystem`, we introduced you to the shell by exploring the available virtual consoles, command types, and the filesystem. This gave you a fair foundation for what we are about to explore in this chapter. 📚

By now, with everything we have been showing you in this book, you are already well-versed in using the command line. You know some of the most common and useful commands available in Linux, as we explored:
* File operations 📂
* Package management 📦
* User and disk management 👤💾
* Network administration 🌐

All this knowledge will eventually be put to use in this chapter, where we will explore **advanced shell features**, **shell variables**, **regular expressions**, and how to take advantage of the powerful **programming and automation features** of the Bash shell. 🛠️

---

# **Bash Shell Features** 🐚

The shell not only runs commands but also has many more features that make a system administrator’s life more comfortable while at the command line. 😊

Some of these features include the use of **wildcards** and **metacharacters**, **brace expansion**, and **variables**. Apart from these, you will learn about the shell’s `PATH` and **aliases**.

### A Little Bit of History: The POSIX Standard 📜

Before we proceed, let’s dig into a little history about the standards the shell is based on.

Back in the day, when **UNIX** emerged as an operating system, the need for a standard to oversee different variants appeared. Thus, the **Institute of Electrical and Electronics Engineers (IEEE)** created the **Portable Operating System Interface (POSIX)** as a family of different standards that were meant to assure compatibility between operating systems. 🤝

Therefore, **UNIX** and **Linux**, as well as **macOS** (based on Darwin, the kernel of macOS derived from UNIX), **AIX**, **HP-UX**, and **Oracle Solaris**, are **POSIX compliant**.

POSIX has different standards for:
* The C language API
* File format definitions
* Directory structures
* Environment variables definitions
* Locale specifications
* Character sets
* Regular expressions

---

# 🐧 **Wildcards and Metacharacters in Linux**

In Linux, **wildcards** are used to match filenames. There are three main types of wildcards:

* `*` **Asterisk**: This is used to match any string of none or more characters. For example, `file*` would match `file`, `file1`, `filename`, and so on. ✨
* `?` **Question mark**: This is used to match a single character. For example, `file?` would match `file1`, `fileA`, but not `file12`. 🤔
* `[ ]` **Brackets**: This is used to match any of the characters inside the brackets. For example, `file[123]` would match `file1`, `file2`, or `file3`, but not `file4`. 🔢

***

## ⚙️ Metacharacters

**Metacharacters** are special characters that are used in Linux and any Unix-based system. These metacharacters are as follows:

| Metacharacter | Description                     | Metacharacter | Description                        |
|----------------|---------------------------------|----------------|------------------------------------|
| >             | Output redirection             | <              | Input redirection                 |
| >>            | Output redirection - append    | <<             | Input redirection - append        |
| *             | File substitution wildcard     | ?              | File substitution wildcard        |
| [ ]           | File substitution wildcard     | :              | Command execution sequence        |
| &#124;        | Pipe for using multiple commands | ( )           | Group of commands in the execution sequence |
| &#124;&#124;  | Conditional execution (OR)     | &&             | Conditional execution (AND)       |
| &             | Run a command in the background | #              | Use a command directly in the shell |
| $             | Variable value expansion       | \              | The escape character              |
| `cmd`         | Command substitution           | $(cmd)         | Command substitution              |


---


# 🚀 **Advanced Command Usage in Linux**

Let’s look at two examples that use metacharacters for **command substitution**. We use the output of one command inside another command. This can be done in two ways, as shown below.

### 🔄 Command Substitution

The purpose of this is to show you how command substitution works in the shell. We are using the output of the `date` command inside the `echo` command.

#### Code Snippet

```bash
hashim@Hashim:~$ echo "Today is `date`"
Today is Tue Sep 30 03:38:53 PM PKT 2025

hashim@Hashim:~$ echo "Today is $(date)"
Today is Tue Sep 30 03:39:26 PM PKT 2025
```

#### Explanation of Commands

  * `echo`: This is one of the simplest commands in Linux. It just prints or "echoes" the message between the quotes to your screen (the standard output).
  * `date`: This command outputs the current date and time of the system.
  * **How it works**: In both examples, the shell first executes the `date` command. It then takes the output of `date` (e.g., "Tue Sep 30 03:38:53 PM PKT 2025") and substitutes it into the `echo` command's string. Finally, `echo` prints the complete string. You can use either backticks (`` `date` ``) or a dollar sign with parentheses (`$(date)`) to perform command substitution.

-----

### 🔗 Using the Pipe (`|`)

You can also combine two or more commands using a **pipe**. The pipe `|` sends the output of the first command to be used as the input for the second command.

In the following example, we’re using the `ls -l /etc` command to get a detailed (long) listing of all files and directories inside the `/etc` directory. We will then "pipe" this long list to the `less` command.

#### Code Snippet

```bash
ls -l /etc | less
```

#### Explanation of Commands

  * `ls -l /etc`: This command lists the contents of the `/etc` directory in a long format, showing permissions, owner, size, and modification date.
  * `|`: The pipe symbol takes the entire output from the `ls -l /etc` command.
  * `less`: This command is a pager, which means it displays content one screen at a time. It receives the list from the `ls` command and shows it to you. This is very helpful for long outputs that don't fit on a single screen.
  * **How to navigate**: Once you are in the `less` view, you can use the **arrow keys** (`↑`, `↓`) or the **Page Up** and **Page Down** keys to navigate through the entire output.

The pipe and command substitution are very useful, especially when you’re working with complex commands or when writing scripts.

-----

### ⛓️ Command Sequencing, Grouping, and Redirection

Now, let’s execute some commands in a sequence. After that, we will use metacharacters to group the commands and redirect their combined output to a file.

#### Code Snippet

```bash
hashim@Hashim:~$ who; pwd;
hashim
/home/hashim

hashim@Hashim:~$ (who; pwd) > users

hashim@Hashim:~$ ls
Desktop   Downloads  Music     Public    Templates  Videos
Documents lvm        Pictures  snap      users

hashim@Hashim:~$ cat users
hashim
/home/hashim
```

#### Step-by-Step Explanation

1.  `who; pwd;`

      * The semicolon (`;`) is a command separator. It tells the shell to run the first command (`who`), and after it finishes, run the second command (`pwd`).
      * `who`: This command prints information about the users who are currently logged in.
      * `pwd`: This command prints the "present working directory," which is your current location in the filesystem.
      * The output of both commands is printed directly to the terminal.

2.  `(who; pwd) > users`

      * `( ... )`: The parentheses group the commands together. The shell treats `who; pwd` as a single unit.
      * `>`: This is the output redirection metacharacter. It takes the entire output from the grouped commands and, instead of printing it to the screen, writes it into a file.
      * `users`: This is the name of the file where the output will be saved. If the file `users` does not exist, it will be created. If it already exists, its contents will be overwritten.

3.  `ls`

      * This command lists the files and directories in the current location. As you can see in the output, the new file named `users` has been created.

4.  `cat users`

      * `cat`: This command is used to display the contents of files. Here, it concatenates (which is what `cat` stands for) and prints the contents of the `users` file to the screen, showing you the output from the `who` and `pwd` commands that was saved earlier.

---

# 🌀 **Brace Expansion in Linux**

Curly brackets (`{}`) can be used to expand the arguments of a command. Brace expansion is not limited to filenames like a wildcard; it works with any type of string. Inside these braces, you can use a single string, a sequence of numbers or letters, or several strings separated by commas.

In this section, we will look at some examples of how to use this type of expansion.

-----

### 🗑️ Deleting Specific Files

First, we will use brace expansion to delete multiple specific files from a directory. Let’s say that we have two files named `report` and `new-report` along with the `users` file from a previous example inside our current directory. We want to delete them all at once.

#### Code Snippet

The command `rm {report,new-report,users}` is used. The shell expands this command into `rm report new-report users` before running it.

```bash
hashim@Hashim:~$ ls
Desktop    lvm         Pictures  snap       Videos
Documents  Music       Public    Templates
Downloads  new-report  report    users

hashim@Hashim:~$ rm {report,new-report,users}

hashim@Hashim:~$ ls
Desktop    Downloads  Music     Public    Templates
Documents  lvm        Pictures  snap      Videos
```

#### Explanation

As you can see from the output, the `ls` command first shows that the files `report`, `new-report`, and `users` exist. After running the `rm` command with brace expansion, the next `ls` command confirms that all three files have been deleted.

-----

### createFile Creating Files in a Sequence

Next, we will show you how to create multiple new files using brace expansion. To create multiple files (five of them, for example) that share parts of their name, like `file1`, `file2`, … `file5`, we can use the following command.

#### Code Snippet

```bash
hashim@Hashim:~$ touch file{1..5}

hashim@Hashim:~$ ls
Desktop    file1  file4  Music     snap
Documents  file2  file5  Pictures  Templates
Downloads  file3  lvm    Public    Videos
```

#### Explanation

  * `touch`: This is a standard Linux command used to create empty files. If a file already exists, `touch` updates its modification time.
  * `file{1..5}`: This is the brace expansion. The shell sees `{1..5}` and expands it into a sequence of numbers from 1 to 5. It then prepends the string `file` to each number in the sequence. The command that actually gets executed is `touch file1 file2 file3 file4 file5`.
  * The `ls` command then shows that all five files have been successfully created.

-----

### 🔥 Deleting Files in a Sequence

Now that we’ve created those files, you can use the same brace expansion logic to delete multiple files at once.

#### Code Snippet

Type the following command into your console:

```bash
rm file{1..5}
```

#### Explanation

This command will delete all five files we created previously (`file1`, `file2`, `file3`, `file4`, and `file5`). The shell expands `file{1..5}` into a list of all five filenames, and the `rm` command then removes them. You can use the `ls` command afterward to see that the contents of the present working directory no longer include those files.

---

# 🏷️ **The Shell's Aliases**

The Linux shell supports **aliases**, which are a very convenient way to create short commands as substitutes for longer ones. For example, in Ubuntu, there is a predefined alias called `ll` that is shorthand for `ls -alF`.

You can define your own aliases too. You can make them **temporary** (for your current session only) or **permanent**, similar to variables.

-----

### ⏳ Creating a Temporary Alias

In the following example, we check the current alias for the `ll` command and then change it for our current session.

#### Code Snippet

```bash
hashim@Hashim:~$ alias ll
alias ll='ls -alF'

hashim@Hashim:~$ alias ll='ls -l'

hashim@Hashim:~$ alias ll
alias ll='ls -l'
```

#### Explanation

1.  `alias ll`: We first type this to see what the `ll` alias is currently set to. The output shows it's `ls -alF`.
2.  `alias ll='ls -l'`: We then redefine the alias. Now, whenever we type `ll`, it will execute `ls -l` instead.
3.  `alias ll`: We run the command again to confirm that the change was successful.

This modification is **only temporary**. It will revert to the default version after you reboot your computer or restart your shell session.

-----

### 💾 Making Aliases Permanent

If you want to make an alias permanent, you should edit the `~/.bashrc` file and add the alias definition inside it.

To do this, open the file with your preferred text editor (like `nano` or `vim`) and add the same lines you used in the terminal to the file. Save the file and then "execute" it to apply the changes.

A better practice is to add your custom aliases to a separate file called `~/.bash_aliases`. You can view the default contents of `.bashrc` for more information on how to use aliases.

> #### 📜 Important Note on `.bashrc`
>
> The `~/.bashrc` file is a hidden script file in your home directory that contains different configurations for your terminal sessions. This file can also contain functions that help you automate repetitive tasks. It is automatically executed every time you log in or open a new terminal, but it can also be manually executed by using the command `source ~/.bashrc`.

-----

### 🛠️ A Practical Example

Let's create a useful alias called `update` that updates and upgrades the system, and then make it permanent.

#### Code Snippet

```bash
hashim@Hashim:~$ alias update='sudo apt update && sudo apt upgrade -y'

hashim@Hashim:~$ alias update
alias update='sudo apt update && sudo apt upgrade -y'

hashim@Hashim:~$ nano ~/.bash_aliases

hashim@Hashim:~$ source ~/.bashrc
```

#### Step-by-Step Explanation

1.  `alias update='sudo apt update && sudo apt upgrade -y'`

      * This command creates a new temporary alias named `update`.
      * `sudo apt update`: This part refreshes your system's list of available software packages.
      * `&&`: This is a logical **AND** operator. The command that follows it will only run if the first command (`sudo apt update`) completes successfully.
      * `sudo apt upgrade -y`: This part upgrades all the installed packages to their latest versions. The `-y` flag automatically answers "yes" to any confirmation prompts.

2.  `alias update`

      * This command simply displays the definition of the `update` alias we just created, confirming it's set correctly for the current session.

3.  `nano ~/.bash_aliases`

      * This command opens the `.bash_aliases` file located in your home directory (`~/`) using the `nano` text editor.
      * Inside this file, you would add the following line:
        ```bash
        alias update='sudo apt update && sudo apt upgrade -y'
        ```
      * You would then save the file and exit the editor. This step makes the alias permanent.

4.  `source ~/.bashrc`

      * This command "sources" or reloads the `.bashrc` file. Since the default `.bashrc` file is configured to also load the `.bash_aliases` file, this command makes your newly added permanent alias available for use immediately in your current terminal session without needing to log out and log back in.

---

# 📦 **Built-in Shell Variables**

The shell has a set of standard, pre-defined variables that are always available. Here is a short list of some of them:

  * 🏠 `HOME`: The user’s home directory (for example, `/home/hashim`).
  * 👤 `LOGNAME`: The user’s login name (for example, `hashim`).
  * 📂 `PWD`: The shell’s **P**resent **W**orking **D**irectory.
  * ⏪ `OLDPWD`: The shell’s previous working directory.
  * 🗺️ `PATH`: The shell’s search path, which is a list of directories separated by colons (`:`) where the shell looks for commands.
  * 🐚 `SHELL`: The path to the shell program itself (e.g., `/bin/bash`).
  * 🙋 `USER`: The user’s login name (often the same as `LOGNAME`).
  * 💻 `TERM`: The type of the Terminal being used (e.g., `xterm-256color`).

To call a variable while in the shell, all you have to do is place a dollar sign (`$`) in front of the variable’s name.

#### Code Snippet

Here is an example showing how to display the values of the variables we just listed using the `echo` command.

```bash
hashim@Hashim:~$ echo $HOME
/home/hashim

hashim@Hashim:~$ echo $LOGNAME
hashim

hashim@Hashim:~$ echo $PWD
/home/hashim

hashim@Hashim:~$ echo $OLDPWD


hashim@Hashim:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin

hashim@Hashim:~$ echo $USER
hashim

hashim@Hashim:~$ echo $TERM
xterm-256color
```

-----

### ✍️ Creating Your Own Shell Variables

You can also assign your own shell variables. In the following example, we’re assigning the string `sysadmin` to a new variable called `MYVAR` and then printing it to the standard output.

The variables listed at the beginning are just a small part of all the variables available. To see **all** the shell variables, you can use the `printenv` command. If the list is too long, you can redirect it to a file. In this example, the list of variables is saved inside the `shell_variables` file.

#### Code Snippet

```bash
hashim@Hashim:~$ MYVAR=sysadmin; echo $MYVAR
sysadmin

hashim@Hashim:~$ printenv > ~/shell_variables

hashim@Hashim:~$ ls
Desktop    Downloads  Music      Public           snap       Videos
Documents  lvm        Pictures   shell_variables  Templates

hashim@Hashim:~$ less shell_variables
```

#### Explanation

1.  `MYVAR=sysadmin; echo $MYVAR`: First, we create a new variable named `MYVAR` and give it the value `sysadmin`. The semicolon (`;`) allows us to run a second command, `echo $MYVAR`, on the same line, which prints the value of our new variable.
2.  `printenv > ~/shell_variables`: The `printenv` command lists all environment variables. The `>` symbol redirects this output and saves it into a file named `shell_variables`. The tilde symbol (`~`) is a shortcut for the current user's home directory.
3.  `ls`: This lists the files, confirming that `shell_variables` was created.
4.  `less shell_variables`: This command opens the new file in the `less` pager so you can view its contents.

-----

### 🌍 Shell vs. Environment Variables

The shell’s variables are **only available inside the current shell**. If you want variables to be known to other programs that are run *by* the shell (like scripts or new shells), you must **export** them using the `export` command. Once a variable is exported from the shell, it becomes an **environment variable**.

### 🛠️ Practical Example: Exporting a Variable

This example shows the difference between a regular shell variable and an exported environment variable.

#### Code Snippet

```bash
hashim@Hashim:~$ ls ~
Desktop    Downloads  Music     Public    Templates
Documents  lvm        Pictures  snap      Videos

hashim@Hashim:~$ MY_MESSAGE="Hello from Shell"; echo $MY_MESSAGE
Hello from Shell

hashim@Hashim:~$ bash

hashim@Hashim:~$ echo $MY_MESSAGE


hashim@Hashim:~$ exit
exit

hashim@Hashim:~$ export MY_MESSAGE

hashim@Hashim:~$ bash

hashim@Hashim:~$ echo $MY_MESSAGE
Hello from Shell
```

#### Step-by-Step Explanation

1.  `ls ~`: This is just a demonstration of the `~` shortcut, listing the contents of the home directory.
2.  `MY_MESSAGE="Hello from Shell"; echo $MY_MESSAGE`: We create a new **shell variable** called `MY_MESSAGE`. The `echo` command confirms it was set correctly.
3.  `bash`: We start a new `bash` session. This new session is a "child" of the original one.
4.  `echo $MY_MESSAGE`: We try to print the variable in the new shell. **Nothing is printed**. This is because `MY_MESSAGE` was only a local shell variable and was not passed down to the child shell.
5.  `exit`: We exit the child shell and return to our original shell.
6.  `export MY_MESSAGE`: Now, we use the `export` command. This turns `MY_MESSAGE` from a simple shell variable into an **environment variable**.
7.  `bash`: We start another new child shell.
8.  `echo $MY_MESSAGE`: We try printing the variable again. This time, it works and prints "Hello from Shell" because the exported variable was inherited by the new child shell.

---

# 🗺️ **The Shell's Search Path**

The **`PATH`** variable is essential in Linux. It helps the shell know where all the programs and commands are located. When you enter a command into your Bash shell, it has to search for that command through the Linux filesystem. The `PATH` variable contains a list of directories that the shell will look through.

You can add new directories to this list. Your addition can be **temporary** (for the current session) or **permanent**.

-----

### ⏳ Adding a Directory Temporarily

To make a directory’s path available temporarily, you simply add it to the `PATH` variable. In the following example, we’re adding the `/home/hashim` directory to `PATH`.

#### Code Snippet

```bash
hashim@Hashim:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin

hashim@Hashim:~$ PATH=$PATH:/home/hashim/

hashim@Hashim:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin:/home/hashim/
```

#### Explanation

1.  `echo $PATH`: We first print the current `PATH` to see the list of directories that are already included. Each directory is separated by a colon (`:`).
2.  `PATH=$PATH:/home/hashim/`: This is the command that does the work.
      * `PATH=...`: We are assigning a new value to the `PATH` variable.
      * `$PATH`: We start the new value by including the entire existing `$PATH`. This is crucial so we don't lose access to all the standard commands.
      * `:`: We add a colon to separate the existing path list from our new directory.
      * `/home/hashim/`: This is the new directory we are appending to the list.
3.  `echo $PATH`: We print the `PATH` again to confirm that our new directory, `/home/hashim/`, has been successfully added to the end of the list.

-----

### 💾 Making Changes Permanent

To make any changes to the `PATH` permanent, you must modify a configuration file, such as `~/.bash_profile` or `~/.bashrc`. By adding the `PATH` modification command to one of these files, it will be executed automatically every time you start a new shell session.

> #### 📝 Important Note
>
> Some Linux distributions, like openSUSE, add an extra `bin` directory inside the user’s home directory (e.g., `/home/hashim/bin`). This is a convenient place where you can put files that you want the shell to execute from anywhere, for example, your own script files. The shell's `$PATH` variable is very important, especially when using scripts, as you will want to create scripts inside a directory that is known by the shell.

#### Steps for a Permanent Change

Here is the typical process for making a change to your `PATH` permanent.

```bash
hashim@Hashim:~$ PATH=$PATH:/home/hashim/

hashim@Hashim:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin:/home/hashim/

hashim@Hashim:~$ nano ~/.bashrc

hashim@Hashim:~$ source ~/.bashrc
```

#### Step-by-Step Explanation

1.  `PATH=$PATH:/home/hashim/` and `echo $PATH`: First, you test the change temporarily in your current session to make sure it works as expected.
2.  `nano ~/.bashrc`: This command opens the `.bashrc` configuration file in the `nano` text editor. Inside this file, you would scroll to the bottom and add the following line:
    ```bash
    export PATH="$PATH:/home/hashim/"
    ```
    (Using `export` is best practice for modifying `PATH` in configuration files). You would then save the file and exit the editor.
3.  `source ~/.bashrc`: This command reloads the `.bashrc` file. This applies the changes to your current terminal session immediately, so you don't have to log out and log back in to activate the new permanent `PATH`.

---  

