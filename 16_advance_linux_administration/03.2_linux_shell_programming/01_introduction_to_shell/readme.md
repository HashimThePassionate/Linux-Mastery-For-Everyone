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