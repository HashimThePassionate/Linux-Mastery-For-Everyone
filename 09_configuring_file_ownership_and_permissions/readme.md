# 🔐 **File Permissions, Compression in Linux**

In Linux, every file and directory has a security system that is based on "permissions" and "ownership." This system decides who can access the file and how—whether they can only read it, edit it, or run it (like a program). These permissions are divided into three categories:

1. 👤 **Owner/User**: This is the person who created the file. As I mentioned earlier, the one who creates the file becomes its owner. The owner has the most control.
2. 👥 **Group**: This is a group of users created for a team or related people. The owner decides which group can access this file.
3. 🌍 **Other/World**: These are all the people who are neither the owner nor members of the group. That means all other users of the system fall into this category.

## 📋 **Types of Permissions and Their Meanings**
There are three permissions for each category, represented by alphabets:
- 📖 **r (Read)**: This means reading the file. If someone has read permission, they can see the content of the file but cannot edit it.
- ✏️ **w (Write)**: This means writing or making changes to the file. If this permission exists, the user can add or delete something in the file.
- ⚙️ **x (Execute)**: This means running the file. This is mostly for scripts or executable programs, like a command or software.
- ❌ **- (No Permission)**: If there is no permission, a dash (-) appears there.

## 🔍 **Format of Permissions**
When you run the `ls -l` command (like `ls -l` in the home directory),
- **First character**: This indicates the type of file or directory:
  - `d`: It is a directory (folder).
  - `-`: It is a normal file (like a text file).
  - `l`: It is a symbolic link (like a shortcut that points to a file).
  - `p`: Named pipe (for program communication).
  - `s`: Socket (for network communication).
  - `b`: Block device (hardware like a hard disk).
  - `c`: Character device (like a keyboard).

- **Next 9 characters**: These are for permissions and are divided into three groups:
  - **First 3 characters**: Permissions for the owner.
  - **Second 3 characters**: Permissions for the group.
  - **Third 3 characters**: Permissions for other/world.

**Example**:  
If the permission string is: `-rwxr-xr-x`  
- The first `-`: This indicates that it is a normal file.
- Owner: `rwx` - The owner has all permissions: read, write, and execute.
- Group: `r-x` - The group has only read and execute, no write.
- Other: `r-x` - Other people also have read and execute, no write.

### **Explanation with a Real Example**
Let’s look at a practical example. If you run this command:  
`root@7e56c80bcd77:/home/packt# ls -l`  
The output might look something like this:  
```
total 32
drwxr-xr-x 3 root root 4096 Apr 29 16:01 backup_dir1
-rw-r--r-- 1 root root   34 May  3 14:30 new-report
lrwxrwxrwx 1 root root   10 May 13 14:50 new-report-link -> new-report
```
- `drwxr-xr-x`: This is a directory (starts with d).  
  - Owner (`rwx`): Read, write, execute - This means the owner can enter the directory, create files, and run something.
  - Group (`r-x`): Read and execute - Group members can view and enter the directory but cannot create anything new.
  - Other (`r-x`): Read and execute - Other people can also view and enter.

- `-rw-r--r--`: This is a normal file (starts with -).  
  - Owner (`rw-`): Read and write - The owner can read and edit the file but cannot execute it.
  - Group (`r--`): Only read - Group members can only view the file.
  - Other (`r--`): Only read - Other people can also only view it.

### **Permissions in Octal Numbers**
Now let’s understand how all this converts into numbers. In Linux, permissions are also represented in "octal" (8-base) numbers, which is very easy. Each permission has a value based on the power of 2:
- **r (read)** = 2² = 4
- **w (write)** = 2¹ = 2
- **x (execute)** = 2⁰ = 1
- **- (no permission)** = 0

For each category, we add these values:
- If `rwx` is present: 4 (r) + 2 (w) + 1 (x) = **7**
- If `r-x` is present: 4 (r) + 0 (no w) + 1 (x) = **5**
- If `rw-` is present: 4 (r) + 2 (w) + 0 (no x) = **6**
- If `---` is present: 0 + 0 + 0 = **0**

**Example**:  
Convert the permission `-rwxr-xr-x` into octal:  
- Owner: `rwx` = 4 + 2 + 1 = 7  
- Group: `r-x` = 4 + 0 + 1 = 5  
- Other: `r-x` = 4 + 0 + 1 = 5  
**Octal Value**: 755

