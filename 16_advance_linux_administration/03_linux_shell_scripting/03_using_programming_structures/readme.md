# 💻 **Using Programming Structures**

In this section, we will demonstrate how to use **conditional** and **looping** statements. These can be invaluable when creating advanced shell scripts. We will also show you how to use arrays, how input reading is used inside scripts, and how to format and print data for the output.

---

## 🗃️ Using Arrays in Bash

We have shown you how to use variables in previous sections. Now, it is time to step up our game and show you how to make use of **arrays**, which are a more complex form of variable.

Let’s say that we need to work with multiple variables that store similar information, such as filenames. Instead of using multiple variables in the form of `filename1`, `filename2`, `filename3` … `filenameN`, we can create a single array that will hold all the filenames.

If you are familiar with other programming languages, arrays may already be familiar to you. But if you do not know any other programming languages, fear not, as Bash provides a simple way to use arrays.

### 1\. Indexed Arrays

Let’s start with an easy example. We need to work with different usernames. Instead of using different variables for each, we can use an **indexed array**:

```bash
usernames=("paul" "janet" "mike" "john" "anna" "martha")
```

The elements inside an array start from index number **0 (zero)**. This is important to remember when we need to access the array's contents. If we want to access the *third* element from the `usernames` array (the "mike" string), we must use the following code:

```bash
echo ${usernames[2]}
```

The output will be `mike` (without quotes).

To print out the **entire array**, use the following code:

```bash
echo ${usernames[*]}
# or
echo ${usernames[@]}
```

To print out the **size** of the array (the number of elements), use the following code:

```bash
echo ${#usernames[@]}
# or
echo ${#usernames[*]}
```

In our case, we have six elements, so the output will be `6`.

#### Adding and Modifying Elements

Let’s say we need to add a new username ("alex") to the array. There are different ways to add it. If we just want to add it to the end, we can use the following code:

```bash
usernames+=("alex")
```

The new username will be appended at the end of the array. At this point, the array contains unordered names. We will cover how to arrange them in alphabetical order in the *Using looping statements* section.

Alternatively, we can add or replace an element at a **specific position** inside the array (let’s say position 1) by using the following code:

```bash
usernames[1]="zack"
```

### 2\. The `declare` Command and Associative Arrays

So far, we’ve only used strings as examples. You can also use integers for indexed arrays.

The built-in command to create an array is `declare`. To create an indexed array, you can use:

```bash
declare -a array_name
```

You can also create **associative arrays** using this command:

```bash
declare -A array_name
```

Associative arrays are based on **key-value pairs**. The following is an example of an associative array declaration:

```bash
declare -A linux_distros=( [KDE]="openSUSE" [GNOME]="Fedora" [Xfce]="Debian" [Cinnamon]="Mint" )
```

  * The items in square brackets `[ ]` are the **keys**.
  * The items in double quotes `" "` are the **values**.

To print all the **values**, you can use the same command as for indexed arrays:

```bash
echo ${linux_distros[@]}
```

To print all the **keys**, use the following command (note the exclamation mark `!`):

```bash
echo ${!linux_distros[@]}
```

The main difference between indexed and associative arrays is that indexed arrays are based on an index value (0, 1, 2...), whereas associative arrays use specific keys to map to their values.

Arrays are important data structures in Bash. We will use them later when we discuss looping statements. But first, let’s learn how to read input data and format output data.

-----

### 🖥️ Full Code Example and Explanation

Here is the full sequence of commands as executed in the terminal.

```bash
hashim@Hashim:~$ usernames=("paul" "janet" "mike" "john" "anna" "martha")
hashim@Hashim:~$ echo ${usernames[2]}
mike
hashim@Hashim:~$ echo ${usernames[*]}
paul janet mike john anna martha
hashim@Hashim:~$ echo ${usernames[@]}
paul janet mike john anna martha
hashim@Hashim:~$ echo ${#usernames[@]} 
6
hashim@Hashim:~$ echo ${#usernames[*]} 
6
hashim@Hashim:~$ usernames+=("alex")
hashim@Hashim:~$ echo ${usernames[@]}
paul janet mike john anna martha alex
hashim@Hashim:~$ usernames[1]="zack"
hashim@Hashim:~$ echo ${usernames[@]}
paul zack mike john anna martha alex
hashim@Hashim:~$ declare -a array_name
hashim@Hashim:~$ declare -A array_name
bash: declare: array_name: cannot convert indexed to associative array
hashim@Hashim:~$ declare -A linux_distros=( [KDE]="openSUSE" [GNOME]="Fedora" [Xfce]="Debian" [Cinnamon]="Mint" )
hashim@Hashim:~$ echo ${linux_distros[@]}
openSUSE Mint Debian Fedora
hashim@Hashim:~$ echo ${!linux_distros[@]}
KDE Cinnamon Xfce GNOME
```

#### ✏️ Detailed Explanation

1.  `usernames=("paul" "janet" "mike" "john" "anna" "martha")`
      * This command creates a new **indexed array** named `usernames` and initializes it with six string elements.
2.  `echo ${usernames[2]}`
      * This prints the element at **index 2** (the third element, since indexing starts at 0). The output is `mike`.
3.  `echo ${usernames[*]}`
      * This prints all elements of the array, treated as a single word. The output is `paul janet mike john anna martha`.
4.  `echo ${usernames[@]}`
      * This also prints all elements of the array. When unquoted, it behaves just like `*`.
5.  `echo ${#usernames[@]}`
      * The `#` prefix is used to get the **length** (or size) of the array. The output is `6`.
6.  `echo ${#usernames[*]}`
      * This is an alternative syntax for getting the array length, which also outputs `6`.
7.  `usernames+=("alex")`
      * The `+=` operator **appends** a new element ("alex") to the end of the `usernames` array.
8.  `echo ${usernames[@]}`
      * This prints the array again to confirm the addition. The output now includes "alex" at the end.
9.  `usernames[1]="zack"`
      * This command **replaces** the element at **index 1** (which was "janet") with a new value, "zack".
10. `echo ${usernames[@]}`
      * This prints the array to confirm the change. The output shows "zack" in the second position.
11. `declare -a array_name`
      * This explicitly **declares** a new, empty indexed array (`-a`) named `array_name`.
12. `declare -A array_name`
      * This command attempts to re-declare the *same variable* (`array_name`) as an **associative array** (`-A`).
13. `bash: declare: array_name: cannot convert indexed to associative array`
      * The shell produces an **error**. You cannot change the type of an existing array from indexed to associative.
14. `declare -A linux_distros=( ... )`
      * A new **associative array** named `linux_distros` is successfully created with four key-value pairs.
15. `echo ${linux_distros[@]}`
      * This command prints all the **values** from the `linux_distros` array. Note that the output order is not guaranteed.
16. `echo ${!linux_distros[@]}`
      * The `!` prefix is used to print all the **keys** from the `linux_distros` array. The order is also not guaranteed.

---

# ⌨️ **Reading Input Data**

