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

# 🗑️ **Deleting Files with the `rm` Command**

The `rm` (remove) command is used to delete files. When you specify the `-r` option, the `rm` command can also remove the contents of a directory (as well as the directory itself).

## Basic File Deletion

To remove a single file, you use the `rm` command followed by the filename.

### 🚀 Practical Example

1.  **Create a file:** First, let's create a file to be deleted.
    ```bash
    hashim@Hashim:~$ touch temp_file.txt
    ```
2.  **Verify the file exists:**
    ```bash
    hashim@Hashim:~$ ls
    temp_file.txt
    ```
3.  **Remove the file:**
    ```bash
    hashim@Hashim:~$ rm temp_file.txt
    ```
4.  **Verify the file is gone:**
    ```bash
    hashim@Hashim:~$ ls
    (output is empty)
    ```

## ⚙️ Useful `rm` Options

The `rm` command has several important options:

  * **`-i` (interactive):** Prompts you for confirmation before every deletion. This is a very safe option to use.
  * **`-r` (recursive):** Required to delete directories and their contents.
  * **`-f` (force):** Suppresses all confirmation prompts and ignores errors for non-existent files.

### 🚀 Practical Example (`-i` Interactive Mode)

This is handy when you want to be prompted before a file is deleted.

1.  **Create a file:**
    ```bash
    hashim@Hashim:~$ touch file_to_delete.txt
    ```
2.  **Run `rm` in interactive mode:**
    ```bash
    hashim@Hashim:~$ rm -i file_to_delete.txt
    rm: remove regular empty file 'file_to_delete.txt'? 
    ```
3.  **Explanation:**
      * The command now pauses and waits for your input.
      * Type **`y`** (for "yes") and press Enter to delete the file.
      * Type **`n`** (for "no") and press Enter to skip the file and not delete it.

### 🚀 Practical Example (`-r` Recursive Mode)

You cannot delete a directory with a simple `rm` command. You must use the `-r` (recursive) option.

1.  **Create a directory and a file inside it:**
    ```bash
    hashim@Hashim:~$ mkdir my_folder
    hashim@Hashim:~$ touch my_folder/some_file.txt
    ```
2.  **Attempt to delete the directory (This will FAIL):**
    ```bash
    hashim@Hashim:~$ rm my_folder
    rm: cannot remove 'my_folder': Is a directory
    ```
3.  **Use the `-r` flag to delete the directory (This will SUCCEED):**
    ```bash
    hashim@Hashim:~$ rm -r my_folder
    ```
4.  **Verify it is gone:**
    ```bash
    hashim@Hashim:~$ ls
    (output is empty)
    ```

## ⚠️ The DANGEROUS Command: `rm -rf *`

You can remove the contents of the current directory and all of its subdirectories with this command:

```bash
rm -rf *
```

> **NOTE: Be EXTREMELY careful when you use `rm -rf *`\!** This command can permanently destroy your files. You might inadvertently delete the wrong tree of files on your machine.
>
>   * `r` = Recursive (deletes all folders and their contents)
>   * `f` = Force (deletes without asking for *any* confirmation)
>   * `*` = Wildcard for "everything in the current directory"

### 🚀 A *Safe* Practical Example (Using a Sandbox)

Let's demonstrate this command safely inside a "sandbox" directory.

1.  **Create a sandbox directory and move into it:**
    ```bash
    hashim@Hashim:~$ mkdir danger_zone
    hashim@Hashim:~$ cd danger_zone
    ```
2.  **Create a test structure inside the sandbox:**
    ```bash
    hashim@Hashim:~/danger_zone$ touch file1.txt file2.txt
    hashim@Hashim:~/danger_zone$ mkdir sub_folder
    hashim@Hashim:~/danger_zone$ touch sub_folder/file3.txt
    ```
3.  **View the structure:**
    ```bash
    hashim@Hashim:~/danger_zone$ ls -R
    .:
    file1.txt  file2.txt  sub_folder

    ./sub_folder:
    file3.txt
    ```
4.  **Run the `rm -rf *` command:** This will delete everything *inside* `danger_zone`.
    ```bash
    hashim@Hashim:~/danger_zone$ rm -rf *
    ```
