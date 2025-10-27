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

# 📄 **Displaying the Contents of Files**

This section introduces several commands for displaying different lines of text in a text file. Before doing so, let’s invoke the `wc` (word count) command to display the number of lines, words, and characters in a text file, as shown here:

```bash
hashim@Hashim:~/Repo$ wc longfile.txt 
  50  257 1364 longfile.txt
```

#### 📜 Code Explanation

  * `wc`: This is the "word count" command. It is used to count lines, words, and characters in a file.
  * `longfile.txt`: This is the target file that `wc` is analyzing.
  * **Output:**
      * `50`: The number of lines in the file.
      * `257`: The number of words in the file.
      * `1364`: The number of characters (bytes) in the file.
      * `longfile.txt`: The name of the file is repeated at the end.

The preceding output shows that the file `longfile.txt` contains 50 lines, 257 words, and 1364 characters. This means that the file size is actually quite small (despite its name).

-----

## 🐱 The `cat` Command

Invoke the `cat` command to display the contents of `longfile.txt`:

```bash
cat longfile.txt
```

#### 📜 Code Explanation

  * `cat`: Short for "catenate," this command reads the contents of one or more files and prints them to standard output (your terminal screen).
  * `longfile.txt`: The specific file to be read and displayed.

The preceding command displays the following text:

```bash
hashim@Hashim:~/Repo$ cat longfile.txt 
Name: Muhammad Hashim line number 1
Profession: Devops Engineer line 2
This is line number 3.
Here is some sample text for line 4.
Each line in this file is unique.
Adding content to line 6.
This is the seventh line.
We are filling up the file.
(other lines are omitted)
```

The preceding command displays the entire contents of a file. However, there are several commands that display only a portion of a file, such as `less`, `more`, `page`, `head`, and `tail` (all of which are discussed later).

As another example, suppose that the file `temp1` has the following contents:

```
this is line1 of temp1
this is line2 of temp1
this is line3 of temp1
```

Let’s also suppose that the file `temp2` has these contents:

```
this is line1 of temp2
this is line2 of temp2
```

Now type the following command that contains the `?` meta character (discussed in detail later in this Section):

```bash
hashim@Hashim:~/Repo$ cat temp?
```

#### 📜 Code Explanation

  * `cat`: The "catenate" command.
  * `temp?`: This uses a wildcard (or "meta character"). The `?` matches **any single character**. Therefore, the shell expands `temp?` to match both `temp1` and `temp2`. The `cat` command then receives both filenames and prints their contents sequentially.

The output from the preceding command is shown here:

```
this is line1 of temp1
this is line2 of temp1
this is line3 of temp1
this is line1 of temp2
this is line2 of temp2
```

-----

## ⬆️ The `head` and `tail` Commands

### `head`

The `head` command displays the first ten lines of a text file (by default). An example of this is here:

```bash
hashim@Hashim:~/Repo$ head longfile.txt
```

#### 📜 Code Explanation

  * `head`: This command reads a file and prints the first part of it (the "head") to standard output.
  * `longfile.txt`: The file to read from.
  * **Behavior:** By default, `head` prints the first **10 lines** of the specified file.

The preceding command displays the following text:

```bash
hashim@Hashim:~/Repo$ head longfile.txt 
Name: Muhammad Hashim line number 1
Profession: Devops Engineer line 2
This is line number 3.
Here is some sample text for line 4.
Each line in this file is unique.
Adding content to line 6.
This is the seventh line.
We are filling up the file.
Line 9 is now complete.
Tenth line of the document.
```

The `head` command also provides an option to specify a different number of lines to display, as shown here:

```bash
head -4 longfile.txt
```

#### 📜 Code Explanation

  * `head`: The command to show the beginning of a file.
  * `-4`: This is an option (or "flag") that modifies the command's behavior. Instead of the default 10 lines, it specifies that only the first **4 lines** should be shown.
  * `longfile.txt`: The target file.

The preceding command displays the following text:

```bash
hashim@Hashim:~/Repo$ head -4 longfile.txt
Name: Muhammad Hashim line number 1
Profession: Devops Engineer line 2
This is line number 3.
Here is some sample text for line 4.
```

### `tail`

The `tail` command displays the last 10 lines (by default) of a text file:

```bash
tail longfile.txt
```

#### 📜 Code Explanation

  * `tail`: This command reads a file and prints the last part of it (the "tail") to standard output.
  * `longfile.txt`: The file to read from.
  * **Behavior:** By default, `tail` prints the **last 10 lines** of the specified file.

The preceding command displays the following text:

```bash
hashim@Hashim:~/Repo$ tail  longfile.txt 
Line 41 is now present.
Here is the text for line 42.
This is line 43.
Line 44 is complete.
Adding the forty-fifth line.
This is line 46.
Line 47 with new content.
Almost done, this is line 48.
The second to last line, number 49.
This is the final line, number 50.
```

Similarly, the `tail` command allows you to specify a different number of lines to display. For example, `tail –4 longfile.txt` displays the last four lines of `longfile.txt`.

### `more` and `less`

Use the `more` command to display a screenful of data, as shown here:

```bash
more longfile.txt
```

#### 📜 Code Explanation

  * `more`: This is a "pager" command. It displays the contents of a file one screen (or "page") at a time, making it easy to read large files.
  * `longfile.txt`: The file to be displayed.

Press the **`<spacebar>`** to view the next screenful of data and press the **`<enter>`** key to see the next line of text in a file. Incidentally, some people prefer the `less` command, which generates essentially the same output as the `more` command (but allows backward navigation, making it more powerful).

-----

## ቧ The Pipe Symbol

A very useful feature of bash is its support for the **pipe symbol (`|`)**. This enables you to "pipe" or redirect the output of one command to become the input of another command. The pipe command is useful when you want to perform a sequence of operations involving various bash commands.

