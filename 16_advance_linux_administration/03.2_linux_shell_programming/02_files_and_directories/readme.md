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

# 🔍 **Comparing File Contents**

There are several commands for comparing text files, such as the `cmp` command and the `diff` command.

  * `diff`: This command reports the **specific differences** between two files, showing you the exact lines that have changed.
  * `cmp`: This command is a simpler version. It only shows *at what byte and line number* the files first differ. If the files are identical, it produces no output.

Both `diff` and `cmp` return an **exit status of 0** if the compared files are identical, and **1** if the files are different. This allows you to use them in a test construct within a shell script.

### `cmp` (Compare) Command

This command finds the *first* difference and stops.

#### 🚀 Practical Example from Scratch

1.  **Create three files.** Two will be identical, and one will be different.

    ```bash
    # Create file 1
    hashim@Hashim:~$ echo "Hello, this is file 1." > file1.txt
    hashim@Hashim:~$ echo "This line is the same." >> file1.txt

    # Create file 2 (identical to file 1)
    hashim@Hashim:~$ echo "Hello, this is file 1." > file2.txt
    hashim@Hashim:~$ echo "This line is the same." >> file2.txt

    # Create file 3 (different from file 1)
    hashim@Hashim:~$ echo "Hello, this is file 1." > file3.txt
    hashim@Hashim:~$ echo "This line is different." >> file3.txt
    ```

2.  **Compare the identical files:**

    ```bash
    hashim@Hashim:~$ cmp file1.txt file2.txt
    hashim@Hashim:~$ echo $?
    0
    ```

      * **📜 Explanation:** `cmp` found no differences, so it printed **nothing**. We check the exit status with `echo $?`, which prints `0` (success/identical).

3.  **Compare the different files:**

    ```bash
    hashim@Hashim:~$ cmp file1.txt file3.txt
    file1.txt file3.txt differ: byte 40, line 2
    hashim@Hashim:~$ echo $?
    1
    ```

      * **📜 Explanation:** `cmp` immediately stopped at the first difference and reported its location: **byte 40, line 2**. The exit status `1` confirms the files are different.

### `diff` (Difference) Command

This command finds *all* differences and displays them.

#### 🚀 Practical Example from Scratch

We will use the same files (`file1.txt` and `file3.txt`) from the previous example.

1.  **Run the `diff` command:**

    ```bash
    hashim@Hashim:~$ diff file1.txt file3.txt
    2c2
    < This line is the same.
    ---
    > This line is different.
    ```

2.  **📜 Code Explanation:**

      * `2c2`: This is `diff`'s notation. It means "Line 2 in file 1 (`2c`) needs to be **changed** to match line 2 in file 2 (`c2`)."
      * `< This line is the same.`: The `<` symbol indicates a line from **file 1**.
      * `---`: This is a separator.
      * `> This line is different.`: The `>` symbol indicates a line from **file 2**.
        This output clearly shows you *what* needs to be changed to make the files identical.

### `comm` (Common) Command

The `comm` command is useful for comparing **sorted** files. It outputs three columns:

  * **Column 1:** Lines unique to `file1`.
  * **Column 2:** Lines unique to `file2`.
  * **Column 3:** Lines common to both files.

#### 🚀 Practical Example from Scratch

1.  **Create two sorted files.**

    ```bash
    hashim@Hashim:~$ echo -e "apple\nbanana\ncherry" > fruit1.txt
    hashim@Hashim:~$ echo -e "apple\ncherry\ngrapes" > fruit2.txt
    ```

2.  **Run the `comm` command:**

    ```bash
    hashim@Hashim:~$ comm fruit1.txt fruit2.txt
    banana
    		grapes
    	apple
    	cherry
    ```

3.  **📜 Code Explanation:**

      * `banana`: This is in **Column 1** (indented 0 tabs). It is unique to `fruit1.txt`.
      * `grapes`: This is in **Column 2** (indented 1 tab). It is unique to `fruit2.txt`.
      * `apple` and `cherry`: These are in **Column 3** (indented 2 tabs). They are common to both files.

4.  **Suppressing columns:** You can use options to suppress columns:

      * `-1`: Suppresses column 1 (unique to file1)
      * `-2`: Suppresses column 2 (unique to file2)
      * `-3`: Suppresses column 3 (common lines)

5.  **Example: Show only lines common to both files**

    ```bash
    hashim@Hashim:~$ comm -12 fruit1.txt fruit2.txt
    apple
    cherry
    ```

      * **📜 Code Explanation:** By using `-12`, we suppressed both Column 1 and Column 2, leaving only Column 3 (the common lines).

---

# 🧩 **The Parts of a Filename**

### `basename` and `dirname`

  * `basename`: "Strips" the path information from a filename, printing only the filename.
  * `dirname`: Strips the `basename` from a filename, printing only the path information.

> **Note:** These commands can operate on any arbitrary string. The argument does not need to refer to an existing file.

#### 🚀 Practical Example from Scratch

1.  **Set a variable:**

    ```bash
    hashim@Hashim:~$ my_path="/usr/local/bin/my_script.sh"
    ```

2.  **Use `basename` to get the file:**

    ```bash
    hashim@Hashim:~$ basename $my_path
    my_script.sh
    ```

      * **📜 Explanation:** It removed the entire path `/usr/local/bin/`.

3.  **Use `dirname` to get the path:**

    ```bash
    hashim@Hashim:~$ dirname $my_path
    /usr/local/bin
    ```

      * **📜 Explanation:** It removed the filename `my_script.sh`.

4.  **Using `basename $0` in a script:**
    The construction `basename $0` is often used to get the name of the currently executing script. This is helpful for "usage" messages if a script is called with missing arguments.

5.  **Create a simple script:**

    ```bash
    hashim@Hashim:~$ nano check_args.sh

    # Inside nano, add these lines:
    #!/bin/bash
    if [ "$#" -ne 2 ]; then
      echo "Usage: `basename $0` user_id password"
      exit 1
    fi
    echo "Processing arguments..."

    # Save and exit nano
    ```

