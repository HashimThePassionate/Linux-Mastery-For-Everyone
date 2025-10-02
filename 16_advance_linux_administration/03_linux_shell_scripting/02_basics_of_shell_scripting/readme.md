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

# 🧩 **Variables in Shell Scripts**

We have already introduced variables, and now it's time to learn how to use them inside a script. Let's quickly recap the types of variables used in Linux.

### 📚 Types of Variables

  * **Environment Variables**: These are available system-wide from the shell (e.g., `HOME`, `USER`). They are available to scripts automatically.
  * **User-Defined Variables**: These are created by users and are typically only available within the script or shell session where they are defined.

You can see a complete list of environment variables on your system by using the `printenv` and/or `set` commands.

-----

### 🏷️ Understanding Naming Conventions

Inside the Linux shell, the system’s environment variables use **only uppercase letters**. You should consider this when creating your own variables to avoid conflicts.

Here are some key points about naming variables:

  * Names are case-sensitive (`myvar` is different from `MyVar`).
  * Names should be up to 20 characters in length.
  * To assign a value, use the equals (`=`) sign with no spaces around it (e.g., `myvar=hello`).

Since system variables are always uppercase, it is a good practice to avoid using all-caps for your own variable names. This prevents you from accidentally overwriting an important system variable. We advise you to consider one of the following rules when creating your variable names:

  * ✅ Use only **lowercase letters**, underscores, and numbers (e.g., `first_name`, `user_id_2`).
  * ✅ **Capitalize** the first letter of each word in the variable name (e.g., `FirstName`, `UserId`).

Now, let’s learn how to define and use variables inside a shell script.

-----

### 🌍 Defining and Using Environment Variables

Let’s create a new file called `user-script.sh` that will show relevant user information by using existing environment variables. After we create the file and enter the code, we will make it executable and then run it.

#### Code Snippet

```bash
hashim@Hashim:~$ touch user-script.sh

hashim@Hashim:~$ nano user-script.sh

hashim@Hashim:~$ cat user-script.sh 
#!/bin/bash
echo "user id: $UID"
echo "user name: $USER"
echo "user's home: $HOME"
echo "user session: $BASH"

hashim@Hashim:~$ chmod u+x user-script.sh 

hashim@Hashim:~$ ./user-script.sh 
user id: 1000
user name: hashim
user's home: /home/hashim
user session: /bin/bash
```

#### Explanation

1.  We created the script file, added the commands, and made it executable with `chmod u+x user-script.sh`.
2.  Inside the script, we used the `echo` command with **double quotes (`"`)**. When you use double quotes, the shell replaces the variable name (like `$UID`) with its actual value.
3.  **Important**: If you were to use **single quotes (`'`)** (e.g., `echo 'user id: $UID'`), the shell would not replace the variable. It would print the literal string `$UID` to the screen.
4.  We used four different environment variables to show information about the user: `UID`, `USER`, `HOME`, and `BASH`.

-----

### ✍️ Using Your Own Variables

You can also define and use your own variables inside a script. A useful feature of the shell is that it can automatically determine the data type (like a number or a string) a variable is using.

You should also know that the values of variables defined inside a shell script are **temporary**. They are only active while the shell is running the script, and they will be lost afterward.

Let’s create a new shell script and use our own variables this time.

#### Code Snippet

```bash
hashim@Hashim:~$ nano user-variable.sh

hashim@Hashim:~$ cat user-variable.sh 
#!/bin/bash
value=25
product='shirt'
echo "The $product cost $value $"

hashim@Hashim:~$ chmod u+x user-variable.sh 

hashim@Hashim:~$ ./user-variable.sh 
The shirt cost 25 $
```

#### Explanation

1.  Here, we created a file called `user-variable.sh`.
2.  Inside the script, we defined two of our own variables:
      * `value=25`: A variable named `value` with a numeric value of `25`.
      * `product='shirt'`: A variable named `product` with a string value of `shirt`.
3.  When we called the variables inside the `echo` command, we used the same dollar sign (`$`) prefix as we did for environment variables.
4.  The script then printed the sentence, substituting `$product` and `$value` with their assigned values.

---

# 🧮 **Using Mathematical Expressions in Shell Scripts**