### **Commands to Manage Permissions**
Now let’s also understand how to change permissions:
1. **chown**: To change the owner of a file.  
   Example: `chown newuser file.txt` - This makes "newuser" the owner of file.txt.
2. **chgrp**: To change the group of a file.  
   Example: `chgrp newgroup file.txt` - This makes "newgroup" the group of file.txt.
3. **chmod**: To change permissions.  
   Example: `chmod 755 file.txt` - This sets the permissions of the file to 755 (rwxr-xr-x).
4. **umask**: This sets the default permissions when a new file is created. (For details, refer to Chapter 4).

---

## **Exercise: Convert Permissions to Octal**
Now let’s solve the exercise you provided. We will convert each permission into octal, step-by-step:

1. **rwx rwx ---**  
   - Owner: `rwx` = 4 + 2 + 1 = 7  
   - Group: `rwx` = 4 + 2 + 1 = 7  
   - Other: `---` = 0 + 0 + 0 = 0  
   **Octal**: 770

2. **rwx r-x ---**  
   - Owner: `rwx` = 7  
   - Group: `r-x` = 4 + 0 + 1 = 5  
   - Other: `---` = 0  
   **Octal**: 750

3. **rwx r-x - - -**  
   - Owner: `rwx` = 7  
   - Group: `r-x` = 5  
   - Other: `---` = 0  
   **Octal**: 750  
   (Note: " - - -" should be understood as `---`.)

4. **rwx - - - - - -**  
   - Owner: `rwx` = 7  
   - Group: `---` = 0  
   - Other: `---` = 0  
   **Octal**: 700

5. **rw- rw- rw-**  
   - Owner: `rw-` = 4 + 2 = 6  
   - Group: `rw-` = 6  
   - Other: `rw-` = 6  
   **Octal**: 666

6. **rw- rw- r - -**  
   - Owner: `rw-` = 6  
   - Group: `rw-` = 6  
   - Other: `r--` = 4  
   **Octal**: 664

7. **rw- rw- - - -**  
   - Owner: `rw-` = 6  
   - Group: `rw-` = 6  
   - Other: `---` = 0  
   **Octal**: 660

8. **rw- r- - r- -**  
   - Owner: `rw-` = 6  
   - Group: `r--` = 4  
   - Other: `r--` = 4  
   **Octal**: 644

9. **rw- r- - - - -**  
   - Owner: `rw-` = 6  
   - Group: `r--` = 4  
   - Other: `---` = 0  
   **Octal**: 640

10. **rw- - - - - - -**  
    - Owner: `rw-` = 6  
    - Group: `---` = 0  
    - Other: `---` = 0  
    **Octal**: 600

11. **r - - - - - - - -**  
    - Owner: `r--` = 4  
    - Group: `---` = 0  
    - Other: `---` = 0  
    **Octal**: 400

---

# 📦 **Commands for File Compression, Uncompression, and Archiving**

In Linux, the main tool for archiving files is called `tar`, which stands for "tape archive." This tool was initially used in Unix to write files to external tape devices for archiving. Nowadays, in Linux, this tool is also used to write to a file in a compressed format. Apart from tar archives, some other popular formats are `gzip` and `bzip` for compressed archives, along with the popular `zip` from Windows. Now let's look at the `tar` command in detail.

### 🔧 **Tar Command for Compressing and Uncompressing**
The `tar` command is used with options and does not offer compression by default. If we want compression, we need to use specific options. Here are some of the most useful arguments that can be used with `tar`:

| Option | Description |
|--------|-------------|
| `tar -c` | Creates an archive |
| `tar -r` | Appends files to an already existing archive |
| `tar -u` | Appends only the files that have changed to an existing archive |
| `tar -A` | Appends one archive to the end of another archive |
| `tar -t` | Lists the contents of the archive |
| `tar -x` | Extracts the archive contents |
| `tar -z` | Uses gzip compression for the archive |
| `tar -j` | Uses bzip2 compression for the archive |
| `tar -v` | Uses verbose mode by printing extra information on the screen |
| `tar -p` | Restores the original permissions and ownership for the extracted files |
| `tar -f` | Specifies the name of the output file |

> 💡 In your daily tasks, you will have to use these arguments in combination with each other.