By default, the shell reads input data from the **standard input**, which is your keyboard. To read from the standard input, you can use the `read` command. This command reads all input data you provide until a new line is entered (which happens when you press the `Enter` key).

With the `read` command, you can provide one or more variables. If you use more variables, each word provided through standard input will be assigned to a variable in order.

-----

### 🖊️ `read` Command Examples

Here is an example of using `read` with one variable and then with multiple variables.

#### Code Snippet

```bash
hashim@Hashim:~$ read name
Muhammad Hashim
hashim@Hashim:~$ echo $name
Muhammad Hashim

hashim@Hashim:~$ read a b c d
monday
hashim@Hashim:~$ echo $a
monday
hashim@Hashim:~$ echo $b


hashim@Hashim:~$ read a b c d
mondey tuesday wednesday thursday
hashim@Hashim:~$ echo $a $b $c $d
mondey tuesday wednesday thursday
```

#### Detailed Explanation

1.  `read name`:

      * We run the `read` command with a single variable, `name`.
      * The shell waits for input. The user types `Muhammad Hashim` and presses `Enter`.
      * The entire line of text is assigned to the `name` variable.
      * `echo $name` confirms this by printing the full value.

2.  `read a b c d`:

      * We run `read` with four variables: `a`, `b`, `c`, and `d`.
      * The user types `monday` and presses `Enter`.
      * The shell assigns the first word (`monday`) to the first variable (`a`).
      * Since no more input was provided on that line, the remaining variables (`b`, `c`, `d`) are left empty.
      * `echo $a` prints `monday`, and `echo $b` prints an empty line.

3.  `read a b c d`:

      * We run the same command again.
      * This time, the user types `mondey tuesday wednesday thursday` and presses `Enter`.
      * The shell assigns the input word by word:
          * `mondey` goes to `$a`
          * `tuesday` goes to `$b`
          * `wednesday` goes to `$c`
          * `thursday` goes to `$d`
      * `echo $a $b $c $d` prints all four values, showing each variable received its relevant value.

-----

### 📂 Reading Input from a File

The `read` command has several options (which you can learn about in its manual: `man read`). It can also receive input from a file by using **input redirection** (the `<` symbol).

For example, if we have a file called `week-days`, we can redirect its content to the `read` command.

#### Code Snippet

```bash
hashim@Hashim:~$ nano week-days

hashim@Hashim:~$ cat week-days 
Monday Tuesday Wednesday Thursday Friday Saturday Sunday

hashim@Hashim:~$ read d1 d2 d3 d4 d5 d6 d7 < week-days 

hashim@Hashim:~$ echo $d1 $d2 $d3 $d4 $d5 $d6 $d7
Monday Tuesday Wednesday Thursday Friday Saturday Sunday
```

#### Detailed Explanation

1.  `nano week-days`: This command is used to create and edit a new file named `week-days`.
2.  `cat week-days`: This command displays the content of the file, which is a single line containing all seven days of the week.
3.  `read d1 d2 d3 d4 d5 d6 d7 < week-days`:
      * We run the `read` command with seven variables (`d1` through `d7`).
      * The `< week-days` part is the **input redirection**. It tells the shell to use the *contents of the `week-days` file* as the standard input for the `read` command, instead of waiting for keyboard input.
      * The `read` command takes the line from the file and assigns each word to the variables in order.
4.  `echo $d1 $d2 $d3 $d4 $d5 $d6 $d7`: This command prints the values of all seven variables, confirming that the data from the file was successfully "read" into them.

The `read` command is very useful for getting user input when creating scripts. We showed you how to use it on the command line, but it is a key component in more advanced scripts.

---

# 🖥️ **Formatting Output Data**

In Linux, the standard output is directed by default to your monitor. You have two main commands for this task:

1.  `echo`: We’ve used this command quite extensively.
2.  `printf`: This section will focus on this command.

-----

## 📢 The `printf` Command

The `printf` command is similar to the one used in the C programming language. It gives you much more control over the output format than `echo`.

A quick manual search (`man printf`) shows the basic syntax:

```bash
printf FORMAT [ARGUMENT] …
```

The command prints all the `[ARGUMENT]` values according to the rules defined in the `FORMAT` string. This format string can contain regular characters, **escape sequences**, and **format specifiers**.

### 1\. Escape Sequences

These are special backslash codes that represent non-printing characters. Here are some of the most widely used sequences:

| Sequence | Description |
| :---: | :--- |
| `\b` | Backspace |
| `\e` | Escape |
| `\f` | Form feed |
| `\n` | **New line** |
| `\r` | Carriage return |
| `\t` | **Horizontal tab** |
| `\v` | Vertical tab |

🔔 **Important**: When using a backslash with `printf` in the shell, it might need to be escaped by using double quotes or another backslash. Please refer to the manual for more details.

### 2\. Format Specifiers

In addition to escape sequences, `printf` uses format specifiers. These are placeholders that define how an argument should be formatted.

| Specifier | Description |
| :---: | :--- |
| `%s` | **String**: Used for basic string output. |
| `%b` | **String (with escapes)**: A string that allows for the interpretation of escape sequences. |
| `%d` | **Integer**: Used for integral (whole number) values. |
| `%f` | **Floating-point**: Used for floating-point (decimal) values. |
| `%x` | **Hexadecimal**: Used for the hexadecimal values of integers. |

-----

## 🚀 Using `printf`: Examples

Now that you know the basics, let’s look at some examples of how to use `printf`.

### Example 1: Basic Command-Line Usage

In the following example, we use `printf` to show the difference between a plain string and one using a format specifier.

#### Code Snippet

```bash
hashim@Hashim:~$ printf "Hello World"
Hello Worldhashim@Hashim:~$ printf "%s\n" "Hello World"
Hello World
hashim@Hashim:~$ printf "%b\n" "Muhammad" "Hashim"
Muhammad
Hashim
```

#### Explanation

1.  `printf "Hello World"`
      * This first command prints the string exactly as given. Notice that the shell prompt `hashim@Hashim:~$` appears immediately after "Hello World" on the same line. This is because **`printf` does not add a new line by default**.
2.  `printf "%s\n" "Hello World"`
      * This command is much cleaner. It uses:
          * `%s`: The string specifier, which acts as a placeholder for the argument "Hello World".
          * `\n`: The new line escape sequence, which moves the cursor to the next line after printing.
3.  `printf "%b\n" "Muhammad" "Hashim"`
      * This command shows how `printf` handles multiple arguments. The format string `"%b\n"` is applied to *each argument* ("Muhammad" and "Hashim") in order.
      * First, it prints "Muhammad" and adds a new line.
      * Second, it prints "Hashim" and adds another new line.

-----

### Example 2: Script with User Input

Let’s use the `printf` command inside a script to read and display user data.

#### Code Snippet

```bash
hashim@Hashim:~$ nano user-data.sh

hashim@Hashim:~$ cat user-data.sh 
#!/bin/bash
printf "Provide your first name:\n"
read FirstName
printf "Provide your last name:\n"
read LastName
printf "Welcome: %s\n" "$FirstName $LastName"

hashim@Hashim:~$ chmod u+x user-data.sh 

hashim@Hashim:~$ ./user-data.sh 
Provide your first name:
Muhammad
Provide your last name:
Hashim
Welcome: Muhammad Hashim
```

