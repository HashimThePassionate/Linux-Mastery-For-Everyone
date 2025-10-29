# 📁 **Working with Files and Directories**

This section discusses files, directories, and various useful Bash commands for managing them. You will learn how to use simple commands that can simplify your tasks.

In section 8 and section 9, you will learn how to create shell scripts involving some of the commands in this section. This will further reduce the amount of time you spend performing routine tasks.

---

## In This section

* **Part 1:** Works with file-related commands, such as `touch`, `mv`, `cp`, and `rm`.
* **Part 2:** Contains shell commands for managing directories.
* **Part 3:** Discusses metacharacters and variables that you can use when working with files and shell scripts.

---

## Create, Copy, Remove, and Move Files

This section discusses file-related commands in Bash, such as `touch`, `cp`, and `rm`. These commands enable you to create, copy, and remove files, respectively.

The following subsections illustrate how to use these convenient commands. Except for the section that discusses how to create text files, the other sections pertain to text files as well as binary files.

## 📂 Working with the `cp` (Copy) Command

You can copy the file `abc` (or any other file) to a new file named `abc2` with this command:

```bash
cp abc abc2
```

### 📜 Code Explanation

  * `cp`: This is the "copy" command.
  * `abc`: This is the **source** file (the file you want to copy).
  * `abc2`: This is the **destination** file (the new file that will be created).


## ⚠️ A Critical Warning: Overwriting Files

Be very careful when you use the `cp` command. If the target file (`abc2` in our example) already exists, `cp` will **permanently overwrite its contents** with the contents of the source file, without asking for confirmation.

### practical example (overwriting)

Let's see this in action:

1.  **Create two different files:**
    ```bash
    hashim@Hashim:~$ echo "This is the original File A." > file_A
    hashim@Hashim:~$ echo "This is the original File B." > file_B
    ```
2.  **Check the content of File B:**
    ```bash
    hashim@Hashim:~$ cat file_B
    This is the original File B.
    ```
3.  **Now, copy File A *onto* File B:**
    ```bash
    hashim@Hashim:~$ cp file_A file_B
    ```
4.  **Check the content of File B again:**
    ```bash
    hashim@Hashim:~$ cat file_B
    This is the original File A.
    ```
    The original content of `file_B` is gone and has been replaced by the content of `file_A`.


# 🗂️ Copying Multiple Files to a Directory

You can also copy one or more files into a directory. The syntax is `cp <source_file_1> <source_file_2> <destination_directory>`.

For example, both of the following commands copy the files `abc` and `abc2` into the `/tmp` directory:

```bash
cp abc abc2 /tmp
```

```bash
cp abc* /tmp
```

### 📜 Code Explanation

  * `cp abc abc2 /tmp`: This command explicitly lists each source file (`abc`, `abc2`) and then provides the destination directory (`/tmp`).
  * `cp abc* /tmp`: This command uses a wildcard (`*`). The shell expands `abc*` to match all files *beginning* with `abc` (e.g., `abc` and `abc2`) and then passes that list to `cp`.

## 🛡️ Preventing Overwrites with `noclobber`

Fortunately, you can prevent an existing file from being overwritten by setting a shell option called `noclobber`.

### practical example (noclobber)

1.  **Enable the `noclobber` option:**

    ```bash
    hashim@Hashim:~$ set -o noclobber
    ```

      * `set -o`: This is the command to set a shell option.
      * `noclobber`: This option prevents output redirection (`>`) and `cp` from overwriting existing files.

2.  **Create our test files:**

    ```bash
    hashim@Hashim:~$ echo "File A" > file_A
    hashim@Hashim:~$ echo "File B" > file_B
    ```

3.  **Now, try to overwrite `file_B`:**

    ```bash
    hashim@Hashim:~$ cp file_A file_B
    bash: file_B: cannot overwrite existing file
    ```

    The shell now blocks the command and gives you an error message, protecting your file.