### 📝 **Example: Creating an Archive**
Let's look at an example. If we want to create an archive of the `files` directory, we use the `-cvf` arguments combined. Check this out:

```bash
root@7e56c80bcd77:/home/packt# ls
backup_dir1  dir1   new-report       new-report-ln  users
development  files  new-report-link  reports
root@7e56c80bcd77:/home/packt# tar -cvf files-archive.tar files/
files/
files/files3
files/files6
files/files5
files/files2
files/files4
files/files1
root@7e56c80bcd77:/home/packt# ls
backup_dir1  dir1   files-archive.tar  new-report-link  reports
development  files  new-report         new-report-ln    users
```

Here:
- `-c`: Created a new archive.
- `-v`: Verbose mode showed the name of each file being added to the archive.
- `-f`: Specified the file name, which is `files-archive.tar`.
- ⚠️ But this archive is not compressed. It only collected the files in one place.

### 🗜️ **Adding Compression**
If we want compression, we need to add the `-z` or `-j` option. Now we will use the `-z` option, which provides the `gzip` compression algorithm. See the following example and compare the size of the two archive files:

```bash
root@7e56c80bcd77:/home/packt# ls
backup_dir1  dir1   files-archive.tar  new-report-link  reports
development  files  new-report         new-report-ln    users  
root@7e56c80bcd77:/home/packt# tar -czvf files-archive-gzipped.tar.gz files
files/
files/files3
files/files6
files/files5
files/files2
files/files4
files/files1
root@7e56c80bcd77:/home/packt# ls
backup_dir1  files                         new-report       reports
development  files-archive-gzipped.tar.gz  new-report-link  users     
dir1         files-archive.tar             new-report-ln
root@7e56c80bcd77:/home/packt# ls -l files-archive-gzipped.tar.gz files-archive.tar
-rw-r--r-- 1 root root   189 May 13 15:46 files-archive-gzipped.tar.gz
-rw-r--r-- 1 root root 10240 May 13 15:44 files-archive.tar
```

Here:
- `-z`: Added `gzip` compression.
- 📉 A new file `files-archive-gzipped.tar.gz` was created, with a size of only 189 bytes, while the previous `files-archive.tar` was 10240 bytes. This shows how much space compression can save.
- 💡 It's important to use an extension like `.tar.gz` so the file type is clear.

### 📂 **Uncompressing an Archive**
Now, if we want to open the archive (uncompress it), we use the `-x` option (mentioned at the beginning of this subsection). Let's uncompress the `files-archive.tar` file we created earlier and use the `-C` option to specify a target directory where the uncompressed files will go. This target directory needs to be created beforehand. Let's do this with the following commands:

```bash
root@7e56c80bcd77:/home/packt# mkdir uncompressed-directory
root@7e56c80bcd77:/home/packt# tar -xvf files-archive.tar -C uncompressed-directory
files/
files/files3
files/files6
files/files5
files/files2
files/files4
files/files1
root@7e56c80bcd77:/home/packt# ls
backup_dir1  files-archive-gzipped.tar.gz  new-report-ln
development  files-archive.tar             reports      
dir1         new-report                    uncompressed-directory     
files        new-report-link               users
```

And if we want to uncompress the compressed file `files-archive-gzipped.tar.gz`:

```bash
root@7e56c80bcd77:/home/packt# tar -xvzf files-archive-gzipped.tar.gz -C uncompressed-directory
files/
files/files3
files/files6
files/files5
files/files2
files/files4
files/files1
root@7e56c80bcd77:/home/packt# ls
backup_dir1  files-archive-gzipped.tar.gz  new-report-ln
development  files-archive.tar             reports
dir1         new-report                    uncompressed-directory     
files        new-report-link               users
```

Here:
- `-x`: Used for extraction.
- `-v`: Verbose mode showed the names of the files.
- `-z`: Handled the `gzip` compressed file.
- `-C`: Specified the target directory where the files will be extracted (`uncompressed-directory`).

### ⭐ **Additional Tips**
- 🔍 If you want to list what's inside the archive, run `tar -tvf files-archive.tar`. This will only show the contents without extracting them.
- 💾 Compression saves space, but you'll need the original space when extracting.
- 🏷️ Always use extensions like `.tar`, `.tar.gz`, or `.tar.bz2` so the file type is clear.

---