#### Detailed Explanation

This script reads two variables from the standard input (keyboard) and then shows both variables on the standard output (monitor).

1.  `nano user-data.sh`: Creates a new file.
2.  `cat user-data.sh`: Displays the contents of the script file.
3.  `#!/bin/bash`: The standard shebang line.
4.  `printf "Provide your first name:\n"`: This prompts the user. The `\n` is important here, as it moves the cursor to a new line so the user can type their input clearly.
5.  `read FirstName`: This command waits for the user to type something and press `Enter`. The input is stored in the `FirstName` variable.
6.  `printf "Provide your last name:\n"`: A second prompt for the last name.
7.  `read LastName`: The input is stored in the `LastName` variable.
8.  `printf "Welcome: %s\n" "$FirstName $LastName"`: This is the final output.
      * The `%s` is the placeholder for a string.
      * `"$FirstName $LastName"` is provided as the argument. The shell combines the two variables into a single string (e.g., "Muhammad Hashim"), which is then inserted into the `%s` position.
9.  `chmod u+x user-data.sh`: This command adds executable permission for the user (`u+x`).
10. `./user-data.sh`: This runs the script, resulting in the interactive session shown.

-----

### Example 3: Advanced Table Formatting

Now, let’s dig deeper into formatting. We can use escape sequences like `\t` (tab) and `\n` (new line) to mimic table formatting.

#### Code Snippet (Command-Line)

```bash
hashim@Hashim:~$ printf "%s\t%s\n" "No." "Item"  "1" "Paper"
No.	Item
1	Paper

hashim@Hashim:~$ printf "%s\t%s\n" "No." "Item"  "1" "Paper" "2" "Pen"
No.	Item
1	Paper
2	Pen
```

#### Explanation

The format string `"%s\t%s\n"` expects two string arguments. `printf` re-uses this format string for every pair of arguments it's given.

  * In the first command, it formats `"No."` and `"Item"`, then formats `"1"` and `"Paper"`.
  * In the second command, it does the same and then continues, formatting `"2"` and `"Pen"`.

This is useful, but we can create a much more complex and precisely aligned table using a script.

#### Code Snippet (Script: `format-output.sh`)

```bash
hashim@Hashim:~$ nano format-output.sh

hashim@Hashim:~$ cat format-output.sh 
#!/bin/bash
separator========================
header="\n %-10s %8s %10s %11s\n"
format=" %-10s %08d %10s %11.2f\n"
width=43
printf "$header" "PRODUCT" "ID" "ISLE" "PRICE"
printf "%${width}s\n" "$separator"
printf "$format" \
Eggs 2876 D02 10 \
Meat 8748 M05 58.75 \
Cereals 3243 C11 25.5

hashim@Hashim:~$ chmod u+x format-output.sh

hashim@Hashim:~$ ./format-output.sh 

 PRODUCT    ID      ISLE       PRICE
                       =======================
 Eggs       00002876       D02       10.00
 Meat       00008748       M05       58.75
 Cereals    00003243       C11       25.50
```

#### Detailed Script Explanation

Let’s detail the `format-output.sh` script.

  * `separator========================`

      * This is a regular shell variable holding a string of `=` characters to be used as a visual separator.

  * `header="\n %-10s %8s %10s %11s\n"`

      * This variable defines the format for the table's header row.
      * `\n`: Start with a new line.
      * `%-10s`: A string (`s`) that is **10 characters** wide and **left-aligned** (indicated by the `-`).
      * `%8s`: A string (`s`) that is **8 characters** wide and **right-aligned** (the default).
      * `%10s`: A string (`s`) that is **10 characters** wide and right-aligned.
      * `%11s`: A string (`s`) that is **11 characters** wide and right-aligned.
      * `\n`: End with a new line.

  * `format=" %-10s %08d %10s %11.2f\n"`

      * This variable defines the format for each *data row* in the table.
      * `%-10s`: A string (`s`), 10 characters wide, left-aligned (for the product name).
      * `%08d`: An integer (`d`), **8 characters** wide. The `0` indicates that if the number is less than 8 digits, it will be **padded with leading zeros** (e.g., `2876` becomes `00002876`).
      * `%10s`: A string (`s`), 10 characters wide, right-aligned (for the isle).
      * `%11.2f`: A floating-point number (`f`), 11 characters wide, with exactly **2 digits after the decimal point** (due to `.2`). This is why `10` becomes `10.00` and `25.5` becomes `25.50`.
      * `\n`: End with a new line.

  * `width=43`

      * A variable holding the total width for aligning the separator.

  * `printf "$header" "PRODUCT" "ID" "ISLE" "PRICE"`

      * This is the first `printf` command. It uses the `$header` format string and applies it to the four arguments ("PRODUCT", "ID", etc.), printing the formatted table header.

  * `printf "%${width}s\n" "$separator"`

      * This line prints the separator. It uses the `$width` variable to dynamically create the format string `"%43s\n"`. This prints the `$separator` string right-aligned within a 43-character-wide space.

  * `printf "$format" \`

      * This is the final `printf` call, which prints all the data rows. The backslash (`\`) at the end of the line is just to continue the command on the next line for readability.
      * It uses the `$format` variable as its format string and re-applies it to each group of four arguments.

As you can see, `printf` is a very versatile and powerful tool that gives you fine-grained control over your script's output. Now that you know the basic tools for input and output data formatting, we can proceed to other useful structures.

---

# 🚦 Understanding Exit Statuses and Testing Structures

To effectively use conditional (`if`) and looping (`for`, `while`) statements, it is essential to understand the **exit status** of a command. When a statement runs, the given condition is checked and tested so that we can see the command's validity.

The status of the most recently executed command is stored inside a special parameter: `$?`.

The question mark (`?`) is replaced by an integer that shows the command's status.

* ✅ **Success:** If the command's execution was successful, the value will be `0` (zero). The parameter will show `$0`.
* ❌ **Failure:** If the command's execution was not successful, the value can be any integer from `1` to `255`. Most regularly, the error number is `1`, so the parameter will be `$1`.

These values are also called **exit codes**.

***

### 🧱 Testing Structures

Alongside exit codes, **testing structures** are also important for conditional and looping statements in Bash. These structures are the building blocks for the statements mentioned. They are considered the shell’s keywords and are represented as follows:

* `[[ ]]` **Double square brackets**: Used to test the *true* or *false* status of a command; it can also perform operations on regular expressions.
* `(( ))` **Double parentheses**: Used for arithmetic operations.
* `test` **Test keyword**: This keyword is used to evaluate expressions such as strings, integers, and file properties.

> 💡 **Important Note**
>
> The syntax of Bash requires spaces to be used before and after the brackets. For example: `[ operation ]`.

***

### ⚙️ Conditional Operators

In conditional statements, other types of operators are used for testing. Let’s look at some of the most commonly used operators.

#### 1. Integer Tests

These operators are used for numerical comparisons.

| Operator | Description |
| :---: | :--- |
| `-eq` | The equality check operator |
| `-ne` | The inequality check operator |
| `-lt` | The less than operator |
| `-le` | The less than or equal to operator |
| `-gt` | The greater than operator |
| `-ge` | The greater than or equal operator |

#### 2. Argument Operators (Positional Parameters)

In addition, there are argument operators in Linux shell scripting:

* `$0`: This variable represents the command used to run the script.
* `$1` through `$n`: These represent the first through nth arguments passed to the command. For example, `$1` refers to the first argument, `$2` refers to the second argument, and so on.

Besides the operators shown in this section, basic mathematical operators are also currently used in conditional statements.

#### 3. String Tests

There are also testing structures for strings:

| Operator | Description |
| :---: | :--- |
| `=` | Tests if strings are identical. (`==` is also accepted) |
| `!=` | Tests if strings are NOT identical. |
| `\<` and `\>` | Less than and greater than are accepted for string comparison, but they **must be escaped** (using the backslash character `\`). |

#### 4. File Type Tests

These testing operators are in the form of options:

| Operator | Description |
| :---: | :--- |
| `-f` | Tests for a regular file. |
| `-d` | Tests for a directory. |
| `-h` or `-L` | Tests for a symbolic link. |
| `-e` | Tests for a file's existence. |

#### 5. Other Test Operators

Here are some other operators that are used for testing:

| Operator | Description |
| :---: | :--- |
| `-a` | The logical **AND**. |
| `-o` | The logical **OR**. |
| `-z` | To check if an input string was entered (i.e., if it is null or empty). |

Testing operators are complex and useful, so it’s very important to learn them.

---

# ❔ **Using Conditional `if` Statements**

Just like in any other programming language, Bash has conditional execution statements, such as `if-then-fi`, `if-then-else-fi`, and nested `if`. It also uses conditional operators such as `&&` (**AND**) and `||` (**OR**). We will show you how to use them in this section.

We will present the `if` statement in all its appearances (`if-then`, `if-then-else`, and nested `if`).

-----

### 1\. The `if-then-fi` Statement

In its most common form, the `if-then-fi` statement has the following syntax:

```bash
if [condition]
then
    commands