5.  **Check the result:** The `danger_zone` directory is now empty.
    ```bash
    hashim@Hashim:~/danger_zone$ ls
    (output is empty)
    ```
6.  **Go back up to your home directory:**
    ```bash
    hashim@Hashim:~/danger_zone$ cd ..
    ```

## ✅ Best Practice: Preview with `ls` First

Before deleting a set of files using a wildcard (like `*`), you should always use the `ls` command *with the exact same wildcard* to preview the files you intend to delete.

### 🚀 Practical Example

1.  **Create a mix of files:**
    ```bash
    hashim@Hashim:~$ touch scriptA.sh scriptB.sh notes.txt report.doc
    ```
2.  **Preview what you *think* you will delete:** You only want to delete the shell scripts (`.sh` files).
    ```bash
    hashim@Hashim:~$ ls *.sh
    scriptA.sh  scriptB.sh
    ```
3.  **Confirm and Delete:** The `ls` command shows *only* the files you want to delete. Now you can safely run the `rm` command with the same pattern.
    ```bash
    hashim@Hashim:~$ rm *.sh
    ```
4.  **Check the result:** Only the `.sh` files are gone.
    ```bash
    hashim@Hashim:~$ ls
    notes.txt  report.doc
    ```

## 📝 Deleting Files from a List

Instead of specifying a list of files on the command line, you can use **command substitution** (the backticks `` `...` ``) to tell `rm` to read its list from another file.