6.  **Make the script executable and run it with no arguments:**

    ```bash
    hashim@Hashim:~$ chmod +x check_args.sh
    hashim@Hashim:~$ ./check_args.sh
    Usage: check_args.sh user_id password
    ```

      * **📜 Code Explanation:**
          * `$0` holds the name of the script *as it was called* (e.g., `./check_args.sh`).
          * `` `basename $0` `` runs `basename` on `./check_args.sh`, which cleans it up to just `check_args.sh`.
          * This makes the error message clear and dynamic, no matter how the script was called.


## 📜 The `strings` Command

The `strings` command displays any printable strings (sequences of text characters) that it finds inside a binary or data file (a non-text file).

#### 🚀 Practical Example from Scratch

We will look for text inside the `/bin/ls` executable program.

1.  **Run the `strings` command:**

    ```bash
    hashim@Hashim:~$ strings /bin/ls
    ```

2.  **Observe the output (a few sample lines):**

    ```
    $FreeBSD: src/bin/ls/cmp.c,v 1.12 2002/06/30 05:13:54 obrien Exp $
    @(#) Copyright (c) 1989, 1993, 1994
    The Regents of the University of California. All rights reserved.
    $FreeBSD: src/bin/ls/ls.c,v 1.66 2002/09/21 01:28:36 wollman Exp $
    $FreeBSD: src/bin/ls/print.c,v 1.57 2002/08/29 14:29:09 keramida Exp $
    $FreeBSD: src/bin/ls/util.c,v 1.38 2005/06/03 11:05:58 dd Exp $
    \"\"
    @(#)PROGRAM:ls PROJECT:file_cmds-264.50.1
    COLUMNS
    1@ABCFGHLOPRSTUWabcdefghiklmnopqrstuvwx
    bin/ls
    Unix2003
    ```

3.  **📜 Code Explanation:**

      * `/bin/ls` is a compiled program, not a text file. If you `cat` it, you see unreadable garbage.
      * `strings` scans that binary file and prints only the sequences of human-readable characters.
      * This is useful for quickly finding information like copyright notices, error messages (`COLUMNS`), or other plain text data embedded within a program.

---

# 🔐 **Working with File Permissions**

In a previous section, you used the `touch` command to create an empty file `abc` and then saw its long listing:

```bash
-rw-r--r-- 1 owner staff 0 Nov 2 17:12 abc
```

Each file in Bash contains a set of permissions for three different user groups: **the owner, the group, and the world (others)**.

The set of permissions are:

  * **r (read):** View the contents of a file.
  * **w (write):** Change or modify the contents of a file.
  * **x (execute):** Run the file as a program.

## File Permissions Explained

Let's break down the permission string `-rw-r--r--`:

| Character(s) | Group | Meaning |
| :--- | :--- | :--- |
| **`_`** | **File Type** | The first character. `_` is a file, `d` is a directory, `l` is a link. |
| **`rw_`** | **Owner** | The user who owns the file. In this case, they can **r**ead and **w**rite. |
| **`r__`** | **Group** | The group assigned to the file. In this case, group members can only **r**ead. |
| **`r__`** | **Others** | "The world." All other users. In this case, they can only **r**ead. |

## Permissions in Octal (Numeric) Mode

These permissions can also be represented by numbers in base-8 (octal).

| Number | Permission | Symbol |
| :--- | :--- | :--- |
| **4** | **read** | `r` |
| **2** | **write** | `w` |
| **1** | **execute** | `x` |

These values are added together for each user group.

  * **0**: (0+0+0) --- No permissions
  * **1**: (0+0+1) --x Execute only
  * **2**: (0+2+0) -w- Write only
  * **3**: (0+2+1) -wx Write and execute
  * **4**: (4+0+0) r-- Read only
  * **5**: (4+0+1) r-x Read and execute
  * **6**: (4+2+0) rw- Read and write
  * **7**: (4+2+1) rwx Read, write, and execute

For example, a file whose permissions are **755** indicates:

  * **Owner**: `7` (4+2+1) -\> `rwx` permissions
  * **Group**: `5` (4+0+1) -\> `r-x` permissions
  * **World**: `5` (4+0+1) -\> `r-x` permissions

## Changing Permissions with `chmod`

The `chmod` (change mode) command enables you to change permissions for files and directories. You can use it in two ways: **symbolic** (with letters) or **octal** (with numbers).

### 🚀 Practical Example: Symbolic Mode (`+x`, `u+x`)

Symbolic mode is good for adding or removing single permissions.

1.  **Create a script file:**

    ```bash
    hashim@Hashim:~$ echo '#!/bin/bash' > my_script.sh
    hashim@Hashim:~$ echo "echo 'Hello World'" >> my_script.sh
    ```