fi
```

This Bash `if` statement runs the `condition` after the `if` keyword. If the command or test is completed successfully—meaning it has an **exit status of zero**—then it will run the `commands` that are listed after the `then` keyword.

-----

### 2\. The `if-then-else-fi` Statement

The `if-then-else-fi` statement is similar to `if-then` and has the following syntax:

```bash
if [condition]
then
    commands
else
    commands
fi
```

Similar to the simpler `if-then` statement, the `condition` is run, and the results are different depending on its exit status.

  * If it completes successfully (exit status 0), the `commands` after the `then` keyword are executed.
  * If there is another exit status (a non-zero value), the `commands` after the `else` keyword are executed.

This gives you more options and alternatives based on the result of the condition. There are also situations when you need to check for more conditions inside a single `if-then` command; for this, you can use **nested `if` statements**.

-----

### 🔢 Example 1: Checking for Even or Odd Numbers

In this example, we will check if a number a user is typing is even or odd. The script will take the input from the user by using the `read` command. Then, it will check if the remainder from its division by two is zero or not. This is how it determines if the number is odd or even.

We will use the `if-then-else` statement for this example, together with the `read` and `printf` commands. The following screenshot shows the code and the execution’s output.

#### Code Snippet and Execution

```bash
hashim@Hashim:~$ nano even_odd.sh
hashim@Hashim:~$ cat even_odd.sh 
#!/bin/bash
echo "Enter a number:"
read number
if [ $((number % 2)) -eq 0 ]
then
    printf "%s\n" "The number $number is even."
else
    printf "%s\n" "The number $number is odd."
fi
hashim@Hashim:~$ chmod u+x even_odd.sh 
hashim@Hashim:~$ ./even_odd.sh 
Enter a number:
25
The number 25 is odd.
hashim@Hashim:~$ ./even_odd.sh 
Enter a number:
24
The number 24 is even.
```

#### ✏️ Detailed Explanation

1.  `nano even_odd.sh`: This command opens the `nano` text editor to create or edit a file named `even_odd.sh`.
2.  `cat even_odd.sh`: This command displays the contents of the `even_odd.sh` file after it has been saved.
3.  `#!/bin/bash`: This is the **shebang**. It tells the operating system to use the Bash interpreter to run this script.
4.  `echo "Enter a number:"`: This command prints the text "Enter a number:" to the screen, prompting the user for input.
5.  `read number`: This command waits for the user to type an input and press `Enter`. The value typed by the user is stored in the variable named `number`.
6.  `if [ $((number % 2)) -eq 0 ]`: This is the core conditional logic.
      * `$((number % 2))`: This is an **arithmetic expansion**. The modulo operator (`%`) calculates the *remainder* after dividing the value of the `number` variable by 2.
      * `[ ... -eq 0 ]`: This is the **test**. It checks if the result of the modulo operation (the remainder) is **eq**ual to `0`. If the remainder is 0, the test is true (exits with status 0).
7.  `then`: This block runs only if the `if` test was true (i.e., the remainder was 0).
8.  `printf "%s\n" "The number $number is even."`: This command prints a formatted string, indicating that the entered number is even.
9.  `else`: This block runs if the `if` test was false (i.e., the remainder was not 0).
10. `printf "%s\n" "The number $number is odd."`: This command prints a formatted string, indicating that the number is odd.
11. `fi`: This keyword marks the end of the `if` statement block.
12. `chmod u+x even_odd.sh`: This command modifies the file's permissions. It **a**dds (`+`) e**x**ecutable (`x`) permission for the **u**ser (`u`), which is necessary to run the script.
13. `./even_odd.sh`: This command executes the script.
14. `Enter a number: 25`: The user types `25`.
15. `The number 25 is odd.`: The script's output. Since 25 divided by 2 has a remainder of 1, the `else` block was executed.
16. `./even_odd.sh`: The script is run a second time.
17. `Enter a number: 24`: The user types `24`.
18. `The number 24 is even.`: The script's output. Since 24 divided by 2 has a remainder of 0, the `then` block was executed.

-----

### 📄 Example 2: Testing for a File

The following script checks if a filename introduced by the user is indeed a file by using the `test -f` operator. We will introduce the absolute path of the file we want to run a check on. The script is called `testing_file.sh`.

#### Script Creation

```bash
hashim@Hashim:~$ nano testing_file.sh
hashim@Hashim:~$ cat testing_file.sh 
#!/bin/bash
#testing if a filename is indeed a file
echo "Enter a file name using absolute path:"
read filename
if [ -z "$filename" ]
then
    printf "%s\n" "no filename entered..."
    exit 1
else
    printf "The filename you entered is: %s\n" "$filename"
fi
printf "%s\n" "Testing if \"$filename\" is a file..."
test -f "$filename"
if [ $? -eq 0 ]
then
    printf "%s\n" "The \"$filename\" represents a file. (0=true):" $?
else
    printf "%s\n" "The \"$filename\" is not a file. (1=false):" $?
fi
hashim@Hashim:~$ chmod u+x testing_file.sh
```