The command `rm \`cat remove\_list.txt\`\` tells the shell:

1.  First, run the command `cat remove_list.txt`.
2.  Take the output of that command (the list of filenames).
3.  Use that output as the arguments for the `rm` command.

### 🚀 Practical Example

1.  **Create some files to delete:**
    ```bash
    hashim@Hashim:~$ touch old_log.txt temp.log main_log.txt
    hashim@Hashim:~$ ls
    main_log.txt  old_log.txt  temp.log
    ```
2.  **Create a file named `remove_list.txt` containing the files you want to delete:**
    ```bash
    hashim@Hashim:~$ nano remove_list.txt
    ```
    (Inside nano, add these two lines):
    ```
    old_log.txt
    temp.log
    ```
3.  **Run the `rm` command with command substitution:**
    ```bash
    hashim@Hashim:~$ rm `cat remove_list.txt`
    ```
4.  **Check the result:** Only `main_log.txt` remains.
    ```bash
    hashim@Hashim:~$ ls
    main_log.txt
    ```

## 📦 A Safer Alternative: Backing Up Files

If there is a possibility you might need some of the files you intend to delete, **do not use `rm`\!** A much safer option is to create a backup directory and **move** (`mv`) those files into it.

### 🚀 Practical Example

1.  **Create your files:**
    ```bash
    hashim@Hashim:~$ touch script_v1.sh script_v2.sh final_script.sh
    ```
2.  **Create a backup directory:**
    ```bash
    hashim@Hashim:~$ mkdir $HOME/backup-shell-scripts
    ```
3.  **Move (instead of delete) all `.sh` files to the backup directory:**
    ```bash
    hashim@Hashim:~$ mv *.sh $HOME/backup-shell-scripts
    ```
4.  **Verify the files are gone from your current directory:**
    ```bash
    hashim@Hashim:~$ ls
    (output is empty)
    ```
5.  **Verify the files are safe in the backup directory:**
    ```bash
    hashim@Hashim:~$ ls $HOME/backup-shell-scripts
    final_script.sh  script_v1.sh  script_v2.sh
    ```

This method is much safer and is highly recommended over permanent deletion.

# 📦 **Moving Files with the `mv` Command**

The `mv` (move) command is equivalent to a combination of `cp` (copy) and `rm` (remove). You can use this command to move one or more files to a new directory or to rename a file or directory.

When used in a non-interactive script, `mv` supports the **`-f` (force)** option to bypass any user input prompts (like overwriting).


## ➡️ Example 1: Renaming a File

The simplest use for `mv` is to rename a file.

### 🚀 Practical Example

1.  **Create a file:**
    ```bash
    hashim@Hashim:~$ touch old_name.txt
    hashim@Hashim:~$ ls
    old_name.txt
    ```
2.  **Rename it using `mv`:**
    ```bash
    hashim@Hashim:~$ mv old_name.txt new_name.txt
    ```
3.  **Verify the change:**
    ```bash
    hashim@Hashim:~$ ls
    new_name.txt
    ```

### 📜 Code Explanation

  * `mv old_name.txt new_name.txt`: The `mv` command takes a **source** (`old_name.txt`) and a **destination** (`new_name.txt`). Since the destination is a filename in the same directory, this operation acts as a "rename." The `old_name.txt` file is gone, and `new_name.txt` now exists.


## ➡️ Example 2: Moving Files to a Directory

You can also use `mv` to move one or more files into a directory.

### 🚀 Practical Example

1.  **Create files and a directory:**
    ```bash
    hashim@Hashim:~$ touch file1.txt file2.txt
    hashim@Hashim:~$ mkdir my_folder
    hashim@Hashim:~$ ls
    file1.txt  file2.txt  my_folder
    ```
2.  **Move both files into `my_folder`:**
    ```bash
    hashim@Hashim:~$ mv file1.txt file2.txt my_folder
    ```
3.  **Verify the files are moved:**
    ```bash
    hashim@Hashim:~$ ls
    my_folder

    hashim@Hashim:~$ ls my_folder
    file1.txt  file2.txt
    ```

### 📜 Code Explanation

  * `mv file1.txt file2.txt my_folder`: When the *last* argument is a directory, `mv` moves all preceding source files (`file1.txt`, `file2.txt`) into that directory.


## ➡️ Example 3: Moving a Directory

When a directory is moved to a pre-existing directory, it becomes a sub-directory of the destination.

### 🚀 Practical Example

1.  **Create two directories:**
    ```bash
    hashim@Hashim:~$ mkdir dir_A
    hashim@Hashim:~$ mkdir destination_folder
    hashim@Hashim:~$ touch dir_A/report.txt
    ```
2.  **Move `dir_A` into `destination_folder`:**
    ```bash
    hashim@Hashim:~$ mv dir_A destination_folder
    ```
3.  **Verify the result:**
    ```bash
    hashim@Hashim:~$ ls destination_folder
    dir_A

    hashim@Hashim:~$ ls destination_folder/dir_A
    report.txt
    ```

### 📜 Code Explanation

  * `mv dir_A destination_folder`: Since `destination_folder` already existed, `mv` "places" the entire `dir_A` directory *inside* it, creating `destination_folder/dir_A`.



# 🔗 The `ln` (Link) Command

The `ln` command enables you to create a **link** to an existing file. The most common type is a **symbolic link** (or "symlink"), created with the `-s` flag.

This is advantageous when the existing file is large because the symbolic link is just a small pointer file and involves minimal additional overhead.

Moreover, changes to the existing file are automatically available in the symbolic link. This means you can maintain one file as **"the source of truth"** instead of making the same update to multiple copies of a file.


## 🚀 Practical Example: Creating and Using a Symlink

Let's walk through the entire lifecycle of a symbolic link.

### 1\. Create the Original File

First, we create our "source of truth" file.

```bash
hashim@Hashim:~$ echo "This is the original document." > document1.txt
hashim@Hashim:~$ cat document1.txt
This is the original document.
```

### 2\. Create the Symbolic Link

Now, we create a symbolic link named `doc2` that points to `document1.txt`.

```bash
hashim@Hashim:~$ ln -s document1.txt doc2
```

### 📜 Code Explanation

  * `ln`: The link command.
  * `-s`: This flag specifies that we want to create a **symbolic** link.
  * `document1.txt`: This is the **target** (the original file we are pointing to).
  * `doc2`: This is the **name of the link** (the new shortcut file).

### 3\. Check the Link and File Listing

If you invoke `ls -l` on `doc2`, you will see something like this:

```bash
hashim@Hashim:~$ ls -l
total 4
-rw-rw-r-- 1 hashim hashim  30 Oct 29 20:12 document1.txt
lrwxrwxrwx 1 hashim hashim  15 Oct 29 20:12 doc2 -> document1.txt
```

  * **`l`**: Notice the `l` at the beginning of the permissions for `doc2`. This indicates it is a **link**.
  * **`-> document1.txt`**: This shows you exactly where the link is pointing.

### 4\. Test the "Source of Truth" Concept

Let's read the link. It will show the *original's* content.

```bash
hashim@Hashim:~$ cat doc2
This is the original document.
```

Now, let's **modify the original file** and read the link again.

```bash
hashim@Hashim:~$ echo "I am adding a new line." >> document1.txt
hashim@Hashim:~$ cat doc2
This is the original document.
I am adding a new line.
```

As you can see, the changes to `document1.txt` were automatically available in `doc2`.

### 5\. Deleting the Link (Safe)

If you remove the symbolic link (`doc2`), it **will not affect** the original file.

```bash
hashim@Hashim:~$ rm doc2
hashim@Hashim:~$ ls
document1.txt
hashim@Hashim:~$ cat document1.txt
This is the original document.
I am adding a new line.
```

The original file is perfectly safe.


### 6\. adding new text in doc2 will reflect in original file

```bash
hashim@Hashim:~/Repo$ echo "I am adding a third line." >> doc2
hashim@Hashim:~/Repo$ cat doc2
This is the original document.
I am adding a new line.
I am adding a third line.
hashim@Hashim:~/Repo$ cat document1.txt 
This is the original document.
I am adding a new line.
I am adding a third line.
```

### 7\. Deleting the Original File (Breaking the Link)

If you remove the original file (`document1.txt`) *without* removing the link, the link becomes "broken."

```bash
# First, let's re-create the link
hashim@Hashim:~$ ln -s document1.txt doc2