For example, the following code snippet combines the `head` command with the `cat` command and the pipe (`|`) symbol:

```bash
cat longfile.txt | head -2
```

#### 📜 Code Explanation

This command involves two processes linked by a pipe:

1.  `cat longfile.txt`: First, the `cat` command reads the *entire* `longfile.txt` and writes its full content to standard output.
2.  `|`: The pipe symbol intercepts the standard output from `cat`. Instead of letting it go to the screen, it **redirects** that output to become the standard **input** for the next command.
3.  `head -2`: The `head` command receives the full file content from the pipe as its input. It then processes this input and prints only the first 2 lines.

The preceding command displays the following text:

```bash
hashim@Hashim:~/Repo$ cat longfile.txt | head -2
Name: Muhammad Hashim line number 1
Profession: Devops Engineer line 2
```

A technical point: the preceding command creates **two bash processes** (one for `cat`, one for `head`), whereas the command `head -2 longfile.txt` only creates a **single bash process** (just `head`).

You can use the `head` and `tail` commands in more interesting ways. For example, the following command sequence displays lines 11 through 15 of `longfile.txt`:

```bash
head -15 longfile.txt | tail -5
```

#### 📜 Code Explanation

This is another two-step piped command:

1.  `head -15 longfile.txt`: This command reads `longfile.txt` and prints its **first 15 lines** to standard output.
2.  `|`: The pipe intercepts those 15 lines and sends them as standard **input** to the `tail` command.
3.  `tail -5`: The `tail` command receives the 15 lines from the pipe. It then processes this input and prints the **last 5 lines** *of that input*. This effectively isolates lines 11, 12, 13, 14, and 15.

The preceding command displays the following text:

```bash
hashim@Hashim:~/Repo$ head -15 longfile.txt | tail -5
Line 11 has been added.
This is placeholder text for line 12.
Continuing with line 13.
Fourteenth line incoming.
We are at line 15.
```

Display the line numbers for the preceding output as follows:

```bash
cat -n longfile.txt | head -15 | tail -5
```

#### 📜 Code Explanation

This is a three-step pipe chain:

1.  `cat -n longfile.txt`: The `cat` command reads the file. The `-n` flag adds line numbers to each line and sends the *numbered* output to the pipe.
2.  `| head -15`: The `head` command receives the numbered lines, takes the **first 15** of them, and sends *those* 15 lines to the next pipe.
3.  `| tail -5`: The `tail` command receives the 15 numbered lines and prints the **last 5** of them.

The preceding command displays the following text:

```bash
hashim@Hashim:~/Repo$ cat -n longfile.txt | head -15 | tail -5
    11	Line 11 has been added.
    12	This is placeholder text for line 12.
    13	Continuing with line 13.
    14	Fourteenth line incoming.
    15	We are at line 15.
```

You will not see the "tab" character from the output, but it is visible if you redirect the previous command sequence to a file and then use the `-t` option with the `cat` command:

```bash
hashim@Hashim:~/Repo$ cat -n longfile.txt | head -15 | tail -5 > 1
hashim@Hashim:~/Repo$ cat -t 1
```

#### 📜 Code Explanation

This example involves two separate commands:

**Command 1:**

  * `cat -n longfile.txt | head -15 | tail -5`: This is the same pipe chain as before, producing 5 lines of numbered text.
  * `> 1`: This is **output redirection**. Instead of printing the 5 lines to the screen (standard output), the `>` symbol tells the shell to **write** that output into a new file named `1`.

**Command 2:**

  * `cat -t 1`: This reads the contents of the newly created file `1`.
  * `-t`: This flag tells `cat` to display non-printing characters. Specifically, it makes **tab characters** visible, representing them as `^I`.

The preceding command displays the following text (where `^I` represents the tab character):

```bash
hashim@Hashim:~/Repo$ cat -n longfile.txt | head -15 | tail -5 > 1
hashim@Hashim:~/Repo$ cat -t 1
    11^ILine 11 has been added.
    12^IThis is placeholder text for line 12.
    13^IContinuing with line 13.
    14^IFourteenth line incoming.
    15^IWe are at line 15.
```

-----

## ✂️ The `fold` Command

The `fold` command enables you to "fold" the lines in a text file, which is useful for text files that contain long lines of text that you want to split into shorter lines.

For example, here are the contents of `longfile2.txt`:

```
the contents of this long file are too long to see in a
single screen and each line contains one or more words and
if you use the cat command the file contents scroll off the
screen so you can use other commands such as the head or
tail or more commands in conjunction with the pipe command
that is very useful in Bash and is available in every shell
including the bash shell csh zsh ksh and Bourne shell
```

You can "fold" the contents of `longfile2.txt` into lines whose length is 45 (just as an example) with this command:

```bash
cat longfile2.txt | fold -45
```

#### 📜 Code Explanation

  * `cat longfile2.txt`: Reads the content of `longfile2.txt` and sends it to standard output.
  * `|`: The pipe redirects this content to become the standard input for the `fold` command.
  * `fold -45`: The `fold` command reads the text from the pipe. The `-45` flag specifies a maximum line width of **45 characters**. `fold` wraps the text, inserting a newline character at (or before) the 45th column.

The output of the preceding command is here:

```bash
hashim@Hashim:~/Repo$ cat longfile2.txt | fold -45
the contents of this long file are too long t
o see in a
single screen and each line contains one or m
ore words and
if you use the cat command the file contents 
scroll off the
screen so you can use other commands such as 
the head or
tail or more commands in conjunction with the
 pipe command
that is very useful in Bash and is available 
in every shell
including the bash shell csh zsh ksh and Bour
ne shell
```

Notice that some words in the preceding output are split based on the line width, and not "newspaper style."