2.  **Check its permissions (it's not executable):**

    ```bash
    hashim@Hashim:~$ ls -l my_script.sh
    -rw-rw-r-- 1 hashim hashim 31 Nov  1 20:43 my_script.sh
    ```

3.  **Make it executable for *all* users:**
    The following command makes `my_script.sh` executable for all users.

    ```bash
    hashim@Hashim:~$ chmod +x my_script.sh
    hashim@Hashim:~$ ls -l my_script.sh
    -rwxrwxr-x 1 hashim hashim 31 Nov  1 20:43 my_script.sh
    ```

      * **📜 Code Explanation:** The `+x` adds the "execute" permission to the owner, group, and others.

4.  **Make another file executable *only* for the owner:**

    ```bash
    hashim@Hashim:~$ touch script2.sh
    hashim@Hashim:~$ chmod u+x script2.sh
    hashim@Hashim:~$ ls -l script2.sh
    -rwxrw-r-- 1 hashim hashim 0 Nov  1 20:45 script2.sh
    ```

      * **📜 Code Explanation:** `u+x` adds the "execute" permission for the **u**ser (owner) only.

### 🚀 Practical Example: Octal Mode (`644`, `444`)

Octal mode is good for setting the *exact* permissions all at once.

1.  **Create a file:**

    ```bash
    hashim@Hashim:~$ touch my_file.txt
    ```

2.  **Make it readable/writable for the owner and read-only for everyone else (`644`):**

    ```bash
    hashim@Hashim:~$ chmod 644 my_file.txt
    hashim@Hashim:~$ ls -l my_file.txt
    -rw-r--r-- 1 hashim hashim 0 Nov 1 17:32 my_file.txt
    ```

      * **📜 Code Explanation:**
          * `6` (Owner): `4+2` = `rw-`
          * `4` (Group): `4` = `r--`
          * `4` (Others): `4` = `r--`

3.  **Make the file read-only for *everyone* (`444`):**

    ```bash
    hashim@Hashim:~$ chmod 444 my_file.txt
    hashim@Hashim:~$ ls -l my_file.txt
    -r--r--r-- 1 hashim hashim 0 Nov 1 17:33 my_file.txt
    ```

      * **📜 Code Explanation:** Now, even you (the owner) cannot write to it.

4.  **Try to write to the read-only file (it will fail):**

    ```bash
    hashim@Hashim:~$ echo "test" >> my_file.txt
    bash: my_file.txt: Permission denied
    ```

### 🚀 Practical Example: Directory Permissions (`1777`, `000`)

1.  **Create a new directory:**

    ```bash
    hashim@Hashim:~$ mkdir my_dir
    ```

2.  **Revoke all permissions for the directory (`000`):**

    ```bash
    hashim@Hashim:~$ chmod 000 my_dir
    hashim@Hashim:~$ ls -ld my_dir
    d--------- 2 hashim hashim 4096 Nov 1 17:34 my_dir
    ```

      * **📜 Code Explanation:** The `d---------` shows that no one (except the `root` user) can read, write, or even enter this directory.
      * **Test it:**
        ```bash
        hashim@Hashim:~$ cd my_dir
        bash: cd: my_dir: Permission denied
        ```

3.  **Provide "sticky bit" permissions (`1777`):**
    This permission is special. It is often used on shared folders (like `/tmp`). It gives everyone read, write, and execute permissions (`777`), but the `1` (the "sticky bit") means that a user can only delete files that *they* own.

    ```bash
    hashim@Hashim:~$ chmod 1777 my_dir
    hashim@Hashim:~$ ls -ld my_dir
    drwxrwxrwt 2 hashim hashim 4096 Nov 1 17:34 my_dir
    ```

      * **📜 Code Explanation:** The permissions are now `rwxrwxrwt`. The `t` at the end signifies the "sticky bit" is active.

### 🚀 Practical Example: The SUID Bit (`u+s`)

The SUID (Set owner User ID) bit is an advanced permission. It **does not apply to shell scripts**, only to binary executables. When set, it allows an ordinary user to execute the program with the same privileges as the **owner** of the file (often `root`).

1.  **Find a binary program (like `passwd`):**

    ```bash
    hashim@Hashim:~$ ls -l /usr/bin/passwd
    -rwsr-xr-x 1 root root 63560 May 13 2024 /usr/bin/passwd
    ```

      * **📜 Code Explanation:** Notice the `s` in the owner's permissions (`rws`). This is the SUID bit. It allows you (a normal user) to run the `passwd` command, which needs to modify the protected `/etc/shadow` file (owned by `root`). You are temporarily running it *as* `root`.

2.  **Set the SUID bit on a program (as an example):**

    ```bash
    # (You would need 'root' privileges to do this)
    $ sudo chmod u+s /usr/bin/my_program
    ```

# 🚀 Practical Example: The SUID Bit (`u+s`)

Here is a complete, practical example from scratch that demonstrates the power of the SUID bit.

**Our Goal:** We will write a simple C program that tries to read the `/etc/shadow` file. This file contains protected password hashes and is **only readable by the `root` user**.

We will see it fail, then we will set the SUID bit and watch it succeed.

## 1\. Create the C Program

First, create a new file named `read_shadow.c` using `nano`.

```bash
hashim@Hashim:~$ nano read_shadow.c
```

Inside the `nano` editor, paste the following C code:

```c
#include <stdio.h>
#include <stdlib.h>

/*
  This program will attempt to open and read the first line
  of the /etc/shadow file.
*/
int main() {
    FILE *file_pointer;
    char line[256];

    printf("Attempting to open /etc/shadow...\n");

    // Try to open the protected file for reading ("r")
    file_pointer = fopen("/etc/shadow", "r");

    // Check if the file pointer is NULL, which means fopen() failed
    if (file_pointer == NULL) {
        perror("Error opening file");
        printf("\nREASON: We are running as a normal user. (This failure is expected)\n");
        return 1;
    }

    // If we get here, fopen() was successful!
    printf("\nSUCCESS: We have root-level permissions!\n");
    printf("--- First line of /etc/shadow ---\n");

    // Read and print the first line from the file
    if (fgets(line, sizeof(line), file_pointer) != NULL) {
        printf("%s", line);
    }

    printf("--- End of file snippet ---\n");

    // Close the file and exit
    fclose(file_pointer);
    return 0;
}
```

Save and exit `nano` (Ctrl+O, Enter, Ctrl+X).

## 2\. Compile the Program

Now, we will use `gcc` (the C compiler) to turn our source code into an executable program.

```bash
hashim@Hashim:~$ gcc read_shadow.c -o read_shadow
```

### 📜 Code Explanation

  * `gcc`: This is the command for the GNU C Compiler.
  * `read_shadow.c`: This is the input source file.
  * `-o read_shadow`: The `-o` flag specifies the **o**utput filename. We are naming our final program `read_shadow`.

## 3\. Test as a Normal User (Expected Failure)

Let's check the permissions and try to run our new program.

```bash
# Check the owner: it's you (hashim)
hashim@Hashim:~$ ls -l read_shadow
-rwxrwxr-x 1 hashim hashim 16304 Nov  1 20:59 read_shadow

# Now, run it
hashim@Hashim:~$ ./read_shadow
Attempting to open /etc/shadow...
Error opening file: Permission denied

REASON: We are running as a normal user. (This failure is expected)
```

As expected, the program fails. The operating system correctly blocked our program (running as user `hashim`) from reading the `root`-only file.

## 4\. Change Owner and Set SUID (as `root`)

Now, we will use `sudo` to become the superuser and modify the file's permissions.

```bash
# 1. Change the owner of the file to 'root'
hashim@Hashim:~$ sudo chown root read_shadow

# 2. Set the SUID bit
hashim@Hashim:~$ sudo chmod u+s read_shadow
```

### 📜 Code Explanation

  * `sudo chown root ...`: This command **ch**anges the **own**er of the file to `root`.
  * `sudo chmod u+s ...`: This **ch**anges the **mod**e. It **adds (`+`)** the **S**UID bit for the **u**ser (owner) of the file.

## 5\. Verify the New Permissions

Let's look at the file's permissions *now*. This is the most important part.

```bash
hashim@Hashim:~$ ls -l read_shadow
-rwsrwxr-x 1 root hashim 16304 Nov  1 20:59 read_shadow
```

Look closely at the output:

  * **`root`**: The owner is now `root`.
  * **`-rwsr-xr-x`**: The owner's execute permission (`x`) has been replaced with an **`s`**. This **`s`** confirms that the SUID bit is set.

## 6\. Test as a Normal User (Success\!)

Now for the final test. We are still the normal user `hashim`, but we are going to run the program that is *owned by `root`* and has the SUID bit.

```bash
hashim@Hashim:~$ ./read_shadow
Attempting to open /etc/shadow...

SUCCESS: We have root-level permissions!
--- First line of /etc/shadow ---
root:$y$j9T$THISISNOTAREALHASH...:/root:/bin/bash
--- End of file snippet ---
```

It worked\!

### 💡 Why Did It Work?

  * When you (`hashim`) ran the program, the operating system saw the **SUID bit (`s`)**.
  * It said, "This is a special program. Even though `hashim` is running it, I will grant it the privileges of its **owner** (`root`) *while it is running*."
  * Your program's **Real User ID** was still `hashim`, but its **Effective User ID** became `root`.
  * The `fopen()` function used the *Effective User ID* (`root`), which had permission to read `/etc/shadow`, and the operation succeeded.

This is the power (and significant security risk) of the SUID bit.

## 📇 Changing Owner, Permissions, and Groups

### 🚀 Practical Example: `chown` and `chgrp`

You must often use `sudo` to run these commands, as you cannot give away files you don't own, and you can only change groups to one you belong to.

1.  **Create a file (it will be owned by you):**

    ```bash
    hashim@Hashim:~$ touch file.txt
    hashim@Hashim:~$ ls -l file.txt
    -rw-r--r-- 1 hashim hashim 0 Nov 1 17:40 file.txt
    ```

2.  **Change the owner (`chown`):**
    *(This command would typically be run by `root` to assign a file to another user.)*

    ```bash
    hashim@Hashim:~$ sudo chown root file.txt
    [sudo] password for hashim:
    hashim@Hashim:~$ ls -l file.txt
    -rw-r--r-- 1 root hashim 0 Nov 1 17:41 file.txt
    ```

      * **📜 Code Explanation:** The owner has been changed from `hashim` to `root`.

3.  **Change the group (`chgrp`):**
    *(Again, this may require `sudo`.)*

    ```bash
    hashim@Hashim:~$ sudo chgrp root file.txt
    hashim@Hashim:~$ ls -l file.txt
    -rw-r--r-- 1 root root 0 Nov 1 17:42 file.txt
    ```

      * **📜 Code Explanation:** The group has been changed from `hashim` to `root`.

4.  **Using the `-R` (Recursive) Option:**
    If you have a directory, you can change the owner/group of *everything inside it* using `-R`.

    ```bash
    hashim@Hashim:~$ mkdir my_project
    hashim@Hashim:~$ touch my_project/file_A.txt
    hashim@Hashim:~$ sudo chown -R hashim my_project
    ```

      * **📜 Code Explanation:** This command changes the owner to `hashim` for the `my_project` directory *and* for `file_A.txt` inside it.

## 🎭 The `umask` and `ulimit` Commands

### `umask` (User Mask)

Whenever you create a file in Bash, the environment variable `umask` contains the **complement** (base-8) of the default permissions. It "masks" or removes permissions from the default.

1.  **Check your `umask`:**

    ```bash
    hashim@Hashim:~$ umask
    0022
    ```

      * **📜 Code Explanation:** The typical value is `0022`. We only care about the last three digits: `022`.

2.  **Calculate the defaults:**

      * System default for new **Directories**: `777` (rwxrwxrwx)
      * System default for new **Files**: `666` (rw-rw-rw-)

    Now, we subtract the `umask` (`022`):

      * **New Directory**: `777` - `022` = **`755`** (`rwxr-xr-x`)
      * **New File**: `666` - `022` = **`644`** (`rw-r--r--`)

3.  **Test the default permissions:**

    ```bash
    # Create a new file
    hashim@Hashim:~$ touch test_file_umask
    hashim@Hashim:~$ ls -l test_file_umask
    -rw-r--r-- 1 hashim hashim 0 Nov 1 17:45 test_file_umask

    # Create a new directory
    hashim@Hashim:~$ mkdir test_dir_umask
    hashim@Hashim:~$ ls -ld test_dir_umask
    drwxr-xr-x 2 hashim hashim 4096 Nov 1 17:45 test_dir_umask
    ```

      * **📜 Code Explanation:** As calculated, the new file is `644` (`rw-r--r--`) and the new directory is `755` (`rwxr-xr-x`).

### `ulimit` (User Limit)

The `ulimit` command specifies the maximum size of a file you can create, among other resource limits.

1.  **Check the file size limit:**

    ```bash
    hashim@Hashim:~$ ulimit -f
    unlimited
    ```

      * **📜 Code Explanation:** The `-f` flag checks for the maximum file size. In this case, it is `unlimited`.

2.  **Check all limits:**

    ```bash
    hashim@Hashim:~$ ulimit -a
    core file size          (blocks, -c) 0
    data seg size           (kbytes, -d) unlimited
    scheduling priority             (-e) 0
    file size               (blocks, -f) unlimited
    pending signals                 (-i) 62963
    max locked memory       (kbytes, -l) 65536
    max memory size         (kbytes, -m) unlimited
    open files                      (-n) 1024
    pipe size            (512 bytes, -p) 8
    POSIX message queues     (bytes, -q) 819200
    real-time priority              (-r) 0
    stack size              (kbytes, -s) 8192
    cpu time               (seconds, -t) unlimited
    max user processes              (-u) 62963
    virtual memory          (kbytes, -v) unlimited
    file locks                      (-x) unlimited
    ```

      * **📜 Code Explanation:** The `-a` (all) flag shows all the resource limits currently applied to your shell session.

---

# 📁 **Working with Directories**

A **directory** is a special type of file that stores filenames and related information. All files (whether they are ordinary files, special files, or other directories) are contained within directories.

## 🌳 The Directory Tree

Unix-based file systems have a **hierarchical structure** for organizing files and directories, often referred to as a **directory tree**.

This tree has a single root node, represented by the **slash character (`/`)**. All other directories are contained below it. The position of any file or directory within this hierarchy is described by its **pathname**.

Elements of a pathname are separated by a slash (`/`) character.

## 🗺️ Absolute and Relative Pathnames

A **pathname** is the unique "address" of a file or directory.

  * **Absolute Path:** A pathname is **absolute** if it is described in relation to the root (`/`). Absolute pathnames **always begin with a `/`**. They work from any location.

      * `/etc/passwd`
      * `/users/oac/ml/class`
      * `/dev/rdsk/Os5`

  * **Relative Path:** A pathname is **relative** if it is described in relation to your **current working directory**. Relative pathnames begin with a directory name, `./` (current directory), or `../` (parent directory).

      * `docs/report.txt` (if `docs` is a folder in your current location)
      * `./docs/report.txt` (same as above)
      * `../../Music` (two levels up, then into the `Music` folder)

## 🧭 Navigating Directories

### 🏠 Going to the Home Directory

You can navigate to your personal home directory with any of these commands:

  * `cd $HOME` (uses the `$HOME` environment variable)
  * `cd ~` (uses the `~` tilde shortcut)
  * `cd` (with no arguments, defaults to home)

> **Note:** In Windows, the `cd` command by itself shows you the current directory; it does *not* navigate to the home directory.

#### 🚀 Practical Example from Scratch

Let's see all three methods in action.

```bash
# 1. Start by moving to a different directory
hashim@Hashim:~$ cd /tmp
hashim@Hashim:/tmp$ pwd
/tmp

# 2. Use 'cd $HOME' to go home
hashim@Hashim:/tmp$ cd $HOME
hashim@Hashim:~$ pwd
/home/hashim

# 3. Go back to /tmp, then use 'cd ~'
hashim@Hashim:~$ cd /tmp
hashim@Hashim:/tmp$ cd ~
hashim@Hashim:~$ pwd
/home/hashim

# 4. Go back to /tmp, then use 'cd'
hashim@Hashim:~$ cd /tmp
hashim@Hashim:/tmp$ cd
hashim@Hashim:~$ pwd
/home/hashim
```

### 🧑‍💼 Going to Another User's Home

The tilde (`~`) always indicates a home directory. If you want to go to *another* user’s home directory, you can use `cd ~username`.

```bash
# This command would navigate to the home directory of a user named 'jsmith'
cd ~jsmith
```

### ⬅️ Going to the Previous Directory

You can navigate back to the location you were in *before* you navigated to the current directory using `cd -`.

#### 🚀 Practical Example from Scratch

```bash
# 1. Start in your home directory
hashim@Hashim:~$ pwd
/home/hashim

# 2. Move to /etc
hashim@Hashim:~$ cd /etc
hashim@Hashim:/etc$ pwd
/etc

# 3. Use 'cd -' to go back (to /home/hashim)
hashim@Hashim:/etc$ cd -
/home/hashim
hashim@Hashim:~$ pwd
/home/hashim

# 4. Use 'cd -' again to go back (to /etc)
hashim@Hashim:~$ cd -
/etc
hashim@Hashim:/etc$ pwd
/etc
```

## 📋 Listing and Finding Directories

### `pwd` (Print Working Directory)

To determine your current directory, use the `pwd` command.

```bash
$ pwd
```

The output will be something like this:

```bash
/Users/owner/Downloads
```

### `ls` (List Contents)

Display the contents of a directory with the `ls` command.

#### 🚀 Practical Example from Scratch

```bash
$ ls /usr/bin
```

A partial listing of the output from the preceding command is here:

```bash
fc
fddist
fdesetup
fg
fgrep
file
```

All the built-in executables (commands) that are discussed in this book reside in the `/usr/bin` directory.

## ➕ Creating Directories (`mkdir`)

### Creating Single or Multiple Directories

If you specify multiple directories on the command line, `mkdir` creates each of them.

#### 🚀 Practical Example from Scratch

This command creates the directories `docs` and `pub` as siblings under the current directory.

```bash
# 1. Create the directories
hashim@Hashim:~$ mkdir docs pub

# 2. List the contents to verify
hashim@Hashim:~$ ls
docs  pub
```

### Creating Nested Directories (`mkdir -p`)

Compare the preceding command with the following command that creates the directory `test` and a sub-directory `data` inside it.

The `-p` (parents) option forces the creation of missing intermediate directories (if any). If an intermediate directory does not exist and you *don't* use `-p`, `mkdir` will issue an error.

#### 🚀 Practical Example from Scratch (The Error)

Suppose that the intermediate sub-directory `accounting` does not exist in `/tmp`.

```bash
hashim@Hashim:~$ mkdir /tmp/accounting/test
mkdir: Failed to make directory "/tmp/accounting/test";
No such file or directory
```

  * **📜 Code Explanation:** The command failed because `/tmp/accounting` does not exist, so it cannot create `test` inside it.

#### 🚀 Practical Example from Scratch (The Solution)

By adding `-p`, we tell `mkdir` to create any parent directories it needs.

```bash
hashim@Hashim:~$ mkdir -p /tmp/accounting/test

# Verify that it worked
hashim@Hashim:~$ ls /tmp/accounting
test
```

  * **📜 Code Explanation:** `mkdir` first created `/tmp/accounting` and then created `test` inside it.

You can also use the `-p` option to create a deep sub-directory structure in your `$HOME` directory, as shown here:

```bash
mkdir -p $HOME/a/b/c/new-directory
```

  * **📜 Code Explanation:** This command creates the following subdirectories if any of them do not already exist:
      * `$HOME/a`
      * `$HOME/a/b`
      * `$HOME/a/b/c`
      * `$HOME/a/b/c/new-directory`

## ➖ Removing Directories

### `rmdir` (Remove *Empty* Directories)

You can delete **empty** directories using the `rmdir` command.

```bash
rmdir dirname1
```

You can delete multiple empty directories at a time as follows:

```bash
rmdir dirname1 dirname2 dirname3
```

The `rmdir` command produces no output if it is successful.

#### 🚀 Practical Example from Scratch (The Error)

`rmdir` will fail if a directory is not empty.

```bash
# 1. Create a directory and put a file in it
hashim@Hashim:~$ mkdir not_empty_dir
hashim@Hashim:~$ touch not_empty_dir/file.txt

# 2. Try to remove it with rmdir
hashim@Hashim:~$ rmdir not_empty_dir
rmdir: failed to remove 'not_empty_dir': Directory not empty
```

### `rm -r` (Remove *Non-Empty* Directories)

If there are files in the `dirname1` directory, you must use `rm -r` (recursive).

```bash
# This command will delete the directory and ALL contents inside it
rm -r dirname1
```

Alternatively, you can first remove the files in the sub-directory and then remove the directory itself:

```bash
# 1. Go into the directory
cd dirname1

# 2. Forcibly remove all files/folders inside
rm -rf *

# 3. Go back up to the parent directory
cd ../

# 4. Now that it's empty, remove it with rmdir
rmdir dirname1
```

## 🚚 Moving and Renaming Directories (`mv`)

The `mv` (move) command can be used to rename a directory, just like renaming a file.

### Renaming a Directory

The syntax for renaming a directory is the same as renaming a file:

```bash
mv olddir newdir
```

#### 🚀 Practical Example from Scratch

```bash
# 1. Create a directory
hashim@Hashim:~$ mkdir dir_to_rename

# 2. Rename it
hashim@Hashim:~$ mv dir_to_rename new_dir_name

# 3. Verify the change
hashim@Hashim:~$ ls
new_dir_name
```

### Moving a Directory

You can also move a directory into another directory.

#### 🚀 Practical Example from Scratch

```bash
# 1. Create a source and a destination
hashim@Hashim:~$ mkdir source_folder
hashim@Hashim:~$ mkdir destination_folder

# 2. Move 'source_folder' *into* 'destination_folder'
hashim@Hashim:~$ mv source_folder destination_folder

# 3. Verify the move
hashim@Hashim:~$ ls destination_folder
source_folder
```

### The "noclobber" Safety Feature

However, if the target directory *already exists* and *contains at least one file*, then the `mv` command will fail. Bash provides this "noclobber" (no-overwrite) feature for non-empty directories, which prevents you from accidentally merging or overwriting an existing directory.

#### 🚀 Practical Example from Scratch (The Error)

Let's use the example from the text.

```bash
# 1. Create a directory in /tmp and put a file in it
hashim@Hashim:~$ mkdir /tmp/abcd
hashim@Hashim:~$ touch /tmp/abcd/red

# 2. Create a directory with the *same name* in our current location
hashim@Hashim:~$ mkdir abcd

# 3. Try to move our 'abcd' directory to '/tmp'
hashim@Hashim:~$ mv abcd /tmp
```

Since the directory `/tmp/abcd` is not empty, you will see the following error message:

```bash
mv: rename abcd to /tmp/abcd: Directory not empty
```

  * **📜 Code Explanation:** The command failed because it saw that `/tmp/abcd` already exists and is not empty, so it refused to overwrite it.

---

# 💬 **Using Quote Characters**

There are three main types of quote characters in Bash, and they are not interchangeable. Each serves a distinct purpose:

  * **Single Quotes (`'`)**
  * **Double Quotes (`"`)**
  * **Backquotes (`` ` ``)**


## 📜 Single Quotes (`'`)

Single quotes are the most powerful type of quote. They preserve the **literal** value of every character inside them.

  * The shell will **not** perform variable substitution.
  * The shell will **not** interpret metacharacters (like `*`, `?`, or `$`).
  * The shell will **not** perform command substitution.

### 🚀 Practical Example from Scratch

1.  **Set a variable and create some files:**

    ```bash
    # Set a variable
    hashim@Hashim:~$ my_var="Hello World"

    # Create files for a globbing test
    hashim@Hashim:~$ touch a.txt b.txt c.txt
    ```

2.  **Run `echo` without quotes (for comparison):**

    ```bash
    hashim@Hashim:~$ echo $my_var and all these files: *
    Hello World and all these files: a.txt b.txt c.txt
    ```

      * **📜 Code Explanation:** The shell performed **variable substitution** (changing `$my_var` to `Hello World`) and **globbing** (expanding `*` to `a.txt b.txt c.txt`).

3.  **Now, run the same command with single quotes:**

    ```bash
    hashim@Hashim:~$ echo '$my_var and all these files: *'
    $my_var and all these files: *
    ```

      * **📜 Code Explanation:** The single quotes prevented all substitutions. The shell treated every character literally, printing the dollar sign and the asterisk exactly as they were typed.

The example `echo '$PATH'` from the text works the same way: it prints the literal string `$PATH` instead of the value of your `PATH` variable.

## 💬 Double Quotes (`"`)

Double quotes are less restrictive. They prevent word splitting and globbing but *still allow* variable substitution and command substitution.

  * Characters are interpreted literally, **except for** `$`, `` ` ``, and `\` (backslash).
  * The shell **will** perform variable substitution (e.g., `$my_var` becomes its value).
  * The shell will **not** perform globbing (e.g., `*` remains a literal `*`).
  * This is the most common type of quote, as it safely handles variables that may contain spaces.

### 🚀 Practical Example from Scratch

We will use the same variable and files from the previous example.

1.  **Run `echo` with double quotes:**
    ```bash
    hashim@Hashim:~$ echo "$my_var and all these files: *"
    Hello World and all these files: *
    ```
      * **📜 Code Explanation:**
          * **`"$my_var"`**: Variable substitution **happened**. `$my_var` was replaced with `Hello World`.
          * **`"*"`**: Globbing was **prevented**. The `*` was treated as a literal asterisk, not as a wildcard for `a.txt b.txt c.txt`.

## \` (Backquotes)

Backquotes are used for **command substitution**. This is an older syntax, but it's important to recognize.

  * The shell executes whatever command is inside the backquotes *first*.
  * The **standard output** of that command then replaces the entire backquoted section.
  * This is often used to store the result of a command in a variable.

### 💡 Modern Alternative: `$(...)`

The modern, recommended syntax for command substitution is `$(...)`. It is cleaner and can be nested (e.g., `$(cmd1 $(cmd2))`).

### 🚀 Practical Example from Scratch

Let's use the example from the text: counting files in your home directory.

1.  **First, let's run the inner command by itself:**
    This command lists all files in your home directory (`ls ~`) and pipes (`|`) that list to `wc -l` (word count - lines) to count them.

    ```bash
    hashim@Hashim:~$ ls ~ | wc -l
           22
    ```

      * **📜 Code Explanation:** For this example, let's assume you have 22 files and folders in your home directory. The output of this command is the number `22`.

2.  **Now, use that command inside an `echo` with backquotes:**

    ```bash
    hashim@Hashim:~$ echo "My home directory contains `ls ~ | wc -l` files."
    My home directory contains 22 files.
    ```

      * **📜 Code Explanation:**
        1.  The shell saw the backquotes and first executed `ls ~ | wc -l`.
        2.  That command produced the output `22`.
        3.  The shell **substituted** `` `ls ~ | wc -l` `` with its output (`22`).
        4.  The final command given to `echo` was `"My home directory contains 22 files."`.


## \\ (Backslash)

The backslash is the **escape character**. It is not a quote, but it serves a similar purpose for a *single character*.

  * It makes the character immediately following it **literal**.
  * It ignores any special metacharacter meaning for that single character.

### 🚀 Practical Example from Scratch

1.  **Escaping a `$` inside double quotes:**
    Double quotes normally expand variables. What if you want to print the literal text `$my_var`?

    ```bash
    hashim@Hashim:~$ echo "The variable is: \$my_var"
    The variable is: $my_var
    ```

      * **📜 Code Explanation:** The backslash `\` told the shell to treat the `$` as a literal character, not as the start of a variable.

2.  **Escaping a `*` without quotes:**

    ```bash
    hashim@Hashim:~$ echo *
    a.txt b.txt c.txt

    hashim@Hashim:~$ echo \*
    *
    ```

      * **📜 Code Explanation:** The `\*` told the shell to treat the `*` as a literal asterisk, not as a wildcard for globbing.

---

# 🌊 **Streams and Redirection Commands**

In Bash, every command you run automatically uses three special data streams. These streams (or "file descriptors") are how a command receives and sends information.

Here are the three standard streams:

| File Descriptor | Name | Symbol | Default |
| :--- | :--- | :--- | :--- |
| **0** | **Standard Input** | `stdin` | Your keyboard |
| **1** | **Standard Output** | `stdout` | Your terminal screen |
| **2** | **Standard Error** | `stderr` | Your terminal screen |

**Redirection** is the process of changing where these streams point. You can redirect output from the screen to a file, or get input from a file instead of the keyboard.

## Redirecting Output (`>` and `>>`)

You can control where the normal output (`stdout`) of a command goes.

  * **`>` (Overwrite):** Redirects `stdout` to a file. If the file exists, **it will be completely overwritten.**
  * **`>>` (Append):** Redirects `stdout` to a file. If the file exists, the new output will be **added to the end of it.**

### 🚀 Practical Example (Overwrite vs. Append)

1.  **Create a simple file list:**
    ```bash
    hashim@Hashim:~$ ls /usr/bin/python*
    /usr/bin/python3
    /usr/bin/python3.11
    ```
2.  **Redirect (Overwrite) with `>`:**
    ```bash
    hashim@Hashim:~$ ls /usr/bin/python* > python_list.txt
    ```
    *(No output appears on the screen.)*
3.  **Check the file's contents:**
    ```bash
    hashim@Hashim:~$ cat python_list.txt
    /usr/bin/python3
    /usr/bin/python3.11
    ```
4.  **Run a new command with `>`:**
    ```bash
    hashim@Hashim:~$ echo "This is a new line." > python_list.txt
    ```
5.  **Check the file again (it's overwritten):**
    ```bash
    hashim@Hashim:~$ cat python_list.txt
    This is a new line.
    ```
6.  **Run a new command with `>>` (Append):**
    ```bash
    hashim@Hashim:~$ echo "This line is appended." >> python_list.txt
    ```
7.  **Check the file (it's added to the end):**
    ```bash
    hashim@Hashim:~$ cat python_list.txt
    This is a new line.
    This line is appended.
    ```

## Redirecting Input (`<`)

You can tell a command to get its input (`stdin`) from a file instead of waiting for you to type.

### 🚀 Practical Example

1.  **Run `wc -l` (word count - lines) by itself:**

    ```bash
    hashim@Hashim:~$ wc -l
    (The cursor just blinks, waiting for you to type...)
    ```

    It's waiting for keyboard input. You would have to type lines and press **Ctrl+D** to end.

2.  **Use `<` to feed it a file:**
    We'll use the `python_list.txt` file we just made.

    ```bash
    hashim@Hashim:~$ wc -l < python_list.txt
    2
    ```

      * **📜 Code Explanation:** The `<` symbol "fed" the contents of `python_list.txt` into the `wc -l` command. The command counted the lines in the file (2) and exited immediately.

## Advanced Redirection: Working with `stderr`

The numbers `0`, `1`, and `2` are the file descriptors for `stdin`, `stdout`, and `stderr`.

  * `cat $abc 1>std1 2>std2`: This redirects `stdout` (stream 1) to a file named `std1` and `stderr` (stream 2) to a file named `std2`.
  * `cmd > file`: This is just a shortcut for `cmd 1>file`.

### The Bit Bucket: `/dev/null`

The "directory" `/dev/null` is a special file. Anything you redirect to this "null bit bucket" is **essentially discarded forever**. This is useful when you want to run a command and safely ignore its output or error messages.

### 🚀 Practical Example (Separating `stdout` and `stderr`)

Let's run a command that we know will produce *both* normal output and an error. `ls` is perfect for this.

1.  **Run `ls` on a real file and a fake file:**

    ```bash
    hashim@Hashim:~$ ls /bin/bash /fake/file
    ls: cannot access '/fake/file': No such file or directory
    /bin/bash
    ```

      * **📜 Code Explanation:** As you can see, both the error (`stderr`) and the successful output (`stdout`) are printed to your screen.

2.  **Now, let's redirect them to separate files:**

    ```bash
    hashim@Hashim:~$ ls /bin/bash /fake/file 1>output.txt 2>errors.txt
    ```

    *(Nothing is printed to the screen.)*

3.  **Check the files:**

    ```bash
    # Check the normal output file:
    hashim@Hashim:~$ cat output.txt
    /bin/bash

    # Check the error file:
    hashim@Hashim:~$ cat errors.txt
    ls: cannot access '/fake/file': No such file or directory
    ```

      * **📜 Code Explanation:** `1>output.txt` captured the `stdout` stream, and `2>errors.txt` captured the `stderr` stream.

## Merging Streams (`2>&1`)

Sometimes you want to capture *everything* (both `stdout` and `stderr`) into a single file, in the order they appear. To do this, you redirect one stream into the other.

The syntax `2>&1` means "redirect stream 2 (`stderr`) to the same place as stream 1 (`stdout`)".

**IMPORTANT:** The order of redirection matters\!

### 🚀 Practical Example (The *Correct* Way)

This command sends `stdout` and `stderr` to one file.

```bash
hashim@Hashim:~$ ls /bin/bash /fake/file > all_output.txt 2>&1
```

1.  **Check the file:**

    ```bash
    hashim@Hashim:~$ cat all_output.txt
    ls: cannot access '/fake/file': No such file or directory
    /bin/bash
    ```

2.  **📜 Code Explanation (How it works):**
    The shell reads this from left to right:

    1.  `> all_output.txt`: First, `stdout` (stream 1) is pointed to the file `all_output.txt`.
    2.  `2>&1`: Second, `stderr` (stream 2) is pointed to "the same place as stream 1" (`&1`). Since stream 1 is pointing to `all_output.txt`, stream 2 is *also* pointed to `all_output.txt`.

### 🚀 Practical Example (The *Wrong* Way)

Note that the following command **will not work** as expected. The error message is displayed on the screen.

```bash
hashim@Hashim:~$ ls /bin/bash /fake/file 2>&1 > wrong_output.txt
ls: cannot access '/fake/file': No such file or directory
```

1.  **Check the file:**

    ```bash
    hashim@Hashim:~$ cat wrong_output.txt
    /bin/bash
    ```

    *Only the normal output was captured\!*

2.  **📜 Code Explanation (Why it failed):**
    The shell reads this from left to right:

    1.  `2>&1`: First, `stderr` (stream 2) is pointed to "the same place as stream 1" (`&1`). At this exact moment, stream 1 is still pointing to its default: the **screen**.
    2.  `> wrong_output.txt`: *After* that, `stdout` (stream 1) is redirected to the file `wrong_output.txt`.
    3.  **Result:** `stderr` goes to the screen, and `stdout` goes to the file.


## Suppressing Error Messages

You can redirect error messages to `/dev/null` if you are certain they can be safely ignored.

### 🚀 Practical Example

1.  **Run `ls` on a non-existent directory (error appears):**

    ```bash
    hashim@Hashim:~$ ls /temp
    ls: cannot access '/temp': No such file or directory
    ```

2.  **Run `ls` again, but redirect `stderr` to `/dev/null`:**

    ```bash
    hashim@Hashim:~$ ls /temp 2>/dev/null
    ```

    *(Nothing is printed. The command is silent.)*

3.  **📜 Code Explanation:**

      * `ls /temp`: This command ran and produced an error.
      * `2>/dev/null`: This instruction caught the error from `stderr` (stream 2) and sent it to `/dev/null`, where it was discarded. `stdout` (stream 1) was unaffected and would have printed to the screen if there had been any.

---