#### ✏️ Detailed Explanation

1.  `nano testing_file.sh`: Opens the `nano` editor for the script file.
2.  `cat testing_file.sh`: Displays the content of the script file.
3.  `#!/bin/bash`: The shebang, indicating the Bash interpreter.
4.  `#testing if...`: A comment describing the script's function.
5.  `echo "Enter a file name...:"`: Prompts the user to enter a full file path.
6.  `read filename`: Stores the user's input in the `filename` variable.
7.  `if [ -z "$filename" ]`: This is the first `if` statement. The `-z` operator tests if the string in `$filename` is **zero-length** (empty).
8.  `then`: This block runs if the user provided no input (pressed Enter).
9.  `printf "%s\n" "no filename entered..."`: Prints an error message.
10. `exit 1`: This command immediately **terminates the script** with an exit status of `1` (which signifies failure).
11. `else`: This block runs if the `$filename` variable is *not* empty.
12. `printf "The filename...: %s\n" "$filename"`: Prints the filename that the user entered.
13. `fi`: Closes the first `if` statement.
14. `printf "%s\n" "Testing if \"$filename\" is a file..."`: Prints a status message before performing the test.
15. `test -f "$filename"`: This is the **key command**. It does not print anything. It performs a test: does the path in `$filename` exist, and is it a **regular file** (`-f`)? It then sets the special `$?` (exit status) variable to `0` if true or `1` if false.
16. `if [ $? -eq 0 ]`: This second `if` statement **checks the exit status** of the *previous* command (`test -f`). It asks, "Was the exit status (`$?`) **eq**ual to 0?"
17. `then`: This block runs if the `test -f` command was successful (the file exists).
18. `printf "... (0=true):" $?`: Prints a success message and also prints the exit status itself, which will be `0`.
19. `else`: This block runs if the `test -f` command failed (the file does not exist or is not a regular file).
20. `printf "... (1=false):" $?`: Prints a failure message and prints the exit status, which will be non-zero (e.g., `1`).
21. `fi`: Closes the second `if` statement.
22. `chmod u+x testing_file.sh`: Makes the script executable.

#### Script Execution and Scenarios

By running this script, you will be prompted to provide the filename. Let’s do some tests. The output shows three different scenarios:

1.  When we do not provide a filename.
2.  When we enter a correct filename that exists.
3.  When we enter a wrong filename that does not exist.

<!-- end list -->

```bash
hashim@Hashim:~$ ./testing_file.sh 
Enter a file name using absolute path:

no filename entered...
hashim@Hashim:~$ ./testing_file.sh 
Enter a file name using absolute path:
/etc/passwd
The filename you entered is: /etc/passwd
Testing if "/etc/passwd" is a file...
The "/etc/passwd" represents a file. (0=true):
0
hashim@Hashim:~$ ./testing_file.sh 
Enter a file name using absolute path:
/etc/hello
The filename you entered is: /etc/hello
Testing if "/etc/hello" is a file...
The "/etc/hello" is not a file. (1=false):
1
```

#### ✏️ Detailed Explanation

  * **Test 1 (Empty Input):**

      * `./testing_file.sh` is run.
      * At the prompt, the user presses `Enter` without typing anything.
      * The `if [ -z "$filename" ]` test is **true**.
      * The script prints "no filename entered..." and immediately stops because of the `exit 1` command.

  * **Test 2 (Valid File):**

      * `./testing_file.sh` is run.
      * The user types `/etc/passwd` (a file that exists on all Linux systems).
      * The `if [ -z ... ]` test is **false**, so the `else` block runs, printing "The filename you entered is: /etc/passwd".
      * The script continues. The `test -f "/etc/passwd"` command runs and **succeeds**, setting `$?` to `0`.
      * The `if [ $? -eq 0 ]` test is **true**.
      * The `then` block runs, printing "The "/etc/passwd" represents a file. (0=true): 0".

  * **Test 3 (Invalid File):**

      * `./testing_file.sh` is run.
      * The user types `/etc/hello` (a file that does not exist).
      * The `if [ -z ... ]` test is **false**, so the `else` block runs, printing "The filename you entered is: /etc/hello".
      * The script continues. The `test -f "/etc/hello"` command runs and **fails**, setting `$?` to `1`.
      * The `if [ $? -eq 0 ]` test is **false**.
      * The `else` block runs, printing "The "/etc/hello" is not a file. (1=false): 1".

---

# 🔄 **Using Looping Statements**

In Bash, looping statements use the `for`, `while`, and `until` commands. We will show you how to use them in this section. Looping statements are used when repeating processes are needed, such as looping through several commands until a condition is met.

-----

## ➡️ Using the `for` statement

As in any other programming language, the need for iterating appears when repetitive tasks have to be done. This means that some commands need to be repeated until one or more conditions are met. This is similar in Bash as in any other programming language, and one of the commands to use is the `for` command.

It has the following structure:

```bash
for var in list
do
    commands
done
```

If you have not had any interaction with such a statement before, we will help you understand what it means.

  * The variables provided through the `var` parameter (e.g., `i`) are assigned a series of values from the `list` parameter, one at a time, in a series of iterations.
  * At the start of the iteration, the `var` is set with the current value in the `list`.
  * Each iteration will use the next value from the `list`, until the last item in the list is reached.
  * The number of items in the `list` will set the total number of iterations.
  * For each iteration, the `commands` inside the `do...done` block will be executed.

This is a basic loop.

### 🔢 Example 1: Looping Through an Array

Let’s see some basic uses of the `for` command. We will loop through a statically declared array. This means that we will not use the input from our user; instead, we will specify the array directly inside the script. We will use a temporary variable (or counter) called `i` to iterate through the entire length of the array. We use `${array[@]}` to specify all elements in the array. The loop will stop when the counter (`i`) reaches the end of the list.

The following figure shows the script’s code, the commands used to run it, and the output. Keep in mind that we provided an array that had values already ordered. This is not a sorting algorithm.

#### 💻 Code Snippet and Execution

```bash
hashim@Hashim:~$ nano array_loop.sh
hashim@Hashim:~$ cat array_loop.sh 
#!/bin/bash
array=(0 1 2 3 4 5 6)
# looping through the entire array
for i in "${array[@]}"
do
    echo $i
done
hashim@Hashim:~$ chmod u+x array_loop.sh 
hashim@Hashim:~$ ./array_loop.sh 
0
1
2
3
4
5
6
```

#### ✏️ Detailed Explanation