In Section 4, you will learn how to display the lines in a text file that match a string or a pattern, and in Section 5 you will learn how to replace a string with another string in a text file.

---

# 🔐 File Ownership: Owner, Group, and World

Bash files can have partial or full **rwx** privileges. These privileges define what can be done with a file:

  * **r (read):** The file's contents can be viewed.
  * **w (write):** The file's contents can be changed or modified.
  * **x (execute):** The file can be run as a program from the command line. This is done by typing the file’s name (or its full path if the file is not in your current directory). Invoking an executable file this way causes the operating system to try to execute the commands inside it.

These permissions are set for three distinct levels of ownership:

1.  **u (User):** The owner of the file (usually the person who created it).
2.  **g (Group):** A specific group of users who share permissions.
3.  **o (Other):** Everyone else (also referred to as "world").

-----

## 🛠️ Using the `chmod` Command

You use the `chmod` (change mode) command to set permissions for files.

### Setting Absolute Symbolic Permissions

You can set all permissions at once using `u`, `g`, and `o` with the `=` operator.

For example, to set the permissions `rwx rw- r--` for a file:

```bash
chmod u=rwx g=rw o=r filename
```

  * `u=rwx`: The **user** (owner) gets read, write, and execute permissions.
  * `g=rw`: The **group** gets read and write permissions (but not execute).
  * `o=r`: **Others** (everyone else) get only read permission.

### Adding or Removing Permissions

You can modify existing permissions using `+` (to add) or `-` (to remove).

  * **Add a permission:** To add the execute (`x`) permission for "others" to a file that is `rwx rw- r--`:

    ```bash
    chmod o+x filename
    ```

    The new permissions would be `rwx rw- r-x`.

  * **Add to all:** You can use `a` (for "all") to add a permission to all three categories (user, group, and other) at once.

    ```bash
    chmod a+x filename
    ```

  * **Remove from all:** Conversely, you can use `a-` to remove a permission from all groups.

    ```bash
    chmod a-x filename
    ```

-----

## 🚀 Practical Examples from Scratch

Let's walk through these commands with a real file.

1.  **Create a file:**
    First, let's create a new file named `test_script.sh`.

    ```bash
    hashim@Hashim:~$ touch test_script.sh
    ```

2.  **Check default permissions:**
    Now, let's see its default permissions using `ls -l`.

    ```bash
    hashim@Hashim:~$ ls -l test_script.sh
    -rw-rw-r-- 1 hashim hashim 0 Oct 25 17:30 test_script.sh
    ```

    As you can see, the default permissions are `rw-rw-r--`. The user (`hashim`) can read/write, the group (`hashim`) can read/write, and others can only read. No one can execute it.

3.  **Set specific permissions (Absolute):**
    Let's set the permissions to `rwx` for the user, `rw-` for the group, and `r--` for others, as in the first example.

    ```bash
    hashim@Hashim:~$ chmod u=rwx,g=rw,o=r test_script.sh
    hashim@Hashim:~$ ls -l test_script.sh
    -rwxrw-r-- 1 hashim hashim 0 Oct 25 17:31 test_script.sh
    ```

    The permissions have been updated exactly as specified.

4.  **Add 'execute' for 'others':**
    Now, let's add the execute (`x`) permission for the "others" category.

    ```bash
    hashim@Hashim:~$ chmod o+x test_script.sh
    hashim@Hashim:~$ ls -l test_script.sh
    -rwxrw-r-x 1 hashim hashim 0 Oct 25 17:32 test_script.sh
    ```

    Notice the permissions for "others" changed from `r--` to `r-x`.

5.  **Add 'execute' for all (if missing):**
    The user and group already have write permissions, but let's imagine we wanted to ensure *everyone* has execute permission. We can use `a+x`.

    ```bash
    hashim@Hashim:~$ chmod a+x test_script.sh
    hashim@Hashim:~$ ls -l test_script.sh
    -rwxrwxr-x 1 hashim hashim 0 Oct 25 17:33 test_script.sh
    ```

    The `x` permission was added to the "group" category (which was `rw-` and became `rwx`). The user and other categories already had `x`, so they remained unchanged.

6.  **Remove 'execute' from all:**
    Finally, let's remove the execute permission from *everyone* at the same time.

    ```bash
    hashim@Hashim:~$ chmod a-x test_script.sh
    hashim@Hashim:~$ ls -l test_script.sh
    -rw-rw-r-- 1 hashim hashim 0 Oct 25 17:34 test_script.sh
    ```

    The file is no longer executable by anyone, returning it to its original permission state.

---

# 📂 **Hidden File Management Concepts**

## Ẩ Hidden Files

An invisible file, more commonly known as a **hidden file**, is any file whose name begins with a dot or period character (`.`).

Bash programs, including the shell itself, use most of these files to store configuration information. By default, they are hidden from a normal `ls` command to avoid cluttering the directory.

Some common examples of hidden files include:

  * **`.profile`**: The initialization script for the Bourne shell (`sh`).
  * **`.bash_profile`**: The initialization script for the bash shell (`bash`).
  * **`.kshrc`**: The initialization script for the Korn shell (`ksh`).
  * **`.cshrc`**: The initialization script for the C shell (`csh`).
  * **`.rhosts`**: The remote shell configuration file.

### Listing Hidden Files

To list invisible files, you must specify the `-1a` option (which stands for "all") for the `ls` command.

```bash
hashim@Hashim:~$ ls -1a
.
..
.array_loop.sh.swp
.bash_aliases
.bash_history
.bash_logout
.bashrc
.cache
.config
Desktop
Documents
Downloads
info
.local
.profile
.ssh
```

