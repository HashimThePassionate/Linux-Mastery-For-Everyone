# 🐧 **UNIX Versus LINUX**

**Unix** is an operating system 🖥️ created by Ken Thompson in the early 1970s. Today, several variants are available, such as **HP/UX** for HP machines and **AIX** for IBM machines.

Linus Torvalds developed the **Linux** operating system during the 1990s. Many Linux commands are the same as their `bash` counterparts. However, differences do exist, often in the commands intended for system administrators. The **Mac OS X** operating system is based on AT&T Unix.

Unix possesses a rich and storied history. If you are interested in learning about its past, you can read online articles and Wikipedia entries. This section will forego those historical details. Instead, it focuses on helping you quickly learn how to become productive with various commands. 🚀

## 🐚 Available Shell Types
The original Unix shell is the **Bourne shell**, written in the mid-1970s by Stephen R. Bourne. The Bourne shell was the first shell to appear on `bash` systems. You will sometimes hear the term "the shell" used as a reference to the Bourne shell.

The Bourne shell is a **POSIX-standard** shell. It is usually installed as `/bin/sh` on most versions of Unix. Its default prompt is the `$` character. Consequently, Bourne shell scripts will execute on almost every version of Unix, ensuring high portability.

In essence, the AT&T branches of Unix support the following shells:

* **Bourne shell (sh)**
* **bash**
* **Korn shell (ksh)**
* **tsh**
* **zsh**


### The C Shell Branch

However, the **BSD branch** of Unix uses the **"C" shell (csh)**. The default prompt for this shell is the `%` character.

In general, shell scripts written for `csh` will not execute on AT&T branches of Unix, unless the `csh` shell is also installed on those machines (and vice versa). ⚠️


### Shell Categories

The Bourne shell is considered the most "unadorned," as it lacks some commands available in other shells, such as `history` and `noclobber`.

The various subcategories for the **Bourne Shell** are as follows:

* Bourne shell (sh)
* Korn shell (ksh)
* Bourne Again shell (bash)
* POSIX shell (sh)
* zsh (“Zee shell”)

The different **C-type shells** are as follows:

* C shell (csh)
* TENEX/TOPS C shell (tcsh)


### 📖 Note on This section's Examples

While the commands and shell scripts in this section are based on the `bash` shell, many of the commands also work in other shells. If not, those other shells typically have a similar command to accomplish the same goal. 💡

Performing an Internet search for "how do I do \<X\> in \<Y\>" will often get you an answer.

Sometimes the command is essentially the same but with slightly different syntax. Typing `man <command_name>` in a command shell can also provide useful information.

---

# ❓ **What is Bash**?

Bash is an acronym for **“Bourne Again Shell.”** It has its roots in the **Bourne shell**, which was created by Stephen R. Bourne.

Shell scripts that are based on the Bourne shell will execute correctly in bash. However, the reverse is not always true. The bash shell provides additional features that are unavailable in the Bourne shell, such as support for **arrays** (which will be discussed later in this section).

-----

## Shells on Mac OS X

On Mac OS X, the `/bin` directory contains the following executable shells:

```bash
-r-xr-xr-x 1 root wheel 1377872 Apr 28 2017 /bin/ksh
-r-xr-xr-x 1 root wheel 630464 Apr 28 2017 /bin/sh
-rwxr-xr-x 1 root wheel 375632 Apr 28 2017 /bin/csh
-rwxr-xr-x 1 root wheel 592656 Apr 28 2017 /bin/zsh
-r-xr-xr-x 1 root wheel 626272 Apr 28 2017 /bin/bash
```