1.  `nano array_loop.sh`: The user opens the `nano` text editor to create a new file named `array_loop.sh`.
2.  `cat array_loop.sh`: After saving the file, the `cat` command is used to display the contents of the script.
3.  `#!/bin/bash`: This is the **shebang**, which tells the operating system to use the Bash interpreter to run the script.
4.  `array=(0 1 2 3 4 5 6)`: An indexed array named `array` is declared and initialized with seven integer values.
5.  `# looping through the entire array`: This is a comment explaining the purpose of the code that follows.
6.  `for i in "${array[@]}"`: This is the `for` loop.
      * `i` is the variable that will hold the value for each iteration.
      * `"${array[@]}"` expands to a list of all elements in the array (`"0" "1" "2" "3" "4" "5" "6"`). The loop will run once for each element (7 times in total).
7.  `do`: Marks the beginning of the command block that will be executed in each loop.
8.  `echo $i`: This command prints the current value stored in the `i` variable.
9.  `done`: Marks the end of the `for` loop.
10. `chmod u+x array_loop.sh`: This command modifies the file's permissions. It **a**dds (`+`) e**x**ecutable (`x`) permission for the **u**ser (`u`), which is required to run the script directly.
11. `./array_loop.sh`: This command executes the script.
12. `0`...`6`: This is the output from the script. The `echo $i` command was executed 7 times, printing the value of `i` on a new line for each iteration of the loop.

-----

### 🧠 Advanced Example: Sorting an Array with Bubble Sort

In the following example, we will bring back the discussion on arrays and use some of them inside a `for` statement. This time, we will show you how to sort an array. We will use most of the structures we’ve already learned about, such as input reading, output formatting, arrays, and `for` statements.

> #### 💡 Important Note
>
> Sorting algorithms are outside the scope of this text. We will only use one type of sort (bubble) to show you how to use arrays and `for` statements and how powerful Bash can be. However, if you plan on doing any serious programming while using the shell, we advise you to use another programming language that is more suited for this type of action, such as **Python**. Python is incredibly versatile and can successfully be used for many administrative tasks.

Let’s get back to our sorting issue. Let’s say we have a random array that we would like to sort. We will use an array that has integer elements only. To make things more interesting, we will prompt the user to introduce the array elements from the standard input.

#### 📜 Script Code

The following figure shows the code of the script:

```bash
hashim@Hashim:~$ nano array_bubble.sh
hashim@Hashim:~$ cat array_bubble.sh 
#!/bin/bash
#asking for array length
echo "total array elements:"
read n
echo "enter numbers"
#asking user to provide the numbers
for (( i = 0; i < $n; i++ ))
do
    read num[$i]
done
#start the sorting
for (( i = 0; i < $n; i++ ))
do
    for (( j = $i; j < $n; j++ ))
    do
        if [ ${num[$i]} -gt ${num[$j]} ]
        then
            k=${num[$i]}
            num[$i]=${num[$j]}
            num[$j]=$k
        fi
    done
done
#sorted array
printf "%s\n" "Sorted array is:"
for (( i = 0; i < n; i++ ))
do
    echo ${num[$i]}
done
```

#### ✏️ Detailed Code Explanation

Let’s explain the code.

  * `echo "total array elements:"`: This prints a prompt asking the user for the length of the array.
  * `read n`: This command waits for user input and stores the typed value (the length) in the variable `n`.
  * `echo "enter numbers"`: This prints a second prompt, asking the user to provide the numbers for the array.
  * `for (( i = 0; i < $n; i++ ))`: This is the first `for` loop, which uses a C-style syntax. It initializes a counter `i` at 0 and loops as long as `i` is less than the value of `$n`, incrementing `i` by one at each step (`i++`).
  * `read num[$i]`: Inside the loop, this command reads the user's input and stores it in an array called `num` at the index `i`. This loop is used to fill the array.
  * `#start the sorting`: A comment indicating the sorting logic is about to begin.
  * `for (( i = 0; i < $n; i++ ))`: This is the **outer** `for` loop of the sorting algorithm. It iterates through each element of the array.
  * `for (( j = $i; j < $n; j++ ))`: This is a **nested** (inner) `for` loop. A new counter `j` is used. It starts at the current value of `i` and loops to the end of the array. This is used to compare the element at `i` with every subsequent element at `j`.
  * `if [ ${num[$i]} -gt ${num[$j]} ]`: This is the comparison. It checks if the number at index `i` (`${num[$i]}`) is **g**reater **t**han (`-gt`) the number at index `j` (`${num[$j]}`).
  * `then`: This block is executed only if the condition is true (the element at `i` is larger than the element at `j`).
  * `k=${num[$i]}`: A temporary variable `k` is used to store the value of the greater number (`num[i]`).
  * `num[$i]=${num[$j]}`: The value of the smaller number (`num[j]`) is moved into the position of the larger number (`num[i]`).
  * `num[$j]=$k`: The original value of the larger number (which was saved in `k`) is placed into the position `num[j]`. This completes the **swap**.
  * `fi`, `done`, `done`: These keywords close the `if` statement, the inner `for` loop, and the outer `for` loop. The loop is finished when all numbers have been cycled through.
  * `#sorted array`: A comment indicating the sorted array will now be printed.
  * `printf "%s\n" "Sorted array is:"`: This prints a clean header message before showing the result.
  * `for (( i = 0; i < n; i++ ))`: This is the final `for` loop, used to iterate through the array (which is now sorted).
  * `echo ${num[$i]}`: This prints the element at index `i` of the `num` array.

#### 🚀 Script Execution and Output

The user input and the output of the command are shown here:

```bash
hashim@Hashim:~$ chmod u+x array_bubble.sh 
hashim@Hashim:~$ ./array_bubble.sh 
total array elements:
3
enter numbers
45
24
56
Sorted array is:
24
45
56
```

#### ⚙️ Execution Analysis

1.  `chmod u+x array_bubble.sh`: First, the script is made executable.
2.  `./array_bubble.sh`: The script is run.
3.  `total array elements:`: The script asks for the array size. The user enters `3`.
4.  `enter numbers`: The script asks for the numbers.
5.  `45`: The user enters `45` (stored in `num[0]`).
6.  `24`: The user enters `24` (stored in `num[1]`).
7.  `56`: The user enters `56` (stored in `num[2]`).
8.  The script then executes the sorting logic silently.
9.  `Sorted array is:`: The script prints the header.
10. `24`, `45`, `56`: The script runs its final `for` loop, printing each element of the now-sorted array.

In the preceding example, the bubble sort works as follows:

  * **Step 1:** The first step is to compare the first two elements in the array, which are `45` and `24`. It sees that `45` is greater than `24`, so the algorithm **swaps** them. The array becomes: `24 45 56`.
  * **Step 2:** The second step is to compare the next two elements, which are now `45` and `56` (because `45` was greater than `24` and is now in the second position). As `45` is not greater than `56`, their position will remain unchanged.
  * **Step 3:** The third step is to do one more pass through all the elements and still do the comparison to ensure the array is fully sorted.

> #### 💡 Important Note
>
> Bubble sort is not an efficient sorting algorithm, but it was a good example of how to use arrays, `for` and `if` statements, input from the user, and output formatting. For more information on the bubble sort algorithm or any other type of sorting algorithm, we would advise a thorough online search or that you read other titles on the subject.


# 🔄 Using the `while` Statement