# Now, remove the ORIGINAL file
hashim@Hashim:~$ rm document1.txt
```

The link file `doc2` will still appear in a listing (often in red to show it's broken), but when you attempt to view its contents, the file is empty (or gives an error) because its target is gone.

```bash
hashim@Hashim:~$ ls -l
lrwxrwxrwx 1 hashim hashim 15 Oct 29 20:15 doc2 -> document1.txt

hashim@Hashim:~$ cat doc2
cat: doc2: No such file or directory
```

---

# 📇 **The `basename`, `dirname`, and `file` Commands**

These commands help you work with file paths and file types.

  * `basename`: Finds the base portion of a filename (the file itself).
  * `dirname`: Finds the directory portion of a filename (the path).
  * `file`: Determines the type of a file (e.g., text, executable).

Here are some examples:

### 🚀 Practical Example 1: `basename` (Simple)

```bash
$ x="/tmp/a.b.c.js"
$ basename $x .js
a.b.c
```

#### 📜 Code Explanation

1.  `x="/tmp/a.b.c.js"`: This line assigns a string (a full file path) to the variable `x`.
2.  `basename $x .js`:
      * `basename`: This is the command.
      * `$x`: This is the first argument, the file path `/tmp/a.b.c.js`. The command strips away the directory path (`/tmp/`), leaving `a.b.c.js`.
      * `.js`: This is an optional second argument. If provided, `basename` will also remove this suffix from the end of the name.
      * **Result**: The command returns `a.b.c`.

### 🚀 Practical Example 2: `basename` with Spaces (The Wrong Way)

This example shows a common pitfall when working with filenames that contain spaces.

```bash
$ a="/tmp/a b.js"
$ basename $a .js
a
b.js
.js
```

#### 📜 Code Explanation

This command **fails** because the variable `$a` was not enclosed in quotes.

1.  `a="/tmp/a b.js"`: Assigns a path with a space to the variable `a`.
2.  `basename $a .js`:
      * The shell performs **word splitting** on the unquoted `$a` *before* `basename` runs.
      * It breaks `"/tmp/a b.js"` into two separate words: `/tmp/a` and `b.js`.
      * The command `basename` receives three arguments: `/tmp/a`, `b.js`, and `.js`.
      * It processes the first argument (`/tmp/a`) and returns `a`.
      * It then (incorrectly) tries to process the other arguments, which results in the jumbled output.

### 🚀 Practical Example 3: `basename` with Spaces (The Correct Way)

To fix the previous error, we use double quotes (`""`) to protect the variable.

```bash
$ a="/tmp/a b.js"
$ basename "$a" .js
a b
```

#### 📜 Code Explanation

  * `basename "$a" .js`: By using `"$a"`, we tell the shell to treat the entire string `"/tmp/a b.js"` as a **single argument**.
  * `basename` correctly receives `/tmp/a b.js`, strips the `/tmp/` path, strips the `.js` suffix, and returns the correct filename: `a b`.

### 🚀 Practical Example 4: `dirname`

This command extracts just the directory portion of a path.

```bash
$ x="/tmp/a.b.c.js"
$ dirname $x
/tmp
```

#### 📜 Code Explanation

  * `dirname $x`: This command reads the variable `$x` and removes everything after the last `/`, returning only the directory path.

### 🚀 Practical Example 5: `file`

This command inspects a file and tells you what type it is.

```bash
$ file /bin/ls
/bin/ls: Mach-O 64-bit executable x86_64
```

#### 📜 Code Explanation

  * `file /bin/ls`: This command analyzes the `/bin/ls` file (the "list" command itself).
  * **Output**: The output `Mach-O 64-bit executable x86_64` identifies it as a 64-bit program. (On Linux, this output would look different, such as `ELF 64-bit LSB executable`).

# 📊 The `wc` Command

You can view the number of **lines**, **words**, and **characters** in a set of files using the `wc` (word count) command.

### 🚀 Practical Example 1: Counting in Multiple Files

If you execute the command `wc *`, you will see information for all the files in a directory.

```bash
# First, create the files from the example
$ echo "one two" > outfile.txt
$ echo -e "one two\nthree four" > output.txt