#### 📜 Code Explanation

  * `ls -1a`: This command lists **all** files and directories within the current directory in vertically.
      * `ls`: The command to "list" directory contents.
      * `-1a`: The "all" option, which instructs `ls` to include entries that start with a `.` (dot).

### Special Directory Entries

In the output above, you will see two special entries:

  * **Single dot `.`**: This represents the **current directory** itself.
  * **Double dot `..`**: This represents the **parent directory** (the directory one level up).

## ⚠️ Handling Problematic Filenames

Problematic filenames are those that contain one or more whitespaces, hidden (non-printing) characters, or start with a dash (`-`) character.

### Files with Whitespaces

You can use **double quotes** to list filenames that contain whitespaces, or you can precede each whitespace with a **backslash (`\`)** character, which acts as an "escape" character.

For example, if you have a file named `One Space.txt`, you can use the `ls` command as follows.

First, a normal `ls` shows the file exists (note the single quotes the `ls` command adds to its own output to show the name):

```bash
hashim@Hashim:~/Repo$ ls
 apple-care.text         longfile2.txt    output.txt           temp2
 checkin-commands.txt    longfile.txt     poem                 test_script.sh
 hello.txt               One              python.py
 instructions            'One Space.txt'  ssl-instructions.txt
 iphonemeetup.txt        outfile.txt      temp1
```

**Method 1: Using Double Quotes**

```bash
hashim@Hashim:~/Repo$ ls -l "One Space.txt"
-rw-rw-r-- 1 hashim hashim 0 Oct 25 18:01 'One Space.txt'
```

#### 📜 Code Explanation

  * `ls -l "One Space.txt"`: By enclosing `"One Space.txt"` in double quotes, you are telling the shell to treat the entire string as a **single argument** (a single filename), including the space in the middle.

**Method 2: Using the Backslash (Escape Character)**

```bash
hashim@Hashim:~/Repo$ ls -l One\ Space.txt
-rw-rw-r-- 1 hashim hashim 0 Oct 25 18:01 'One Space.txt'
```

#### 📜 Code Explanation

  * `ls -l One\ Space.txt`: The backslash (`\`) tells the shell that the *very next character* should be treated literally and not as a special character. In this case, it "escapes" the space, preventing the shell from splitting `One` and `Space.txt` into two separate arguments.


### Files Starting with a Dash (`-`)

Filenames that start with a dash (`-`) character are difficult to handle. This is because the dash character is the prefix that specifies **options** for bash commands.

Consequently, if you have a file whose name is `–abc`, then the command `ls –abc` will not work correctly. The shell interprets `–abc` as an option for the `ls` command (and in this case, `ls` does not have an `abc` option).

In most cases, the best solution to this type of file is to **rename the file**. This can be done in your operating system's graphical interface, or you can use the following special syntax for the `mv` ("move") command to rename the file.

```bash
mv -- -abc.txt renamed-abc.txt
```

#### 📜 Code Explanation

  * `mv`: The command to "move" or rename a file.
  * `--`: This is a special, universal argument. It tells the command to **stop parsing for options**. After this point, every argument that follows is treated as a literal filename, even if it starts with a dash.
  * `-abc.txt`: Because this comes *after* the `--`, the `mv` command correctly understands it as the name of the source file.
  * `renamed-abc.txt`: This is the new, non-problematic name for the file.

---

# 🌍 **Working with Environment Variables**

There are many built-in environment variables available. The following subsections discuss the `env` command and then some of the more common variables.

## 🖥️ The `env` Command

The `env` ("environment") command displays the variables that are in your bash environment. These variables are key-value pairs that store configuration information for your system and shell session.

An example of the output of the `env` command is here:

```bash
HELL=/bin/bash
SESSION_MANAGER=local/Hashim:@/tmp/.ICE-unix/2702,unix/Hashim:/tmp/.ICE-unix/2702
QT_ACCESSIBILITY=1
COLORTERM=truecolor
XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu-xorg:/etc/xdg
XDG_MENU_PREFIX=gnome-
GNOME_DESOP_SESSION_ID=this-is-deprecated
GNOME_SHELL_SESSION_MODE=ubuntu
SSH_AUTH_SOCK=/run/user/1000/keyring/ssh
MEMORY_PRESSURE_WRITE=c29tZSAyMDAwMDAgMjAwMDAwMAA=
XMODIFIERS=@im=ibus
DESKTOP_SESSION=ubuntu-xorg
GTK_MODULES=gail:atk-bridge
PWD=/home/hashim
LOGNAME=hashim
XDG_SESSION_DESKTOP=ubuntu-xorg
XDG_SESSION_TYPE=x11
GPG_AGENT_INFO=/run/user/1000/gnupg/S.gpg-agent:0:1
SYSTEMD_EXEC_PID=2735
XAUTHORITY=/run/user/1000/gdm/Xauthority
WINDOWPATH=2
HOME=/home/hashim
USERNAME=hashim
LANG=en_US.UTF-8
_=/usr/bin/env
```

### 📜 Output Explanation

This output is a list of all environment variables for the current session. Each line follows a `VARIABLE_NAME=value` format.

  * `HELL=/bin/bash`: This (likely a typo in the source, usually `SHELL`) indicates the path to the current shell.
  * `PWD=/home/hashim`: The **Print Working Directory** (your current location).
  * `LOGNAME=hashim`: The user who is logged in.
  * `HOME=/home/hashim`: The absolute path to the current user's home directory.
  * `LANG=en_US.UTF-8`: The language and character encoding settings.
  * `_=/usr/bin/env`: A special variable that holds the last command that was executed.

## 💡 Useful Environment Variables

This section discusses some important environment variables, most of which you probably will not need to modify. However, it is useful to be aware of the existence of these variables and their purpose.

  * 🏠 **`HOME`**: This variable contains the absolute path of the user’s home directory.
  * 🌐 **`HOSTNAME`**: This variable specifies the Internet name of the host machine.
  * 👤 **`LOGNAME`**: This variable specifies the user’s login name.
  * 🗺️ **`PATH`**: This variable specifies the search path (see the next subsection).
  * 🐚 **`SHELL`**: This variable specifies the absolute path of the current shell (e.g., `/bin/bash`).
  * 🧑‍💻 **`USER`**: This variable specifies the user’s current username. This value might be different from the `LOGNAME` if a superuser (like root) executes the `su` command to emulate another user’s permissions.

## 🔧 Setting the `PATH` Environment Variable

Programs and other executable files can exist in many different directories. Operating systems provide a **search path** that lists the directories where the OS searches for executable files.

You can add a directory to your `PATH` so that you can invoke an executable file by specifying just its filename. You will not need to specify the full path to the executable file.

The search path is stored in an environment variable, which is a named string maintained by the operating system. These variables contain information available to the command shell and other programs.

The path variable is named **`PATH`** in bash or **`Path`** in Windows (bash is case-sensitive, but Windows is not).

### Examples of Setting the `PATH`

**To set the path in bash/Linux (pre-pending a directory):**

```bash
export PATH=$HOME/anaconda:$PATH
```

#### 📜 Code Explanation

  * `export`: This command makes the variable (in this case, `PATH`) available to all child processes and scripts launched from the current shell.
  * `PATH=...`: This is the assignment operation, setting a new value for the `PATH` variable.
  * `$HOME/anaconda`: This is the new directory we want to add. `$HOME` is a variable that resolves to your home directory (e.g., `/home/hashim`).
  * `:`: This is the **delimiter** that separates directories in the `PATH` string.
  * `$PATH`: This accesses the *current* value of the `PATH` variable and appends it after the new directory. This is crucial for **pre-pending** (adding to the front) so you don't lose all your existing command paths.

**To add the Python directory to the path for a particular session in bash (appending):**

```bash
export PATH="$PATH:/usr/local/bin/python"
```

#### 📜 Code Explanation

  * `export PATH=...`: Same as before.
  * `"$PATH:/usr/local/bin/python"`: This time, the existing path (`$PATH`) is listed first, followed by the delimiter (`:`) and the new directory. This **appends** the new path to the end of the list. The double quotes (`"`) are good practice to prevent errors if any paths contain spaces.

**In the Bourne shell (`sh`) or ksh shell, you might see this:**

```bash
PATH="$PATH:/usr/local/bin/python"
```

#### 📜 Code Explanation

  * This is the same assignment as the bash example, but it omits the `export` command. This means the change will apply to the current shell, but it may not be passed down to other scripts or programs that this shell executes.

## 🔬 Practical `PATH` Demonstration

Let's look at how the `PATH` variable changes.

**1. Check the initial `PATH`:**

```bash
hashim@Hashim:~$ echo $PATH | fold -50
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:
/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/
snap/bin:/home/hashim/
```

#### 📜 Code Explanation

  * `echo $PATH`: This command prints the current value stored in the `PATH` variable.
  * `|`: The pipe symbol. It takes the output from the `echo` command and sends it as input to the next command.
  * `fold -50`: The `fold` command wraps the incoming text at a maximum width of 50 characters, making the long `PATH` string easier to read.

**2. Add the `anaconda` directory:**

```bash
hashim@Hashim:~$ export PATH=$PATH:$HOME/anaconda
hashim@Hashim:~$ echo $PATH | fold -50
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:
/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/
snap/bin:/home/hashim/:/home/hashim/anaconda
```

#### 📜 Code Explanation

  * `export PATH=$PATH:$HOME/anaconda`: This command appends the `anaconda` directory (located in the user's home) to the end of the current `PATH`.
  * `echo $PATH | fold -50`: We run the check command again.
  * **Result**: The output now clearly shows `:/home/hashim/anaconda` at the very end.

**3. Add the `python` directory:**

```bash
hashim@Hashim:~$ PATH="$PATH:/usr/local/bin/python"
hashim@Hashim:~$ echo $PATH | fold -50
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:
/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/
snap/bin:/home/hashim/:/home/hashim/anaconda:/usr/
local/bin/python
```

#### 📜 Code Explanation

  * `PATH="$PATH:/usr/local/bin/python"`: This command appends the `/usr/local/bin/python` directory to the *new* `PATH` (which already includes the `anaconda` directory).
  * `echo $PATH | fold -50`: We run the check command a final time.
  * **Result**: The output now shows both new directories appended to the original path.

## 🏷️ Defining Custom Variables

You can also define your own variables in the shell. The following command defines an environment variable called `h1`:

```bash
h1=$HOME/test
```

Now enter the following command:

```bash
Echo $h1
```

You will see the following output:

```bash
hashim@Hashim:~$ h1=$HOME/test
hashim@Hashim:~$ echo $h1
/home/hashim/test
```

#### 📜 Code Explanation

  * `h1=$HOME/test`: This command creates a new shell variable named `h1`. It assigns it the string value `$HOME/test`. The shell expands `$HOME` to `/home/hashim`, so the final value stored is `/home/hashim/test`.
  * `echo $h1`: This command prints the value stored inside the `h1` variable. The dollar sign (`$`) is used to access the value of a variable.

## 🚀 Specifying Aliases

An **alias** is a shortcut or a nickname for a longer command.

The next code snippet shows you how to set the alias `ll` so that it displays the long listing of a directory:

```bash
alias ll="ls -l"
```

#### 📜 Code Explanation

  * `alias`: The command to create an alias.
  * `ll=`: The new shortcut name you want to use.
  * `"ls -l"`: The full command that will be executed when you type `ll`.

The following three alias definitions involve the `ls` command and various switches:

```bash
alias ll="ls -l"
alias lt="ls -lt"
alias ltr="ls -ltr"
```

#### 📜 Code Explanation

  * `alias ll="ls -l"`: Creates the `ll` alias for a long list.
  * `alias lt="ls -lt"`: Creates the `lt` alias for a long list, sorted by modification **time** (`t`).
  * `alias ltr="ls -ltr"`: Creates the `ltr` alias for a long list, sorted by time, in **reverse** (`r`) order.

As an example, you can replace the command `ls -ltr` (the letters “l,” “t,” and “r”) that you saw earlier in the chapter with the `ltr` alias and you will see the same reversed time-based long listing of filenames (reproduced here):

```bash
hashim@Hashim:~/Repo$ ltr
total 52
-rw-rw-r-- 1 hashim hashim   32 Oct 25 15:48  apple-care.text
-rw-rw-r-- 1 hashim hashim   34 Oct 25 15:48  checkin-commands.txt
-rw-rw-r-- 1 hashim hashim   32 Oct 25 15:49  iphonemeetup.txt
-rw-rw-r-- 1 hashim hashim   20 Oct 25 15:49  hello.txt
-rw-rw-r-- 1 hashim hashim   26 Oct 25 15:49  outfile.txt
-rw-rw-r-- 1 hashim hashim   30 Oct 25 15:50  output.txt
-rw-rw-r-- 1 hashim hashim   94 Oct 25 15:50  ssl-instructions.txt
drwxrwxr-x 2 hashim hashim 4096 Oct 25 15:54  instructions
```

#### 📜 Code Explanation

  * `ltr`: By typing the alias, the shell automatically executes the full command `ls -ltr`.

You can also define an alias that contains the bash pipe (`|`) symbol:

```bash
hashim@Hashim:~/Repo$ alias ltrm="ls -ltr|more"
```

#### 📜 Code Explanation

  * `alias ltrm="ls -ltr|more"`: This creates a new alias named `ltrm`.
  * The command `"ls -ltr|more"` will run `ls -ltr` and then **pipe** its output to the `more` command, which displays the output one page at a time.

Here is the output from running that alias:

```bash
hashim@Hashim:~/Repo$ alias ltrm="ls -ltr|more"
hashim@Hashim:~/Repo$ ltrm
total 52
-rw-rw-r-- 1 hashim hashim   32 Oct 25 15:48 apple-care.text
-rw-rw-r-- 1 hashim hashim   34 Oct 25 15:48 checkin-commands.txt
-rw-rw-r-- 1 hashim hashim   32 Oct 25 15:49 iphonemeetup.txt
-rw-rw-r-- 1 hashim hashim   20 Oct 25 15:49 hello.txt
-rw-rw-r-- 1 hashim hashim   26 Oct 25 15:49 outfile.txt
-rw-rw-r-- 1 hashim hashim   30 Oct 25 15:50 output.txt
-rw-rw-r-- 1 hashim hashim   94 Oct 25 15:50 ssl-instructions.txt
drwxrwxr-x 2 hashim hashim 4096 Oct 25 15:54 instructions
-rw-rw-r-- 1 hashim hashim    0 Oct 25 15:57 poem
-rw-rw-r-- 1 hashim hashim   69 Oct 25 16:05 temp1
-rw-rw-r-- 1 hashim hashim   46 Oct 25 16:05 temp2
-rw-rw-r-- 1 hashim hashim 1364 Oct 25 16:09 longfile.txt
-rw-rw-r-- 1 hashim hashim  405 Oct 25 17:00 longfile2.txt
-rwxrw-r-- 1 hashim hashim   21 Oct 25 17:37 python.py
-rw-rw-r-- 1 hashim hashim    0 Oct 25 17:41 test_script.sh
-rw-rw-r-- 1 hashim hashim    0 Oct 25 18:01 One Space.txt
--More--
```

#### 📜 Code Explanation

  * `ltrm`: Running this alias executes `ls -ltr | more`.
  * `--More--`: This at the bottom of the screen confirms that the output was successfully piped to the `more` command, which is now pausing the output.

In a similar manner, you can define aliases for directory-related commands:

```bash
alias ltd="ls -lt | grep '^d'"
alias ltdm="ls -lt | grep '^d' | more"
```

#### 📜 Code Explanation

  * `alias ltd="ls -lt | grep '^d'"`: This creates an alias `ltd` (list time directories).
      * `ls -lt`: Lists files sorted by time.
      * `| grep '^d'`: Pipes the output to `grep`.
      * `'^d'`: This is a regular expression that searches for lines that **start with** (`^`) the letter `d`. In an `ls -l` output, `d` signifies a directory.
      * **Result**: This alias lists *only directories*, sorted by time.
  * `alias ltdm="ls -lt | grep '^d' | more"`: This creates a similar alias, `ltdm`, but pipes the final filtered list of directories to `more`.

Here is the output from running those aliases:

```bash
hashim@Hashim:~/Repo$ alias ltd="ls -lt | grep '^d'"
hashim@Hashim:~/Repo$ alias ltdm="ls -lt | grep '^d' | more"
hashim@Hashim:~/Repo$ ltd
drwxrwxr-x 2 hashim hashim 4096 Oct 25 15:54 instructions
hashim@Hashim:~/Repo$ ltdm
drwxrwxr-x 2 hashim hashim 4096 Oct 25 15:54 instructions
```

#### 📜 Code Explanation

  * `ltd`: This command runs, and in this example, it finds only one directory (`instructions`).
  * `ltdm`: This command runs and finds the same single directory. Because the output is so short (only one line), the `more` command does not need to pause, so the output appears identical to `ltd`.

---

# 🔎 **Finding Executable Files**

There are several commands available for finding executable files (binary files or shell scripts) by searching the directories listed in the `PATH` environment variable. These include `which`, `whence`, `whereis`, and `whatis`. These commands produce results similar to the `which` command, as discussed below.

-----

## `which`

The `which` command gives the full path to whatever executable you specify. It will return a blank line if the executable is not in any directory specified in the `PATH` environment variable. This is useful for finding whether a particular command or utility is installed on the system.

```bash
which rm
```

The output of the preceding command is here:

```bash
/usr/bin/rm
```

### 📜 Code Explanation

  * `which`: This command searches all directories listed in your `$PATH` environment variable to find the location of a specified executable.
  * `rm`: This is the argument passed to `which`. It is the name of the executable file we are looking for (the "remove" command).
  * `/usr/bin/rm`: This is the output, showing the absolute path to the `rm` command. This tells you exactly which `rm` program will be run when you type `rm` in your terminal.

## `whereis`

The `whereis` command provides the information that you get from the `which` command, but it also includes the path to the command's manual (man) page and source code, if available.

```bash
whereis rm
```

The output is:

```bash
rm: /usr/bin/rm /usr/share/man/man1/rm.1.gz
```

### 📜 Code Explanation

  * `whereis`: This command searches a standard set of directories for binaries, manual pages, and source files.
  * `rm`: The name of the command to locate.
  * `rm:`: The output is prefixed with the name of the command.
  * `/usr/bin/rm`: This is the path to the binary executable (the program itself).
  * `/usr/share/man/man1/rm.1.gz`: This is the path to the compressed manual (man) page for the `rm` command.

## `whatis`

The `whatis` command looks up the specified command in the `whatis` database, which contains short, one-line descriptions of commands. This is useful for quickly identifying system commands and important configuration files.

```bash
whatis rm
```

The output is:

```bash
rm (1)               - remove files or directories
```

### 📜 Code Explanation

  * `whatis`: This command searches the `whatis` database for a brief description matching the argument.
  * `rm`: The name of the command to look up.
  * `rm (1)`: The output shows the command name (`rm`) and the manual section it belongs to (`1` for executable programs or shell commands).
  * `- remove files or directories`: This is the concise, one-line description of what the `rm` command does.

Consider `whatis` a simplified `man` command. The `man` command (e.g., typing `man ls`) displays several pages of detailed explanation regarding a command.

-----

# 🗣️ **The `printf` Command and The `echo` Command**

In brief, you should use the `printf` command instead of the `echo` command if you need to control the output format.

One key difference is that the `echo` command, by default, prints a **newline character** at the end of its output. The `printf` statement does not print a newline character unless you explicitly tell it to. (Keep this point in mind when you see the `printf` statement in the `awk` code samples in Chapter 7.)

### `printf` Example

As a simple example, place the following code snippet in a shell script:

```bash
printf "%-5s %-10s %-4s\n" ABC DEF GHI
printf "%-5s %-10s %-4.2f\n" ABC DEF 12.3456
```

Make the shell script executable, and then launch the shell script. You will see the following output:

```bash
ABC DEF GHI
ABC DEF 12.35
```

#### 📜 Code Explanation

  * **`printf "%-5s %-10s %-4s\n" ABC DEF GHI`**
      * `printf`: The "print formatted" command.
      * `"..."`: This is the format string that defines how the output should look.
      * `%-5s`: This is a format specifier. `s` means "string." `5` means "allocate 5 character spaces." `-` means "left-justify" the text within those spaces. This is applied to `ABC`.
      * `%-10s`: A left-justified, 10-space string field. This is applied to `DEF`.
      * `%-4s`: A left-justified, 4-space string field. This is applied to `GHI`.
      * `\n`: This is the special character for a **newline**. Without this, the next `printf` would start on the same line.
  * **`printf "%-5s %-10s %-4.2f\n" ABC DEF 12.3456`**
      * `%-5s %-10s`: These are the same string specifiers for `ABC` and `DEF`.
      * `%-4.2f`: This is a format specifier for a floating-point number (`f`). The `.2` instructs `printf` to round the number to **two decimal places**. The `-` and `4` ensure it is left-justified in a 4-space field.
      * `12.3456`: This number is passed to the `%.2f` specifier, which formats and rounds it to `12.35`.

### `echo` Example

Type the following pair of commands:

```bash
echo "ABC DEF GHI"
echo "ABC DEF 12.3456"
```

You will see the following output:

```bash
ABC DEF GHI
ABC DEF 12.3456
```

#### 📜 Code Explanation

  * `echo "ABC DEF GHI"`: The `echo` command prints the given string exactly as it is. After printing, it automatically adds a newline character.
  * `echo "ABC DEF 12.3456"`: This command prints its string on the next line (due to the newline from the first `echo`). It prints the number `12.3456` without any formatting or rounding, and then adds its own newline at the end.

-----

# ✂️ **The `cut` Command**

The `cut` command enables you to extract fields (sections of text) from an input stream using a specified delimiter. It can also be used to extract a range of characters based on their position.

(Note: A **delimiter** is another word commonly used for `IFS` (Internal Field Separator), especially when it is part of a command syntax instead of being set as an outside variable.)

### Example 1: Extract by Delimiter

Here is an example of extracting a field using a delimiter.

```bash
hashim@Hashim:~/Repo$ x="Muhammad Hashim Tahir"
hashim@Hashim:~/Repo$ echo $x | cut  -d" " -f 2
Hashim
```

#### 📜 Code Explanation

  * `x="Muhammad Hashim Tahir"`: This line assigns a string value to the shell variable `x`.
  * `echo $x`: This command prints the value of `x` (`Muhammad Hashim Tahir`) to standard output.
  * `|`: This is the pipe. It takes the output from the `echo` command and sends it as the input to the `cut` command.
  * `cut`: The "cut" command, used for extracting sections of text.
  * `-d" "`: This is the **delimiter** option. It tells `cut` to use the space character (`" "`) as the separator between fields.
  * `-f 2`: This is the **field** option. It tells `cut` to extract only the **2nd** field.
  * **Result:** `cut` receives `Muhammad Hashim Tahir`. It splits this string by spaces into:
    1.  `Muhammad`
    2.  `Hashim`
    3.  `Tahir`
        It then returns the 2nd field, which is `Hashim`.

### Example 2: Extract by Character Range

Consider this code snippet:

```bash
hashim@Hashim:~/Repo$ x="Muhammad Hashim Tahir"
hashim@Hashim:~/Repo$ echo $x | cut -c10-15
Hashim
```

#### 📜 Code Explanation

  * `x="Muhammad Hashim Tahir"`: The same variable is set.
  * `echo $x | cut`: The value of `x` is piped as input to the `cut` command.
  * `-c10-15`: This is the **character** option. It tells `cut` to extract the characters from position **10** through position **15**. (Character counting starts at 1).
  * **Result:** In the string `Muhammad Hashim Tahir`:
      * `H` is the 10th character.
      * `a` is the 11th.
      * `s` is the 12th.
      * `h` is the 13th.
      * `i` is the 14th.
      * `m` is the 15th.
        The command extracts and prints `Hashim`.

# 🚀 Bash Script: Filename Manipulation

This document details a shell script designed to parse, modify, and reconstruct a filename based on dot-delimited fields. Below are the script's contents, a detailed explanation of its logic, and instructions on how to make it executable and run it.

-----

## 📜 Example 3: cut in a Shell Script

Here is the complete, proper shell script, including the necessary shebang (`#!/bin/bash`) line.

```bash
#!/bin/bash
# Script to split, modify, and reassemble a filename

fileName="06.22.04p.vp.0.tgz"

f1=`echo $fileName | cut -d"." -f1`
f2=`echo $fileName | cut -d"." -f2`
f3=`echo $fileName | cut -d"." -f3`
f4=`echo $fileName | cut -d"." -f4`
f5=`echo $fileName | cut -d"." -f5`

f5=`expr $f5 + 12`

newFileName="${f1}.${f2}.${f3}.${f4}.${f5}"

echo "newFileName: $newFileName"
```

-----

## 💻 Detailed Code Explanation

Each line of the script performs a specific function:

  * `#!/bin/bash`
    This initial line is called a "shebang." It tells the operating system to execute this file using the `/bin/bash` (Bash shell) interpreter.

  * `fileName="06.22.04p.vp.0.tgz"`
    This line declares a new variable named `fileName` and assigns it the string value `06.22.04p.vp.0.tgz`.

  * `f1=`echo $fileName | cut -d"." -f1\`` This line uses command substitution (indicated by the backticks `` `...` ``) to set the variable `f1\`.

      * `echo $fileName`: Prints the content of the `fileName` variable.
      * `|`: The pipe symbol sends the output of the `echo` command as input to the `cut` command.
      * `cut -d"." -f1`: The `cut` command is instructed to use a period (`-d"."`) as the delimiter and to extract the **first field** (`-f1`).
      * **Result**: `f1` is set to the value `06`.

  * `f2=`echo $fileName | cut -d"." -f2\``  Following the same logic, this line extracts the **second field** ( `-f2\`).

      * **Result**: `f2` is set to the value `22`.

  * `f3=`echo $fileName | cut -d"." -f3\``  This line extracts the **third field** ( `-f3\`).

      * **Result**: `f3` is set to the value `04p`.

  * `f4=`echo $fileName | cut -d"." -f4\``  This line extracts the **fourth field** ( `-f4\`).

      * **Result**: `f4` is set to the value `vp`.

  * `f5=`echo $fileName | cut -d"." -f5\``  This line extracts the **fifth field** ( `-f5\`).

      * **Result**: `f5` is set to the value `0`.

  * `f5=\`expr $f5 + 12\``This line performs an arithmetic operation to update the`f5\` variable.

      * `expr`: The "expression" command, used for basic math in shell scripts.
      * `$f5 + 12`: The command evaluates the expression `0 + 12`.
      * **Result**: The variable `f5` is reassigned the new value `12`.

  * `newFileName="${f1}.${f2}.${f3}.${f4}.${f5}"`
    This line constructs a new string by combining the values of the variables, separated by literal dots.

      * The values are `06`, `22`, `04p`, `vp`, and the *modified* value `12`.
      * Note: The original sixth field (`tgz`) was never captured, so it is not included in the new filename.
      * **Result**: `newFileName` is set to the string `06.22.04p.vp.12`.

  * `echo "newFileName: $newFileName"`
    Finally, this command prints the string ` newFileName:  ` followed by the value of the `newFileName` variable.

-----

## 🚀 How to Create and Run the Script

Here are the steps to save, add permissions, and execute the script.

### 1\. Create the Script File

First, you need to create the file. You can use a command-line text editor like `nano` or `vi`.

```bash
nano SplitName1.sh
```

Inside the editor, paste the complete script content from the section above. Save and exit the editor.

### 2\. Provide Execute Permissions

By default, new files do not have permission to be executed. You must add this permission using the `chmod` command.

```bash
chmod u+x SplitName1.sh
```

  * **`chmod`**: The "change mode" command, used to modify file permissions.
  * **`u+x`**: This flag grants (`+`) execute (`x`) permission to the **user** (`u`) who owns the file.

### 3\. Run the Script

Now that the file is executable, you can run it by specifying its path.

```bash
./SplitName1.sh
```

  * **`./`**: This prefix tells the shell to look for the executable file in the current directory (`.`).


## 📊 Expected Output

When you run the script, you will see the following output printed to your terminal:

```bash
newFileName: 06.22.04p.vp.12
```
---