The `while` loop is similar to the `for` loop, but it also acts as a combination of an `if` statement. The loop will continue executing its commands **as long as a specific condition is true**.

The syntax is as follows:

```bash
while condition
do
    commands
done
```

The `condition` is tested every time an iteration is started. If the `condition` remains true (returns an exit status of zero), the `commands` are executed. This continues until the `condition` changes its status to false.

Let’s see an example.

### Code Snippet and Execution

```bash
hashim@Hashim:~$ nano while_loop_1.sh
hashim@Hashim:~$ cat while_loop_1.sh 
#!/bin/bash
#asking the user for the maximum value
printf "Provide the maximum value: "
read max
#start the loop
printf "Listing numbers in descending order... \n"
while [ $max -gt 0 ]
do
    printf "%s" $max " "
    max=$[$max - 1]
done
printf "\n"

hashim@Hashim:~$ chmod u+x while_loop_1.sh 
hashim@Hashim:~$ ./while_loop_1.sh 
Provide the maximum value: 10
Listing numbers in descending order... 
10 9 8 7 6 5 4 3 2 1 
hashim@Hashim:~$ ./while_loop_1.sh 
Provide the maximum value: 30
Listing numbers in descending order... 
30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
```

### ✏️ Detailed Explanation

1.  `nano while_loop_1.sh`: The user opens the `nano` text editor to create the script file.
2.  `cat while_loop_1.sh`: This command displays the contents of the file after it's saved.
3.  `#!/bin/bash`: The **shebang**, which specifies that this script should be run by the Bash interpreter.
4.  `#asking...`: A comment explaining the code's purpose.
5.  `printf "Provide the maximum value: "`: This prints a prompt for the user. It does not add a new line, so the user types on the same line.
6.  `read max`: This command waits for the user's input and stores it in a variable named `max`.
7.  `#start the loop`: Another explanatory comment.
8.  `printf "Listing numbers... \n"`: Prints a status message, followed by a new line.
9.  `while [ $max -gt 0 ]`: This is the `while` loop's condition. It checks if the value in the `$max` variable is **g**reater **t**han (`-gt`) 0. The loop will execute as long as this is true.
10. `do`: This keyword marks the beginning of the command block for the loop.
11. `printf "%s" $max " "`: This command prints the current value of `$max` (formatted as a string `%s`), followed by a space.
12. `max=$[$max - 1]`: This is the most critical line for the loop. It performs an arithmetic operation (`$[$... - 1]`) to decrease the value of `max` by 1 and re-assigns the new, lower value to the `max` variable. This is what prevents an **infinite loop**.
13. `done`: This keyword marks the end of the loop block.
14. `printf "\n"`: After the loop finishes, this command prints a single new line to clean up the prompt.
15. `chmod u+x while_loop_1.sh`: This makes the script executable for the user.
16. `./while_loop_1.sh`: The script is run. The user provides `10`. The loop runs 10 times, printing the number and a space, then decrementing it. When `max` becomes `0`, the condition `[ 0 -gt 0 ]` becomes false, and the loop terminates.
17. `./while_loop_1.sh`: The script is run a second time with the value `30`, demonstrating the same logic with a different input.

The `while` statement is very useful and straightforward, being a great addition to the `for` statement. Now, let’s see the `until` statement.

-----

## 🔁 Using the `until` Statement

This looping structure is the **opposite** of the `while` loop. It uses a condition that is false from the start. The commands inside the structure will be executed **as long as the condition remains false**.

The syntax is as follows:

```bash
until condition
do
    commands
done
```

For a very quick example, let’s redo the `while` loop from the previous example by using the `until` statement this time.

### Code Snippet and Execution

Here’s the code:

```bash
hashim@Hashim:~$ nano until_loop_1.sh
hashim@Hashim:~$ cat until_loop_1.sh 
#!/bin/bash
#asking the user for the maximum value
printf "Provide the maximum value: "
read max
#starting the loop
printf "Listing numbers in descending order... \n"
until [ $max -eq 0 ]
do
    printf "%s" $max
    max=$[$max - 1]
done
printf "\n"
hashim@Hashim:~$ chmod u+x until_loop_1.sh 
hashim@Hashim:~$ ./until_loop_1.sh 
Provide the maximum value: 10
Listing numbers in descending order... 
10987654321
```

### ✏️ Detailed Explanation

1.  `nano until_loop_1.sh`: The user opens the `nano` editor to create this new script.
2.  `cat until_loop_1.sh`: This displays the script's content.
3.  The setup (`#!/bin/bash`, `printf`, `read max`, `printf`) is identical to the `while` loop example.
4.  `until [ $max -eq 0 ]`: This is the key difference. The `until` loop's condition checks if the value of `$max` is **eq**ual (`-eq`) to 0. The loop will continue to run *as long as this condition is false*. When `max` is `10`, `[ 10 -eq 0 ]` is false, so the loop runs. This continues until `max` becomes `0`, at which point `[ 0 -eq 0 ]` is **true**, and the loop stops.
5.  `do`: Marks the start of the command block.
6.  `printf "%s" $max`: This command prints the current value of `max`. **Note:** Unlike the `while` script, a space (`" "`) was not included here. This is why the final output (`10987654321`) is all run together without spaces.
7.  `max=$[$max - 1]`: Just like in the `while` loop, the value of `max` is decreased by 1 in each iteration.
8.  `done`: Marks the end of the loop.
9.  `printf "\n"`: Prints a final new line.
10. `chmod u+x until_loop_1.sh`: Makes the script executable.
11. `./until_loop_1.sh`: The script is run, the user enters `10`, and the loop executes 10 times, printing the numbers without spaces.

Can you spot the differences between the `until` and `while` loops?

  * **`while` loop:** `while [ $max -gt 0 ]` (Runs while the condition is **true**).
  * **`until` loop:** `until [ $max -eq 0 ]` (Runs while the condition is **false**).

The commands inside the `until` loop are the same as the ones used inside the `while` loop. The **condition** is different. The output, as you might expect, is the same (in terms of the numbers printed) as when using a `while` loop.

---

# 🛑 Exiting Loop Statements

The two commands that are used in Bash to exit loops are **`break`** and **`continue`**. They are relatively straightforward. Whenever you want to exit a loop, you can use one of them.

Let’s use a simple script that iterates through a series of integer numbers until a specified value is reached, at which point it exits the iteration.

-----

## 1\. Using the `break` Command (Version 1)

Here is the code:

#### 💻 Code Snippet

```bash
hashim@Hashim:~$ nano break_loop.sh
hashim@Hashim:~$ cat break_loop.sh 
#!/bin/bash
echo "Enter maximum sequence: "
read max
echo "Enter break value: "
read brk
echo "Starting the iteration..."
for (( i = 0; i < $max; i++ ))
do
    echo $i
    if [ $i -eq $brk ]
    then
        break
    fi
done
hashim@Hashim:~$ chmod u+x break_loop.sh
```