# Now, run wc on files starting with 'o'
$ wc o*
  2  4  19 output.txt
  1  2   8 outfile.txt
  3  6  27 total
```

*(Note: The counts above are based on the files created here, but the format matches the user's text.)*

#### 📜 Code Explanation

  * `wc o*`: This runs the `wc` command on all files that start with the letter `o`.
  * **Output Columns**: The output shows three columns for each file: **Lines**, **Words**, and **Characters**.
  * `total`: The last line is a summary of all the files processed.

### 🚀 Practical Example 2: Counting Files in a Directory

You can count the number of files in a directory with `ls -1 | wc`.

```bash
# Using the two files we just created
$ ls -1 | wc
       2       2      20
```

#### 📜 Code Explanation

1.  `ls -1`: This command lists the files in the current directory, placing **one file per line**.
2.  `|`: The pipe symbol sends the output of `ls -1` (a list of filenames) as input to the `wc` command.
3.  `wc`: The `wc` command reads the input and counts the lines, words, and characters.
4.  **Result**: The first number (`2`) is the **line count**, which, in this case, is equal to the **file count**.

# 🐱 The `cat` Command

The `cat` command displays the *entire contents* of a file, which is convenient for small files.

### 🚀 Practical Example

```bash
# First, create the file
$ echo "iPhone meetup" > iphonemeetup.txt
$ echo "=============" >> iphonemeetup.txt
$ echo "* iPhone.WebDev.com" >> iphonemeetup.txt
$ echo "* iui.googlecode.com" >> iphonemeetup.txt

# Now, use 'cat' to display it
$ cat iphonemeetup.txt
iPhone meetup
=============
* iPhone.WebDev.com
* iui.googlecode.com
```

#### 📜 Code Explanation

  * `cat iphonemeetup.txt`: This command reads the file `iphonemeetup.txt` and prints its full contents to standard output (the terminal).

You can see size-related attributes with the `wc` command:

```bash
$ wc iphonemeetup.txt
       4      10      77 iphonemeetup.txt
```

*(This output shows our created file has 4 lines, 10 words, and 77 characters.)*

# 📖 The `more` and `less` Commands

The `more` command enables you to view "pages" of content in a file. Press the **space bar** to advance to the next page and press the **return key** to advance a single line. The `less` command is similar (but more modern, as it allows you to scroll backward).

### 🚀 Practical Example 1: Basic Usage

```bash
# First, create a long file
$ seq 1 100 > abc.txt