A comparison matrix detailing the support for various features among these shells is available online:
[https://stackoverflow.com/questions/5725296/difference-between-sh-and-bash](https://stackoverflow.com/questions/5725296/difference-between-sh-and-bash)


## Checking Your Shell Version

In some environments, the Bourne shell (`sh`) is actually the bash shell. You can check this by typing the following command:

```bash
sh --version
# or
bash --version
```

The output of the preceding command might be as follows:

```bash
GNU bash, version 5.2.37(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2022 Free Software Foundation, Inc.
```

> 💡 **Note:** If you are new to the command line (whether on Mac, Linux, or PCs), please read the Preface, which provides some useful guidelines for accessing command shells.

## 🆘 Getting Help for bash Commands

If you want to see the options for a specific bash command, invoke the `man` command. This will show you a description of that bash command and its available options:

```bash
man cat
```

The `man` command produces terse (brief) explanations. If those explanations are not clear enough, you can search for online code samples that provide more details.

-----

## 🧭 Navigating Around Directories

In a command shell, you will often perform basic operations. These include displaying (or changing) the current directory, listing the contents of a directory, and displaying the contents of a file.

The following commands show you how to perform these operations. You can execute a subset of these commands in the sequence that is relevant to your needs. Options for some of the commands in this section (such as the `ls` command) are described in greater detail later in this section.

### 📂 `pwd` (Print Working Directory)

A frequently used bash command is `pwd`. It displays the current directory, as shown here:

```bash
pwd
```

The output of the preceding command might look something like this:

```bash
/home/hashim/Repo
```

### ➡️ `cd` (Change Directory)

Use the `cd` command to go to a specific directory.

  * **Absolute Path:** To navigate to the `/home/hashim/info/documents` directory, type the command:
```bash
cd /home/hashim/info/documents
```
  * **Relative Path:** If you are currently in the `/home/hashim` directory, you can just type:
```bash
hashim@Hashim:~$ pwd
/home/hashim

cd info/

hashim@Hashim:~/info$ pwd
/home/hashim/info
```

You can navigate to your home directory with either of these commands:

```bash
hashim@Hashim:~/info/documents$ pwd
/home/hashim/info/documents

hashim@Hashim:~/info/documents$ cd $HOME
hashim@Hashim:~$ pwd
/home/hashim


hashim@Hashim:~/info/documents$ cd ../../
hashim@Hashim:~$ pwd
/home/hashim
```

One convenient way to return to the previous directory is the command:

```bash
hashim@Hashim:~$ cd -
/home/hashim/info/documents
```

> **Note on Windows:** The `cd` command on Windows merely displays the current directory and does not change the current directory (unlike the Unix `cd` command).


## 📜 The `history` Command

The `history` command displays a list (i.e., the history) of commands that you executed in the current command shell, as shown here:

```bash
hashim@Hashim:~$ history
```

A sample output of the preceding command is here:

```bash
212  rm break_loop.sh 
213  rm break_loop_2.sh 
214  clear
215  ls
216  cd #
217 l
218  cd ~
219  ls
220  clear
221  mkdir Repo
222  ls
223  cd Repo/
224  clear
225  history
```

### Re-executing Commands

If you want to navigate to the directory that is shown in line 223, you can do so simply by typing the following command:

```bash
hashim@Hashim:~$ !223


cd Repo/
hashim@Hashim:~/Repo$ pwd
/home/hashim/Repo
```
> ⚠️ **Warning:** Be careful with the `!` option with bash commands. The command that matches the `!` might not be the one you intended. It is safer to use the `history` command first and then explicitly specify the correct number (from that history) when you invoke the `!` operator.

-----

## 📋 Listing Filenames with the `ls` Command

The `ls` command is for listing filenames, and there are many switches available that you can use, as shown in this section.

### Default `ls`

For example, the `ls` command displays the following filenames (the actual display depends on the font size and the width of the command shell) on my Ubuntu:

```bash
hashim@Hashim:~/Repo$ ls
apple-care.text       iphonemeetup.txt  outfile.txt  ssl-instructions.txt
checkin-commands.txt  kyrgyzstan.txt    output.txt
```

### `ls -1` (Vertical Listing)

The command `ls -1` (the digit “1”) displays a vertical listing of filenames:

```bash
hashim@Hashim:~/Repo$ ls -1
apple-care.text
checkin-commands.txt
iphonemeetup.txt
kyrgyzstan.txt
outfile.txt
output.txt
ssl-instructions.txt
```

### `ls -l` (Long Listing)

The command `ls -l` (the letter “l”) displays a long listing of filenames:

```bash
hashim@Hashim:~/Repo$ ls -l
total 28
-rw-rw-r-- 1 hashim hashim 32 Oct 25 15:48 apple-care.text
-rw-rw-r-- 1 hashim hashim 34 Oct 25 15:48 checkin-commands.txt
-rw-rw-r-- 1 hashim hashim 20 Oct 25 15:49 hello.txt
-rw-rw-r-- 1 hashim hashim 32 Oct 25 15:49 iphonemeetup.txt
-rw-rw-r-- 1 hashim hashim 26 Oct 25 15:49 outfile.txt
-rw-rw-r-- 1 hashim hashim 30 Oct 25 15:50 output.txt
-rw-rw-r-- 1 hashim hashim 94 Oct 25 15:50 ssl-instructions.txt
```

### `ls -lt` (Time-based Long Listing)

The command `ls -lt` (the letters “l” and “t”) displays a time-based long listing:

```bash
hashim@Hashim:~/Repo$ ls -lt
total 28
-rw-rw-r-- 1 hashim hashim 94 Oct 25 15:50 ssl-instructions.txt
-rw-rw-r-- 1 hashim hashim 30 Oct 25 15:50 output.txt
-rw-rw-r-- 1 hashim hashim 26 Oct 25 15:49 outfile.txt
-rw-rw-r-- 1 hashim hashim 20 Oct 25 15:49 hello.txt
-rw-rw-r-- 1 hashim hashim 32 Oct 25 15:49 iphonemeetup.txt
-rw-rw-r-- 1 hashim hashim 34 Oct 25 15:48 checkin-commands.txt
-rw-rw-r-- 1 hashim hashim 32 Oct 25 15:48 apple-care.text
```

### `ls -ltr` (Reversed Time-based Long Listing)

The command `ls -ltr` (the letters “l,” “t,” and “r”) displays a reversed time-based long listing of filenames:

```bash
hashim@Hashim:~/Repo$ ls -ltr
total 28
-rw-rw-r-- 1 hashim hashim 32 Oct 25 15:48 apple-care.text
-rw-rw-r-- 1 hashim hashim 34 Oct 25 15:48 checkin-commands.txt
-rw-rw-r-- 1 hashim hashim 32 Oct 25 15:49 iphonemeetup.txt
-rw-rw-r-- 1 hashim hashim 20 Oct 25 15:49 hello.txt
-rw-rw-r-- 1 hashim hashim 26 Oct 25 15:49 outfile.txt
-rw-rw-r-- 1 hashim hashim 30 Oct 25 15:50 output.txt
-rw-rw-r-- 1 hashim hashim 94 Oct 25 15:50 ssl-instructions.txt
```

### 🔍 Understanding the Long Listing (`ls -l`) Columns

Here is the description of all the listed columns in the preceding output:

1.  **Column \#1:** Represents file type and permission given on the file (see below).
2.  **Column \#2:** The number of memory blocks taken by the file or directory.
3.  **Column \#3:** The (bash user) owner of the file.
4.  **Column \#4:** Represents the group of the owner.
5.  **Column \#5:** Represents the file size in bytes.
6.  **Column \#6:** The date and time when this file was created or last modified.
7.  **Column \#7:** Represents the file or directory name.

### 🚩 File Type Indicators

In the `ls -l` listing example, every file line begins with `d`, `-`, or `l`. These characters indicate the type of file that is listed. These (and other) initial values are described as follows:

  * **`-`**: Regular file (ASCII text file, binary executable, or hard link)
  * **`b`**: Block special file (such as a physical hard drive)
  * **`c`**: Character special file (such as a physical hard drive)
  * **`d`**: Directory file that contains a listing of other files and directories
  * **`l`**: Symbolic link file
  * **`p`**: Named pipe (a mechanism for inter-process communications)
  * **`s`**: Socket (for inter-process communication)

> ℹ️ Consult online documentation for more details regarding the `ls` command.

---