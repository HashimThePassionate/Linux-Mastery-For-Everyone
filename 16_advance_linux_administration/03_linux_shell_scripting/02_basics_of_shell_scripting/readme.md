# 📜 **Creating a Shell Script File**

The most appropriate way to write scripts is to create them in a file, called a **shell script file**. In Linux, it's a good practice for these files to use the `.sh` extension for clarity. However, this is not mandatory because Linux file types are not determined by their extensions, unlike in Windows.

The most important characteristic that makes a file a script is the very first line of text inside it. For a Bash shell script, this line must be in the following format:

```bash
#!/bin/bash
```

### The Shebang (`#!`)

When the file is opened and executed, this first line tells the system's interpreter that it is a script file that should be run by the Bash shell (located at `/bin/bash`). If you were using a different shell (like `zsh` or `ksh`), this line would point to its location.

Normally, the hashtag (`#`) is used to start a commented line that the shell ignores. However, this very first line is a special exception. The `#!` combination, also called the **shebang**, is a directive to the system's program loader.

-----

### ✍️ Creating a Basic Script

Let’s create a basic script file.

#### Code Snippet

```bash
hashim@Hashim:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  snap  Templates  Videos

hashim@Hashim:~$ nano basic-scripts.sh

hashim@Hashim:~$ cat basic-scripts.sh 
#!/bin/bash
whoami
who
date
uptime
```

#### Explanation

1.  `ls`: We list the contents of the home directory.
2.  `nano basic-scripts.sh`: We use the `nano` text editor to create and edit a new file named `basic-scripts.sh`.
3.  `cat basic-scripts.sh`: After saving and exiting `nano`, we use the `cat` command to display the contents of our new script file.
      * `#!/bin/bash`: The shebang, telling the system to use Bash.
      * `whoami`: A command that prints the current user's username.
      * `who`: A command that shows who is currently logged into the system.
      * `date`: A command that displays the current date and time.
      * `uptime`: A command that shows how long the system has been running.

You could write all the commands on a single line separated by semicolons (`;`), but using different lines for each command is much clearer and easier to read.

-----

### 🏃‍♂️ Running the Script and Fixing Errors

Now that we have our first script, let’s run it. We will try to run it by simply invoking its name at the command line.

#### Problem 1: Command Not Found

```bash
hashim@Hashim:~$ ls -la basic-scripts.sh 
-rw-rw-r-- 1 hashim hashim 35 Oct  2 12:16 basic-scripts.sh

hashim@Hashim:~$ basic-script.sh
basic-script.sh: command not found
```

You might be wondering why we get this error. It is because the shell does not know where your script is located. It cannot find the file inside its `PATH` variable. To overcome this, we have two options:

1.  We can add the directory where our script resides to the shell’s `PATH`.
2.  We can use a relative or absolute path when we run the script.

#### 🛠️ Examples for Both Options

  * **Option 1: Add directory to `PATH` (Temporary)**
    You can add the current directory (`.`) to your `PATH` for the current session.

    ```bash
    # Add the current directory (.) to the PATH variable
    $ export PATH="$PATH:."
    $ chmod u+x basic-scripts.sh

    # Now you can run the script by name
    $ basic-scripts.sh 
    ```

  * **Option 2: Use a Relative Path**
    This is the more common and convenient method. A relative path specifies the location of a file relative to the current directory.

    ```bash
    # Use ./ to specify the current directory
    $ ./basic-scripts.sh
    ```

#### Problem 2: Permission Denied

We will use the second method. Let’s invoke the script file with its location. We will get another error, this time saying `Permission denied`.

This is because we do not have permission to execute the file. By default, when you create a new file, it only gets read and write permissions for the owner. To fix this, we need to make the file executable using the `chmod` command.

```bash
# Add 'execute' permission ('x') for the 'user' ('u')
$ chmod u+x basic-scripts.sh
```

#### ✅ Successful Execution

Once we’ve set the executable permission on the file, we can run it again. This time, the script will be executed successfully.

```bash
hashim@Hashim:~$ ./basic-scripts.sh
bash: ./basic-scripts.sh: Permission denied

hashim@Hashim:~$ chmod u+x basic-scripts.sh

hashim@Hashim:~$ ./basic-scripts.sh
hashim
hashim   :0           2025-10-02 11:51 (:0)
hashim   pts/0        2025-10-02 11:52 (:0)
Thu Oct  2 12:21:49 PM PKT 2025
 12:21:49 up 30 min,  2 users,  load average: 0.31, 0.25, 0.35
```

As shown in the output, the script was executed, and every command inside it ran in order:

  * `hashim`: This is the output of the `whoami` command.
  * The next two lines showing logged-in users are from the `who` command.
  * `Thu Oct 2 12:21:49 PM PKT 2025`: This is the output of the `date` command.
  * `12:21:49 up 30 min...`: This is the output of the `uptime` command.

> #### ⭐ Important Note on Good Practice
>
> As a general rule, when writing scripts, you should use as many comments as possible to detail each variable and parameter you choose. This is considered good programming practice and will make your code more enjoyable to write and more relevant. Documenting your coding steps will make your scripts easier to read at a later time, both for yourself and for anyone else who might come across your code.


---