# Now, view it with 'more'
$ more abc.txt
1
2
3
4
...
--More--(1%)
```

#### 📜 Code Explanation

  * `seq 1 100 > abc.txt`: This creates a file named `abc.txt` containing 100 lines (the numbers 1 through 100).
  * `more abc.txt`: This opens the file. It displays the first screenful of content and then pauses, showing `--More--` at the bottom.

### 🚀 Practical Example 2: Inefficient Usage

```bash
$ cat abc.txt | more
```

#### 📜 Code Explanation

  * This achieves the same result, but it is **less efficient** because it uses two processes (`cat` and `more`) when only one (`more abc.txt`) is needed.

### 🚀 Practical Example 3: `more` Options

The `more` command contains some useful options:

  * **Start from a specific line** (e.g., line 15):

    ```bash
    $ more +15 abc.txt
    ```

    *(This command will open `abc.txt` but will start the view at line 15.)*

  * **Squeeze blank lines** (remove multiple consecutive blank lines):

    ```bash
    # Create a file with blank lines
    $ echo -e "Line 1\n\n\nLine 2" > blank_file.txt

    # View with -s (squeeze)
    $ more -s blank_file.txt
    Line 1

    Line 2
    ```

      * **📜 Code Explanation**: The `-s` flag compressed the three blank lines into one.

  * **Search for a pattern**:

    ```bash
    # Start viewing 'abc.txt' at the first line containing "30"
    $ more +/30 abc.txt
    ```

# ⬆️ The `head` Command

The `head` command enables you to display an initial set of lines (the "head" of the file). The default number is 10.

### 🚀 Practical Example 1: Basic Usage

```bash
# Create a test file
$ seq 1 10 > test.txt

# Display the first 3 lines
$ head -3 test.txt
1
2
3
```

#### 📜 Code Explanation

  * `head -3 test.txt`: The `-3` flag tells `head` to display only the first 3 lines, overriding the default of 10.
  * `cat test.txt | head -3`: This is the less efficient, pipe-based way to do the same thing.

### 🚀 Practical Example 2: `head` on Multiple Files

```bash
# Create the files
$ echo -e "one two\nthree four\none two three four\n..." > columns2.txt
$ echo -e "123 one two\n456 three four\none two three four\n..." > columns3.txt

# Run 'head' on both
$ head -3 columns[2-3].txt
==> columns2.txt <==
one two
three four
one two three four

==> columns3.txt <==
123 one two
456 three four
one two three four
```

#### 📜 Code Explanation

  * `head -3 columns[2-3].txt`: This runs `head` on all files matching the pattern. `head` adds `==> ... <==` headers to the output to show which file the content belongs to.

### 🚀 Practical Example 3: `head` in a Script

This code snippet checks if the first line of `test.txt` contains the string "aa".

```bash
# Create the test file
$ echo "first line contains aa" > test.txt
$ echo "second line" >> test.txt

# Run the commands
$ x=`cat test.txt | head -1 | grep aa`
$ if [ "$x" != "" ]; then echo "found aa in the first line"; fi
found aa in the first line
```

#### 📜 Code Explanation

1.  `x=\`cat test.txt | head -1 | grep aa\`\`: This line is complex:
      * `cat test.txt`: Reads the file.
      * `| head -1`: Pipes the content to `head`, which takes *only the first line*.
      * `| grep aa`: Pipes that single line to `grep`, which checks if it contains "aa".
      * Since it *does* contain "aa", `grep` outputs the line `first line contains aa`.
      * `x=\`...\`` : The variable  `x\` captures that output.
2.  `if [ "$x" != "" ]`: This checks if the variable `x` is *not* empty. Since `grep` found a match, `x` is not empty, and the condition is true.
3.  `echo "..."`: The "found" message is printed.

### 🚀 Practical Example 4: `head` with `wc`

```bash
# Create a 10-line file
$ seq 1 10 > iphonemeetup.txt

$ head iphonemeetup.txt | wc
      10      10      21
```

#### 📜 Code Explanation

  * `head iphonemeetup.txt`: This command outputs the first 10 lines (which is the whole file in this case).
  * `| wc`: This pipes the 10 lines to `wc`, which counts the lines, words, and characters of that output.

### 🚀 Practical Example 5: `head` with Numbered Lines

```bash
# Use the same file
$ cat -n iphonemeetup.txt | head -4
     1  1
     2  2
     3  3
     4  4