Being a programming language, the shell has built-in features to work with numbers. The Bash shell provides the `expr` command, which is used for different mathematical operations. To learn about all the operations that are supported, you can visit the internal manual page for the `expr` command.

```bash
man expr
```

We will show you how to use some of the operations that `expr` provides. Keep in mind that you may need to "escape" some characters (by using a backslash `\`) when using the `expr` command, as they might otherwise be misinterpreted by the shell.

Let’s create a new script file called `math.sh` to perform some basic mathematical operations.

-----

### 🔢 Method 1: The `expr` Command

This is the traditional way to perform math in shell scripts.

#### Code Snippet

```bash
hashim@Hashim:~$ nano math.sh

hashim@Hashim:~$ cat math.sh 
#!/bin/bash
price=20
vat=2
total=$(expr $price + $vat)
echo "The total price is: $total"

hashim@Hashim:~$ chmod u+x math.sh 

hashim@Hashim:~$ ./math.sh 
The total price is: 22
```

#### Explanation

  * `total=$(expr $price + $vat)`: This is the line that performs the calculation.
      * `$(...)`: This is **command substitution**. It tells the shell to execute the command inside the parentheses and substitute the command's output in its place.
      * `expr $price + $vat`: The `expr` command evaluates the mathematical expression. It takes the values from the `$price` and `$vat` variables and adds them together. The result (`22`) is then assigned to the `total` variable.

-----

### 🔢 Method 2: Arithmetic Expansion `$[...]`

Besides the `expr` command, we can also use square brackets, which is a much simpler way to perform mathematical operations. Let’s modify the script to use this method.

#### Code Snippet

```bash
hashim@Hashim:~$ cat math.sh 
#!/bin/bash
price=20
vat=2
total=$[($price+$vat)]
echo "The total price is: $total"

hashim@Hashim:~$ ./math.sh 
The total price is: 22
```

#### Explanation

  * `total=$[($price+$vat)]`: This syntax is a form of **arithmetic expansion**. The `$[...]` tells Bash to perform the calculation inside the brackets. This method is often easier to read and write than `expr`.
      * **Note**: A more modern and widely recommended syntax for arithmetic expansion is `total=$(($price + $vat))`.

-----

### 小数点 Handling Floating-Point Numbers with `bc`

A major limitation of the shell's built-in math features (`expr` and arithmetic expansion) is that they only support **integer arithmetic**. If you try to use a floating-point value (a number with a decimal, like `2.5`), the script will show an error.

The most common way to overcome this limitation is by using the **Bash Calculator**, or the `bc` command.

> #### 💡 Important Note
>
> If you need full support for floating-point operations directly in your shell, you might want to consider using the **Z shell (Zsh)**. It is installed by default in some Linux distributions (like Manjaro and Kali Linux) and on macOS.

Let’s see how we can use the `bc` command inside our `math.sh` script to handle decimal numbers.

#### Code Snippet

```bash
hashim@Hashim:~$ nano math.sh

hashim@Hashim:~$ cat math.sh 
#!/bin/bash
price=20
vat=2.50
total=$(echo "scale=2; $price + $vat" | bc )
echo "The total price is: $total"

hashim@Hashim:~$ ./math.sh 
The total price is: 22.50
```

#### Explanation

In this example, we assigned a floating-point value to the `vat` variable and used the `bc` command to calculate the total.

  * `total=$(echo "scale=2; $price + $vat" | bc )`: This line performs the floating-point calculation.
      * `echo "scale=2; $price + $vat"`: This part creates a string of instructions for the `bc` command.
          * `scale=2`: This is a special option for `bc` that sets the number of decimal places to use in the result.
          * `;`: This separates the option from the actual calculation.
          * `$price + $vat`: The mathematical expression we want to compute.
      * `|`: The **pipe** sends the output of the `echo` command (the instruction string) as input to the `bc` command.
      * `bc`: The Bash Calculator reads the input string, performs the high-precision calculation, and prints the result. The result is then captured by command substitution and stored in the `total` variable.

Creating Bash shell scripts involves more than just math operations or running a sequence of commands. Sometimes, decisions need to be made inside the shell, depending on the input provided and the output expected.


---