4.  **To disable `noclobber`, use `+o`:**

    ```bash
    hashim@Hashim:~$ set +o noclobber
    ```


## ⚙️ Useful `cp` Switches (Options)

The `cp` command provides several useful switches that enable you to control the set of files that you want to copy:

  * **`-a` (archive):** This is the archive flag. It's a "super-copy" that preserves the entire directory structure, file permissions, timestamps, and ownership. It's the same as using `-dR --preserve=all`.
  * **`-u` (update):** This is the update flag. `cp` will only copy the file if the source file is *newer* than the destination file (or if the destination file doesn't exist).
  * **`-r` or `-R` (recursive):** These are the recursive flags, which are essential for copying directories.


## 🌳 Example: Recursive Copy (`-r`)

The `–r` option is useful for copying the files and all the sub-directories of one directory to another directory.

For example, the following commands copy the files and sub-directories (if any) from `$HOME/abc` to the directory `$HOME/def`:

### practical example (recursive copy)

1.  **First, create our source and destination directories:**

    ```bash
    hashim@Hashim:~$ mkdir -p $HOME/abc/subdir
    hashim@Hashim:~$ mkdir -p $HOME/def
    ```

      * `mkdir -p`: Creates the directory and any necessary parent directories.

2.  **Create some files inside the source directory `$HOME/abc`:**

    ```bash
    hashim@Hashim:~$ echo "This is file 1" > $HOME/abc/file1.txt
    hashim@Hashim:~$ echo "This is a sub-file" > $HOME/abc/subdir/subfile.txt
    ```

3.  **View the source directory:**

    ```bash
    hashim@Hashim:~$ ls -R $HOME/abc
    /home/hashim/abc:
    file1.txt  subdir

    /home/hashim/abc/subdir:
    subfile.txt
    ```

      * `ls -R`: Lists the contents of the directory *recursively*, showing all sub-folders.

4.  **Now, perform the recursive copy:**

    ```bash
    hashim@Hashim:~$ cd $HOME/abc
    hashim@Hashim:~/abc$ cp -r . ../def
    ```

      * `cd $HOME/abc`: We first change our location *into* the source directory.
      * `cp -r . ../def`: This is the copy command.
          * `-r`: The recursive flag, telling `cp` to copy directories.
          * `.`: This is a shortcut for the **current directory** (`/home/hashim/abc`).
          * `../def`: This is the path to the destination. `..` means "go up one level" (to `/home/hashim`), and then go into the `def` directory.

5.  **Finally, check the destination directory `$HOME/def`:**

    ```bash
    hashim@Hashim:~/abc$ ls -R $HOME/def
    /home/hashim/def:
    file1.txt  subdir

    /home/hashim/def/subdir:
    subfile.txt
    ```

    As you can see, the entire contents of the `abc` directory, including its `subdir` and all files, have been successfully copied into the `def` directory.



# 🚀 Copying Files with Command Substitution

This section details how to use command substitution (the backticks `` `...` ``) to generate a dynamic list of files and pass them to the `cp` (copy) command.


## 📜 Listing 2.1: The Basic Command

Listing 2.1 shows a two-step process to create a directory and then copy a specific set of files into it using command substitution.

```bash
mkdir textfiles
cp `ls *.txt` textfiles
```

*(Note: The original text used `*txt`. We are using `*.txt` as it is the standard and more common way to find text files.)*

### 🚀 Practical Example (How it Works)

Let's see this in action from scratch.

1.  **First, create some sample files:**

    ```bash
    hashim@Hashim:~$ touch file1.txt notes.txt report.doc
    hashim@Hashim:~$ ls
    file1.txt  notes.txt  report.doc
    ```

2.  **Now, run the commands from Listing 2.1:**

    ```bash
    hashim@Hashim:~$ mkdir textfiles
    hashim@Hashim:~$ cp `ls *.txt` textfiles
    ```

3.  **Check the result:**

    ```bash
    hashim@Hashim:~$ ls textfiles/
    file1.txt  notes.txt
    ```

    As you can see, only the `.txt` files were copied. The `report.doc` was ignored.

### 📜 Code Explanation

1.  **`mkdir textfiles`**
    This command simply **creates a new directory** named `textfiles` in your current location.

2.  cp \`ls *.txt\` textfiles
    This command has two parts, and the shell processes it from the inside out:

      * **Command Substitution (`` `ls *.txt` ``):** The shell first executes the command inside the backticks. `ls *.txt` finds all files ending in `.txt` and lists them as a string (e.g., `file1.txt notes.txt`).
      * **Shell Expansion:** The shell then *replaces* the backtick expression with its output. The final command becomes:
        `cp file1.txt notes.txt textfiles`
      * **Execution:** The `cp` command runs, copying `file1.txt` and `notes.txt` into the `textfiles` directory.

> **💡 A Better Way: `$(...)`**
> Using backticks `` `...` `` is an older syntax. The modern, recommended syntax is `$(...)`. It's cleaner and avoids confusion. The command is:
> `cp $(ls *.txt) textfiles`


## ⚠️ The Caveat: This Ignores Directories

This command substitution method **will not copy any subdirectories** that happen to match the pattern (e.g., `archive.txt`). The `cp` command, by default, will skip directories and print an error message.

### 🚀 Practical Example (The Failure)

1.  **Create a file and a directory that match the `*.txt` pattern:**

    ```bash
    hashim@Hashim:~$ touch regular.txt
    hashim@Hashim:~$ mkdir archive.txt
    hashim@Hashim:~$ echo "secret log" > archive.txt/log.dat
    ```

2.  **Try to copy all `*.txt` items using the same command:**

    ```bash
    hashim@Hashim:~$ mkdir textfiles2
    hashim@Hashim:~$ cp $(ls *.txt) textfiles2
    cp: omitting directory 'archive.txt'
    ```

3.  **Check the result:**

    ```bash
    hashim@Hashim:~$ ls textfiles2/
    regular.txt
    ```

    The `cp` command failed to copy the `archive.txt` directory and only copied the `regular.txt` file.


## ❌ Flawed Attempts to Fix This

The original text notes that just adding the recursive flag (`-r`) to the `cp` command *will not* fix this. Both of the following commands will still fail to copy the contents properly:

  * `cp $(ls -R *.txt) textfiles`
  * `cp -r $(ls -R *.txt) textfiles`

The `ls -R` command produces a flat list of all matching files (e.g., `archive.txt/log.dat`), which the `cp` command cannot find in the current directory.


## ✅ The Correct Way to Copy Directories

If you want to copy a directory, you should **not** use `ls` in command substitution. Just use the `cp` command directly.

### ➡️ Solution A: Copy the Entire Directory

To copy the directory `archive.txt` *into* the `textfiles` directory (so you have `textfiles/archive.txt`), use the `-r` (recursive) flag.

```bash
cp -r archive.txt textfiles
```

#### 🚀 Practical Example

```bash
hashim@Hashim:~$ mkdir textfiles3
hashim@Hashim:~$ cp -r archive.txt textfiles3
hashim@Hashim:~$ ls -R textfiles3/
textfiles3/:
archive.txt

textfiles3/archive.txt:
log.dat
```

As you see, the `archive.txt` folder itself was placed *inside* `textfiles3`.

### ➡️ Solution B: Copy Only the Directory's Contents

If you want to copy *only the contents* of `archive.txt` (i.e., `log.dat`) into `textfiles`, use the `/*` wildcard.

```bash
cp -r archive.txt/* textfiles
```

#### 🚀 Practical Example

```bash
hashim@Hashim:~$ mkdir textfiles4
hashim@Hashim:~$ cp -r archive.txt/* textfiles4
hashim@Hashim:~$ ls textfiles4/
log.dat
```

Here, the `/*` wildcard tells `cp` to copy all items *inside* `archive.txt`, but not the folder itself.