```

#### 📜 Code Explanation

  * `cat -n iphonemeetup.txt`: This command reads the file and adds line numbers (`-n`) to its output.
  * `| head -4`: This pipes the numbered output to `head`, which displays only the first 4 lines.

# ⬇️ The `tail` Command

The `tail` command enables you to display a set of lines at the end of a file. The default number is 10.

### 🚀 Practical Example 1: Basic Usage

```bash
# Create a test file
$ seq 1 10 > test.txt

# Display the last 3 lines
$ tail -3 test.txt
8
9
10
```

#### 📜 Code Explanation

  * `tail -3 test.txt`: The `-3` flag tells `tail` to display only the last 3 lines.
  * `cat test.txt | tail -3`: This is the less efficient, pipe-based way to do the same thing.

-----

### 🚀 Practical Example 2: `tail` on Multiple Files

```bash
# Use the 'columns' files from the 'head' example
$ tail -3 columns[2-3].txt
==> columns2.txt <==
three four
one two three four
...

==> columns3.txt <==
456 three four
one two three four
...
```

#### 📜 Code Explanation

  * `tail -3 columns[2-3].txt`: This runs `tail` on all matching files, showing the last 3 lines from each. It also adds headers (`==> ... <==`) for clarity.

### 🚀 Practical Example 3: `tail` in a Script

This code snippet checks if the *last* line of `test.txt` contains the string "aa".

```bash
# Create the test file
$ echo "first line" > test.txt
$ echo "last line has aa" >> test.txt

# Run the commands
$ x=`cat test.txt | tail -1 | grep aa`
$ if [ "$x" != "" ]; then echo "found aa in the test.txt"; fi
found aa in the test.txt
```

#### 📜 Code Explanation

*(Note: The original text incorrectly states this checks the "first line". This code checks the LAST line.)*

1.  `x=\`cat test.txt | tail -1 | grep aa\`\`:
      * `cat test.txt`: Reads the file.
      * `| tail -1`: Pipes the content to `tail`, which takes *only the last line*.
      * `| grep aa`: Pipes that single line to `grep`, which checks if it contains "aa".
      * It *does* contain "aa", so `x` is set to `last line has aa`.
2.  `if [ "$x" != "" ]`: The variable `x` is not empty, so the condition is true.
3.  `echo "..."`: The "found" message is printed.

### 🚀 Practical Example 4: `tail` with `wc`

```bash
# Create a 10-line file
$ seq 1 10 > iphonemeetup.txt

$ tail iphonemeetup.txt | wc
      10      10      21
```

#### 📜 Code Explanation

  * `tail iphonemeetup.txt`: This command outputs the last 10 lines (the whole file).
  * `| wc`: This pipes the 10 lines to `wc` for counting.
  * **Note**: If a file has 10 or fewer lines, the output of `head`, `tail`, and `cat` will be identical.

### 🚀 Practical Example 5: `tail -f` (Follow)

The `tail -f` option is useful when you have a long-running process that is redirecting output to a file (like a log). `tail -f` will "follow" the file and print new lines as they are added.

**This example requires two separate terminals.**

**In Terminal 1:**

1.  Create an empty log file.
2.  Run `tail -f` to start monitoring it. The cursor will just blink, waiting for new content.

<!-- end list -->

```bash
# In Terminal 1
$ touch /tmp/abc
$ tail -f /tmp/abc
(cursor blinks and waits...)
```

**In Terminal 2:**

1.  Append some text to the log file.

<!-- end list -->

```bash
# In Terminal 2
$ echo "Process started... log entry 1" >> /tmp/abc
$ echo "WARNING: Low memory" >> /tmp/abc
```

**Observe Terminal 1:**

As soon as you run the commands in Terminal 2, the new lines will **instantly appear** in Terminal 1.

```bash
# In Terminal 1
$ tail -f /tmp/abc
Process started... log entry 1
WARNING: Low memory
(cursor blinks and waits for more...)
```

This is a critical command for monitoring log files in real time.

---