#### ✏️ Detailed Explanation

  * `nano break_loop.sh`: This command opens the `nano` text editor to create or edit the script file.
  * `cat break_loop.sh`: This command displays the contents of the `break_loop.sh` file.
  * `#!/bin/bash`: This is the **shebang**, which tells the operating system to use the Bash interpreter to run this script.
  * `echo "Enter maximum sequence: "`: This prints a prompt for the user, asking for the maximum value of the sequence.
  * `read max`: This command waits for the user's input and stores it in the variable named `max`.
  * `echo "Enter break value: "`: This prints a prompt asking for the value at which the loop should stop.
  * `read brk`: This command reads the user's input and stores it in the `brk` variable.
  * `echo "Starting the iteration..."`: This prints a status message to the user.
  * `for (( i = 0; i < $max; i++ ))`: This is a C-style `for` loop. It initializes a variable `i` at 0, and will loop as long as `i` is less than the value of `$max`. It increments `i` by 1 after each iteration (`i++`).
  * `do`: This keyword marks the beginning of the block of commands to be executed in each loop.
  * `echo $i`: This command **prints the current value of `i`** to the screen.
  * `if [ $i -eq $brk ]`: This is a conditional test. It checks if the current value of `i` is **eq**ual (`-eq`) to the value stored in `$brk`.
  * `then`: This keyword starts the block of commands to run if the condition is true.
  * `break`: This is the **`break` command**. It immediately and completely **terminates the `for` loop**. Execution will continue after the `done` keyword.
  * `fi`: This keyword closes the `if` statement.
  * `done`: This keyword closes the `for` loop block.
  * `chmod u+x break_loop.sh`: This command modifies the file's permissions. It **a**dds (`+`) e**x**ecutable (`x`) permission for the **u**ser (`u`), which allows the script to be run.

#### 🚀 Execution and Output

When executing, the user is asked to introduce the maximum value for the sequence and the value of `break`. The `for` loop goes through the sequence until the value of `break` is reached, and it exits the loop. We used the `break` command to exit the loop and started the iteration from zero. You can test the outcome with different values.

The following is the output when using a value of `10` for the maximum sequence and a value of `5` for `break`:

```bash
hashim@Hashim:~$ ./break_loop.sh 
Enter maximum sequence: 
10
Enter break value: 
5
Starting the iteration...
0
1
2
3
4
5
```

As you can see, the loop exits after 5 is reached. **However, it shows the value of 5; it does not skip it.** This happens because the `echo $i` command is placed *before* the `if` statement that checks for the break condition.

This can be fixed (it is not necessarily an issue, but more of an algorithm design decision). Let’s look at the following code.

-----

## 2\. Using the `break` Command (Version 2)

For the breaking value to *not* be shown, we moved the `echo $i` command *after* the conditional `if-then` statement. This will prevent the script from showing the breaking value provided by the user.

#### 💻 Code Snippet

```bash
hashim@Hashim:~$ nano break_loop_2.sh
hashim@Hashim:~$ cat break_loop_2.sh 
#!/bin/bash
echo "Enter maximum sequence: "
read max
echo "Enter break value: "
read brk
echo "Starting the iteration..."
for (( i = 0; i < $max; i++ ))
do
    if [ $i -eq $brk ]
    then
        break
    fi
    echo $i
done
hashim@Hashim:~$ chmod u+x break_loop_2.sh
```

#### ✏️ Detailed Explanation

The script is almost identical, but the logic inside the loop is reversed:

  * `for (( i = 0; i < $max; i++ ))`: The loop starts.
  * `if [ $i -eq $brk ]`: The script **checks the condition first**. If the current value of `i` (e.g., 5) equals the break value `$brk` (e.g., 5)...
  * `then`: ...the `then` block is executed.
  * `break`: The loop is immediately terminated.
  * `fi`: The `if` statement ends.
  * `echo $i`: This command is now *after* the `if` block. It will only be executed if the `break` command was *not* run. When `i` is 5, the loop breaks before this line is ever reached.

#### 🚀 Execution and Output

```bash
hashim@Hashim:~$ ./break_loop_2.sh 
Enter maximum sequence: 
10
Enter break value: 
5
Starting the iteration...
0
1
2
3
4
```

Both use cases are valid, and the position of the output printing command is only relevant to your needs.

-----

## 3\. Using the `continue` Command

In the following example, we will show you how to use the `continue` command. The algorithm is similar to the one used in the `break` example.

### 🛑 `break` vs. `continue` ➡️

This difference is sufficient for you to understand the differences between `break` and `continue`:

  * **`break`**: Exits the loop *completely* when a condition is met.
  * **`continue`**: **Skips the rest of the commands** in the current iteration when a certain condition is met and **continues to the next iteration** of the loop sequence.

Let’s see the code for the same script, using the `continue` command. This time, the value provided by the user will not be shown in the output, but the loop will continue.

#### 💻 Code Snippet

```bash
hashim@Hashim:~$ nano continue_loop.sh
hashim@Hashim:~$ cat continue_loop.sh 
#!/bin/bash
echo "Enter maximum sequence: "
read max
echo "Enter the value to skip: "
read skp
echo "Starting the iteration..."
for (( i = 0; i < $max; i++ ))
do
    if [ $i -eq $skp ]
    then
        continue
    fi
    echo $i
done
hashim@Hashim:~$ chmod u+x continue_loop.sh
```

#### ✏️ Detailed Explanation

As you can see, the code is similar to what was used in the previous example. The only difference is the use of the `continue` command instead of `break`, and we've renamed the `brk` variable to `skp` (for "skip") for clarity.

  * `nano continue_loop.sh`: Opens the `nano` editor.
  * `cat continue_loop.sh`: Displays the script's content.
  * `#!/bin/bash`: The shebang.
  * `echo "Enter maximum sequence: "`: Prompts for the max value.
  * `read max`: Stores the value in `max`.
  * `echo "Enter the value to skip: "`: Prompts for the value to skip.
  * `read skp`: Stores the value in `skp`.
  * `echo "Starting the iteration..."`: Prints a status message.
  * `for (( i = 0; i < $max; i++ ))`: The loop starts, iterating from 0 up to `$max`.
  * `if [ $i -eq $skp ]`: This checks if the current value of `i` is **eq**ual to the value to skip (`$skp`).
  * `then`: If the condition is true (e.g., `i` is 5 and `$skp` is 5)...
  * `continue`: The **`continue` command** is executed. This immediately **stops the current iteration** and tells the loop to jump back to the top and start the **next iteration** (with `i = 6`).
  * `fi`: Closes the `if` statement.
  * `echo $i`: This line is only reached if the `if` condition was false. If the `continue` command was run, this `echo` command is skipped for that iteration.
  * `done`: Closes the `for` loop.
  * `chmod u+x continue_loop.sh`: Makes the script executable.

#### 🚀 Execution and Output

Now, let’s see the output that’s obtained:

```bash
hashim@Hashim:~$ ./continue_loop.sh 
Enter maximum sequence: 
10
Enter the value to skip: 
5
Starting the iteration...
0
1
2
3
4
6
7
8
9
```

As shown in the preceding figure, the number **5** is not shown. This means that the loop skipped that iteration when the `continue` command was used.


---