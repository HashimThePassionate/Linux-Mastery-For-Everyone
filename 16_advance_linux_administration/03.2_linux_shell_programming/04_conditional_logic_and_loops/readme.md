# 🧠 **Conditional Logic & Loops**

This section introduces you to **operators**, **conditional logic**, and **loops** in **Bash scripting**.
You’ll learn how to handle **numeric data**, **string variables**, and **control flow** to make your scripts smarter and more efficient.

## ⚙️ **1. Arithmetic Operations & Variables**

In this section, you’ll learn:

* ➕ How to perform **arithmetic operations**
* 🧮 The **operators** available for numeric calculations
* 🪣 How to **assign values** to variables
* ⌨️ How to **read user input** in a shell script

> 💡 The `expr` command is commonly used to perform arithmetic tasks such as **addition**, **subtraction**, **multiplication**, and **division** on numeric values.

## 🧩 **2. Testing Variables, Files & Directories**

You’ll explore how to use the **`test` command** to evaluate:

* 🔢 Variable comparisons
* 📁 File and directory properties

### 🔍 **Common Test Operators**

| 🧠 **Type**            | 📝 **Description**      | 💡 **Examples**                              |
|-- |--- |---- |
| **Numeric Comparison** | Compare numeric values  | `-eq`, `-lt`, `-gt`                          |
| **String Comparison**  | Compare strings         | `==`, `<`, `>`                               |
| **File Tests**         | Check file properties   | `-f` (file), `-d` (directory), `-e` (exists) |
| **Boolean (Logical)**  | Combine or negate tests | `-a` (AND), `-o` (OR), `!` (NOT)             |

## 🧠 **3. Conditional Logic & Loops**

This part covers **decision-making** and **repetition** in Bash scripts.

### 🧩 **Conditional Statements**

* `if / else / fi` – Perform actions based on conditions
* `case / esac` – Multi-branch decision structure (like a switch statement)

### 🔁 **Loop Structures**

* `for` loops – Iterate through a list of values
* **Nested for** loops – Loop inside another loop
* `while` loops – Repeat while a condition is true
* `until` loops – Repeat until a condition becomes true

You’ll also learn to **define and call custom functions** to make your code reusable and organized.

## 🧮 **4. Working with Arrays**

Bash supports **arrays**, allowing you to store multiple values in a single variable.

You’ll learn how to:

* 🧱 **Create arrays**
* 🔄 **Iterate through elements**
* ✏️ **Update array contents**

---

# 🧮 The `expr` Command

The `expr` command is the "old-school calculator" of the shell. It's an external program used to evaluate expressions, including both arithmetic and simple string matching.

While Bash now has more modern, built-in ways to do math (like `((...))`), `expr` is still important to recognize, especially in older scripts.

### 🚀 Practical Example: Basic Arithmetic

The `expr` command supports basic operations. The key rule is that **spaces are required** between all operators and expressions.

```bash
# 1. This is correct
hashim@Hashim:~$ expr 2 + 2
4

# 2. This is INCORRECT (missing spaces)
hashim@Hashim:~$ expr 2+2
2+2
```

To store the result in a variable, you must use backticks (`` `...` ``) or the modern `$(...)` for command substitution.

```bash
hashim@Hashim:~$ sum=`expr 2 + 2`
hashim@Hashim:~$ echo "The sum: $sum"
The sum: 4
```

### 🚀 Practical Example: Finding String Length

An interesting (and somewhat non-obvious) use of `expr` is to find the length of a string. It does this by using a regular expression.

```bash
hashim@Hashim:~$ x="abc"
hashim@Hashim:~$ expr "$x" : '.*'
3
hashim@Hashim:~$ x="abc"
hashim@Hashim:~$ len=$(expr "$x" : '.*')
hashim@Hashim:~$ echo "$len"
3
```

#### 📜 Code Explanation

  * `expr "$x" : '.*'`: This command compares the string in `$x` (`"abc"`) to the regular expression `'.*'` (which means "any character, zero or more times").
  * The colon (`:`) is a matching operator.
  * `expr` returns the **length** of the matching pattern, which in this case is the entire string (3 characters).

#### 💡 Modern (and Better) Way

This is a clunky way to get string length. The modern, built-in Bash way is much cleaner:

```bash
hashim@Hashim:~$ x="abc"
hashim@Hashim:~$ echo ${#x}
3
```


## ➕ Arithmetic Operators (with `expr`)

The Bash shell supports all standard arithmetic operations. When using `expr`, you must pay special attention to the multiplication operator.

| Operator | Purpose | `expr` Example |
| :--- | :--- | :--- |
| `+` | Addition | `expr 10 + 5` |
| `-` | Subtraction | `expr 10 - 5` |
| `\*` | Multiplication | `expr 10 \* 5` |
| `/` | Division (Integer) | `expr 10 / 5` |
| `%` | Modulus (Remainder) | `expr 10 % 3` |

**Why `\*`?** The asterisk (`*`) is a special metacharacter (a wildcard) used for filename globbing. To tell the shell "no, I mean literal multiplication," you must **escape it** with a backslash (`\`).

### 🚀 Practical Example: Arithmetic Script

Let's create a script from scratch to see all the operators in action.

**1. Create the script file:**

```bash
hashim@Hashim:~$ nano simple_math.sh
```

**2. Add the following content:**

```bash
#!/bin/bash
# A simple script to demonstrate expr arithmetic

x=15
y=4

echo "x = $x, y = $y"
echo "---"

sum=`expr $x + $y`
diff=`expr $x - $y`
prod=`expr $x \* $y`
div=`expr $x / $y`
mod=`expr $x % $y`

echo "sum = $sum"
echo "difference = $diff"
echo "product = $prod"
echo "quotient = $div"
echo "modulus = $mod"
```

**3. Make the script executable and run it:**

```bash
hashim@Hashim:~$ chmod u+x simple_math.sh
hashim@Hashim:~$ ./simple_math.sh
```

**4. Output:**

```
x = 15, y = 4
---
sum = 19
difference = 11
product = 60
quotient = 3
modulus = 3
```

#### 📜 Code Explanation

  * div=`expr $x / $y` : This performs **integer division**. 15 divided by 4 is 3.75, but Bash (with  `expr` ) drops the decimal, resulting in  3.
  * mod=`expr $x % $y` : This calculates the remainder of 15 divided by 4. (4 goes into 15 three times (12), with  `3` left over).


## ⚖️ Boolean and Numeric Operators

This is a critical concept in Bash. You **must** use different operators for comparing numbers versus comparing strings.

### Numeric Operators (`-eq`, `-ne`, `-gt`, `-lt`, `-ge`, `-le`)

These operators are for **numeric values only**. They will not work correctly for "abc" or "hello".

| Operator | Meaning |
| :--- | :--- |
| `-eq` | **Eq**ual to |
| `-ne` | **N**ot **e**qual to |
| `-gt` | **G**reater **t**han |
| `-lt` | **L**ess **t**han |
| `-ge` | **G**reater than or **e**qual to |
| `-le` | **L**ess than or **e**qual to |

### String Operators (`==`, `!=`)

These operators are for **string values**.

| Operator | Meaning |
| :--- | :--- |
| `==` | Equal to |
| `!=` | Not equal to |

### 🚀 Practical Example: `if` Statements

These operators are almost always used inside `if` statements, which require `[` square brackets `]`.

**CRITICAL:** There **must be spaces** inside the brackets: `[ $x -eq $y ]` is correct. `[$x -eq $y]` is an error.

**1. Create the script file:**

```bash
hashim@Hashim:~$ nano comparison_test.sh
```

**2. Add the following content:**

```bash
#!/bin/bash
# A script to demonstrate numeric and string comparisons

a=10
b=5
str1="hello"
str2="goodbye"

# --- Numeric Comparison ---
echo "--- Numeric Tests (a=10, b=5) ---"
if [ $a -gt $b ]; then
  echo "$a is greater than $b"
fi

if [ $a -ne $b ]; then
  echo "$a is not equal to $b"
fi

# --- String Comparison ---
echo "--- String Tests (str1=hello, str2=goodbye) ---"
if [ $str1 == "hello" ]; then
  echo "str1 is 'hello'"
fi

if [ $str1 != $str2 ]; then
  echo "$str1 is not the same as $str2"
fi
```

**3. Make it executable and run it:**

```bash
hashim@Hashim:~$ chmod u+x comparison_test.sh
hashim@Hashim:~$ ./comparison_test.sh
```

**4. Output:**

```
--- Numeric Tests (a=10, b=5) ---
10 is greater than 5
10 is not equal to 5
--- String Tests (str1=hello, str2=goodbye) ---
str1 is 'hello'
hello is not the same as goodbye
```


## 🧩 Compound Logical Operators (`-a`, `-o`, `!`)

You can combine tests using logical AND, OR, and NOT.

| Operator | Meaning |
| :--- | :--- |
| `!` | **NOT** (reverses the value) |
| `-a` | **AND** (both operands must be true) |
| `-o` | **OR** (at least one operand must be true) |

### 🚀 Practical Example: Compound `if`

Let's edit our script to use these.

```bash
hashim@Hashim:~$ nano comparison_test.sh
```

**5. Add this new block to the end of the script:**

```bash
# --- Compound Tests ---
echo "--- Compound Tests (a=10, b=5) ---"

# Example 1: AND (-a)
# Is 'a' less than 20 AND 'b' greater than 100?
if [ $a -lt 20 -a $b -gt 100 ]; then
  echo "AND test: This will NOT print (because b is not > 100)"
else
  echo "AND test: False (This is correct)"
fi

# Example 2: OR (-o)
# Is 'a' less than 20 OR 'b' greater than 100?
if [ $a -lt 20 -o $b -gt 100 ]; then
  echo "OR test: True (because a is < 20)"
fi

# Example 3: NOT (!)
# Is it NOT true that 'a' equals 10?
if [ ! $a -eq 10 ]; then
  echo "NOT test: This will NOT print (because a does equal 10)"
else
  echo "NOT test: False (This is correct)"
fi

# Example 4: Grouping \( ... \)
# Is (a=10 AND b=5) OR is (a=100)?
if [ \( $a -eq 10 -a $b -eq 5 \) -o \( $a -eq 100 \) ]; then
    echo "Grouping test: The first group (a=10 AND b=5) was true."
fi
```

**6. Run the updated script:**

```bash
hashim@Hashim:~$ ./comparison_test.sh
```

**7. New Output:**

```
--- Compound Tests (a=10, b=5) ---
AND test: False (This is correct)
OR test: True (because a is < 20)
NOT test: False (This is correct)
Grouping test: The first group (a=10 AND b=5) was true.
```

  * **📜 Code Explanation for Grouping:**
      * `\( ... \)`: You must **escape the parentheses** with backslashes. This is because parentheses have a special meaning (creating a subshell).
      * The `if` statement checks `(True AND True) OR (False)`, which evaluates to `True`.


## 💡 A Note on `[ ... ]` and the `test` Command

Logical conditions and other tests are usually enclosed in square brackets `[]`.

It's helpful to know that **`[` is actually a command**, not just syntax. It is a built-in command that is an alias for the `test` command.

This means that the following two `if` statements are **exactly identical**:

```bash
if [ $var -eq 0 ]; then echo "True"; fi
```

```bash
if test $var -eq 0 ; then echo "True"; fi
```

Because `[` is a command, it requires a space after it, just like any other command (e.g., `echo "hi"`, not `echo"hi"`). This is why `[ $var -eq 0 ]` is valid but `[$var -eq 0 ]` is an error.

### 💡 Pro-Tip: The Modern `[[ ... ]]`

For modern Bash scripting, it is highly recommended to use **double square brackets `[[ ... ]]`**.

  * `if [[ $var -eq 0 && $var2 -gt 2 ]]`
  * `if [[ $str1 == "hello" ]]`

This is a Bash "keyword," not a command. It is safer, more powerful, and has a more natural syntax (e.g., it uses `&&` for AND instead of `-a`).


---

# 💻 Working with Variables

You have already seen some examples of variables in Bash. This section provides more information about how to assign values to variables. You will also see how to use conditional logic to test the values of variables.

Always remember that **Bash variables are fundamentally typeless**. This means the shell makes no distinction between a number (like `123`) and a string (like `"abc"`).

However, you will get an error message if you attempt to perform arithmetic operations on non-numeric values in Bash, which is not always the case in languages like JavaScript.



## ✍️ Assigning Values to Variables

This section contains simple examples of assigning values to variables.

### 🚫 The "No Spaces" Rule

Make sure that you do not insert any whitespace around the equals sign (`=`) during variable assignment.

#### 🚀 Practical Example (Correct vs. Incorrect)

  * **Correct (No spaces):**

    ```bash
    hashim@Hashim:~$ z="abc"
    hashim@Hashim:~$ echo $z
    abc
    ```

  * **Incorrect (With spaces):**

    ```bash
    hashim@Hashim:~$ z = "abc"
    -bash: z: command not found
    ```

      * **📜 Code Explanation:** When you add spaces, the Bash shell interprets `z` as a *command* (which it tries to run) and `="abc"` as the arguments for that command. This is why you get `command not found`.

### 🚫 No `$` on Assignment

One more thing to keep in mind: the `$` symbol is only used when **reading** (accessing) a variable, not when **assigning** (writing) it.

  * **Incorrect (Using `$`):**
    ```bash
    hashim@Hashim:~$ $y=3
    -bash: =3: command not found
    ```
      * **📜 Code Explanation:** The shell first tries to expand `$y`. If `y` is empty, the command becomes `=3`, which is not a valid command.

### 💬 Double (`"`) vs. Single (`'`) Quotes

  * **Double Quotes (`""`):** These allow **variable expansion**. The shell will replace variable names (like `$x`) with their values.
  * **Single Quotes (`''`):** These are **literal**. They prevent *all* expansion and treat every character (like `$`) as plain text.

#### 🚀 Practical Example

```bash
hashim@Hashim:~$ x="abc"
hashim@Hashim:~$ y="123"

# 1. Variables are expanded in double quotes
hashim@Hashim:~$ echo "x = $x and y = ${y}"
x = abc and y = 123

# 2. You can concatenate variables directly
hashim@Hashim:~$ echo "xy = $x$y"
xy = abc123

# 3. Double quotes expand $x, but single quotes treat $x literally
hashim@Hashim:~$ echo "double and single quotes: $x" '$x'
double and single quotes: abc $x
```

  * **📜 Code Explanation:**
      * In the first `echo`, `$x` and `${y}` were replaced by their values (`abc` and `123`). Using braces (`${y}`) is a more robust way to write a variable, especially when it is next to other text.
      * In the third `echo`, the first `$x` (in double quotes) was expanded, but the second `$x` (in single quotes) was printed literally.



## ⚙️ Listing 4.1: `variable-operations.sh`

This script illustrates how to assign variables with different values and update them using "Parameter Expansion."

### The Script

```bash
#!/bin/bash
# variable-operations.sh

#length of myvar:
myvar=123456789101112
echo ${#myvar}

#print last 5 characters of myvar:
echo ${myvar: -5}

#10 if myvar was not assigned
echo ${myvar:-10}

#last 10 symbols of myvar
echo ${myvar: -10}

#substitute part of string with echo:
echo ${myvar//123/999}

#add integers a to b and assign to c:
a=5
b=7
c=$((a+b))
echo "a: $a b: $b c: $c"

# other ways to calculate c:
c=`expr $a + $b`
echo "c: $c"
c=`echo "$a+$b"|bc`
echo "c: $c"
```

### Script Output

When you launch the code in Listing 4.1, you will see the following output:

```
15
01112
123456789101112
6789101112
999456789101112
a: 5 b: 7 c: 12
c: 12
c: 12
```

### 📜 Code Explanation

1.  **`myvar=123456789101112`**
    Assigns a long string of 15 digits to the variable `myvar`.

2.  **`echo ${#myvar}`**

      * **Output:** `15`
      * **Explanation:** The `${#variable}` syntax returns the **character length** of the variable.

3.  **`echo ${myvar: -5}`**

      * **Output:** `01112`
      * **Explanation:** This is **substring extraction**. The `${variable: -5}` syntax extracts the last 5 characters. (Note: The space after the colon is important to distinguish it from the syntax in the next step).

4.  **`echo ${myvar:-10}`**

      * **Output:** `123456789101112`
      * **Explanation:** This syntax provides a **default value**. It means: "If `myvar` is unset or empty, print `10`. Otherwise, print the value of `myvar`." Since `myvar` *is* set, it prints the full value.

5.  **`echo ${myvar: -10}`**

      * **Output:** `6789101112`
      * **Explanation:** This is another substring extraction, this time getting the last 10 characters.

6.  **`echo ${myvar//123/999}`**

      * **Output:** `999456789101112`
      * **Explanation:** This is **search and replace**. The `${variable//find/replace}` syntax replaces all occurrences of "find" with "replace". In this case, it finds "123" at the beginning and replaces it with "999".

7.  **`c=$((a+b))`**

      * **Output:** (prints `a: 5 b: 7 c: 12`)
      * **Explanation:** This is the **modern, built-in** method for performing arithmetic in Bash. The `$((...))` construct evaluates the expression inside.

8.  **`c=\`expr $a + $b\`\`**

      * **Output:** (prints `c: 12`)
      * **Explanation:** This is the **old, external command** method. `expr` evaluates the expression. Note that `expr` requires spaces around the operators (e.g., `5 + 7`, not `5+7`).

9.  **`c=\`echo "$a+$b" | bc\`\`**

      * **Output:** (prints `c: 12`)
      * **Explanation:** This is the most powerful method. It pipes the string "5+7" into the `bc` (Basic Calculator) command. `bc` is essential when you need to perform floating-point (decimal) math, as both `expr` and `$(())` only handle integers.



## ⌨️ The `read` Command for User Input

The `read` command pauses a script and waits for the user to type in data, which is then stored in a variable.

The syntax `read -n number_of_chars myvar` reads a specific number of characters from the input into `myvar`.

### 🚀 Practical Example 1: `-n` (Number of Characters)

The following code snippet reads **two** characters from the command line and then continues *without* waiting for the Enter key.

```bash
hashim@Hashim:~$ echo "Press any two keys to continue..."
Press any two keys to continue...
hashim@Hashim:~$ read -n 2 var
ab  <-- User types 'a' then 'b', and the script immediately continues
hashim@Hashim:~$ echo "You entered: $var"
You entered: ab
```

  * **📜 Code Explanation:** The `-n 2` flag was satisfied as soon as the user typed the second character (`b`), so the `read` command finished.

### 🚀 Practical Example 2: `-s` (Silent / Secret)

The following command reads a password in non-echoed mode. The user's typing is hidden.

```bash
hashim@Hashim:~$ echo "Please enter your password:"
Please enter your password:
hashim@Hashim:~$ read -s var
      <-- User types their password, but nothing appears on screen. They press Enter.
hashim@Hashim:~$ echo "Password stored!"
Password stored!
```

  * **📜 Code Explanation:** The `-s` (silent) flag is crucial for securely handling passwords so they are not displayed on the screen.

### 🚀 Practical Example 3: `-p` (Prompt)

This code snippet displays a prompt on the same line that the user types on.

```bash
hashim@Hashim:~$ read -p "Enter your name: " var
Enter your name: Hashim  <-- User types 'Hashim' and presses Enter
hashim@Hashim:~$ echo "Hello, $var"
Hello, Hashim
```

  * **📜 Code Explanation:** The `-p` (prompt) flag displays the string `"Enter your name: "` and then waits for the user's input on that very same line. This is much cleaner than using a separate `echo` command.

---

# 🤖 Boolean and String Operators in Bash

Bash provides various operators for testing string variables and for combining these tests using Boolean logic.

### 📋 Basic String Tests

Let's suppose we have two variables, `x` and `y`, with the following values:

```bash
hashim@Hashim:~$ x="abc"
hashim@Hashim:~$ y="efg"
```

Here are the results of basic test operators:

  * `[ $x = $y ]`: This expression is **false** because the values "abc" and "efg" are not equal.
  * `[ $x != $y ]`: This expression is **true** because the values "abc" and "efg" are not equal.
  * `[ -z $x ]`: This expression is **false** because the variable `$x` contains "abc", which has a non-zero length.
  * `[ -n $x ]`: This expression is **true** because the variable `$x` contains "abc", which has a non-zero length.

You can combine these operators to create compound statements, similar to numeric comparisons.


### ⚠️ Important Distinctions

Keep in mind the difference between string and numeric operators:

  * **`==`**: Use this operator for **string** comparisons.
  * **`-eq`**: Use this operator for **numeric** tests and comparisons.

### 📏 String Length Operators

You can explicitly determine if a string has a non-zero length:

  * **`-n s1`**: Returns **true** if the string `s1` has a **non-zero** length.
  * **`-z s1`**: Returns **true** if the string `s1` has a **zero** length.


### ✨ Best Practice: Double Brackets

When you perform string comparisons, it is highly recommended to use **double square brackets** (`[[ ... ]]`). Using single brackets (`[ ... ]`) can sometimes lead to unexpected errors.

### 🧬 String Comparison Reference

Here are the common string comparison operators used with `[[ ... ]]`:

  * **`[[ $str1 = $str2 ]]`**: Returns **true** when `str1` is equal to `str2`.
  * **`[[ $str1 == $str2 ]]`**: An alternative method to check for string equality.
  * **`[[ $str1 != $str2 ]]`**: Returns **true** when `str1` and `str2` do not match.
  * **`[[ $str1 > $str2 ]]`**: Returns **true** when `str1` is alphabetically **greater** than `str2`.
  * **`[[ $str1 < $str2 ]]`**: Returns **true** when `str1` is alphabetically **lesser** than `str2`.
  * **`[[ -z $str1 ]]`**: Returns **true** if `str1` holds an empty string.
  * **`[[ -n $str1 ]]`**: Returns **true** if `str1` holds a non-empty string.


### 🔗 Compound Operators and String Operators

You can combine multiple string-related conditions using logical operators like `&&` (AND) and `||` (OR).

Here is the general syntax:

```bash
if [[ -n $str1 ]] && [[ -z $str2 ]] ;
then
 commands;
fi
```

#### 💡 Example: Compound `if` Statement

Consider the following script:

```bash
hashim@Hashim:~$ str1="Not empty "
hashim@Hashim:~$ str2=""

hashim@Hashim:~$ if [[ -n $str1 ]] && [[ -z $str2 ]];
then
 echo "str1 is non-empty and str2 is empty string."
fi
```

##### Code Explanation

  * `str1="Not empty "`: Assigns a non-empty string to the variable `str1`.
  * `str2=""`: Assigns an empty string to the variable `str2`.
  * `if [[ -n $str1 ]] && [[ -z $str2 ]];`: This is the conditional check.
      * `[[ -n $str1 ]]`: Checks if `str1` is non-empty. This is **true**.
      * `&&`: The logical AND operator. It requires both sides to be true.
      * `[[ -z $str2 ]]`: Checks if `str2` is empty. This is **true**.
      * Since both conditions are true, the `then` block is executed.
  * `echo "str1 is non-empty and str2 is empty string."`: Prints the specified message to the console.

##### Output

```
str1 is non-empty and str2 is empty string.
```


### ⛓️ Compound Expressions as Command Chains

You will often see Bash scripts (especially installation scripts) that use compound expressions to perform multiple operations.

The **`&&` (AND)** operator is frequently used to "connect" multiple commands that must be executed sequentially (from left to right).

  * Each command in the sequence is executed **only if all preceding commands** in the sequence have executed successfully (i.e., returned an exit status of 0).
  * If the current command in the sequence **fails** (does not execute successfully), the remaining commands to the right of the failed command **will not be executed**.

#### 🚀 Practical Example: Command Chaining

Consider the following script. Before running it, try to predict the output\!

```bash
hashim@Hashim:~$ OLDDIR=`pwd`
hashim@Hashim:~$ cd /tmp
hashim@Hashim:~$ CURRDIR=`pwd`
hashim@Hashim:~$ echo "current directory: $CURRDIR"
hashim@Hashim:~$ mydir="/tmp/abc/def"
hashim@Hashim:~$ mkdir -p $mydir && cd $mydir && echo "now inside $mydir"
hashim@Hashim:~$ newdir=`pwd`
hashim@Hashim:~$ echo "new directory: $newdir"
hashim@Hashim:~$ echo "new directory: `pwd`"
hashim@Hashim:~$ cd $OLDDIR
hashim@Hashim:~$ echo "current directory: `pwd`"
```

##### Code Explanation (Line-by-Line)

  * `OLDDIR=`pwd\`\`: Stores the current working directory (e.g., `/home/hashim`) in the variable \`OLDDIR\`.
  * `cd /tmp`: Changes the directory to `/tmp`.
  * `CURRDIR=`pwd\`\`: Stores the new current directory (`/tmp`) in the variable \`CURRDIR\`.
  * `echo "current directory: $CURRDIR"`: Prints the value of `CURRDIR`.
  * `mydir="/tmp/abc/def"`: Assigns the string "/tmp/abc/def" to the variable `mydir`.
  * `mkdir -p $mydir && cd $mydir && echo "now inside $mydir"`: This is the command chain.
    1.  `mkdir -p $mydir`: Creates the directory `/tmp/abc/def`. The `-p` flag ensures that parent directories (`abc`) are created if they don't exist. This command succeeds.
    2.  `&&`: Since the first command succeeded, the next command is executed.
    3.  `cd $mydir`: Changes the current directory to `/tmp/abc/def`. This command succeeds.
    4.  `&&`: Since the second command succeeded, the final command is executed.
    5.  `echo "now inside $mydir"`: Prints the message "now inside /tmp/abc/def".
  * `newdir=`pwd\`\`: Stores the current directory (`/tmp/abc/def`) in the variable \`newdir\`.
  * `echo "new directory: $newdir"`: Prints the value of `newdir`.
  * ` echo "new directory:  `pwd`"`: Prints the current directory again, demonstrating the same value.
  * `cd $OLDDIR`: Changes the directory back to the originally saved directory (e.g., `/home/hashim`).
  * ` echo "current directory:  `pwd`"`: Prints the final directory to confirm the return.

##### Predicted Output

(Assuming your starting directory was `/home/hashim`)

```
current directory: /tmp
now inside /tmp/abc/def
new directory: /tmp/abc/def
new directory: /tmp/abc/def
current directory: /home/hashim
```

---

# 📁 File Test Operators in Bash

The Bash shell supports a wide range of operators designed to test various properties of files. These are essential for writing scripts that can safely check for files, directories, and permissions before attempting an operation.


### 📋 Operator Reference

Suppose that the variable `file` points to a non-empty text file that has read, write, and execute permissions. Here is a list of common file test operators:

  * **`-b file`**: Checks if `file` is a block special file (e.g., `/dev/sda`).
  * **`-c file`**: Checks if `file` is a character special file (e.g., `/dev/tty`).
  * **`-d file`**: Checks if `file` is a directory.
  * **`-e file`**: Checks if `file` exists.
  * **`-f file`**: Checks if `file` is an ordinary (regular) file.
  * **`-g file`**: Checks if `file` has its set group ID (SGID) bit set.
  * **`-k file`**: Checks if `file` has its "sticky bit" set.
  * **`-p file`**: Checks if `file` is a named pipe (FIFO).
  * **`-t file`**: Checks if `file` descriptor is open and associated with a terminal.
  * **`-u file`**: Checks if `file` has its set user id (SUID) bit set.
  * **`-r file`**: Checks if `file` is readable.
  * **`-w file`**: Checks if `file` is writeable.
  * **`-x file`**: Checks if `file` is executable.
  * **`-s file`**: Checks if `file` has a size greater than 0 (is non-empty).
  * **`f1 -nt f2`**: Checks if file `f1` is **n**ewer **t**han file `f2`.
  * **`f1 -ot f2`**: Checks if file `f1` is **o**lder **t**han file `f2`.


### 💡 Example: Testing File Existence (`-e`)

Here is a practical example of testing for the existence of a file using the `–e` option. We will check for a file that almost certainly exists on a Linux system.

```bash
hashim@Hashim:~$ fpath="/etc/passwd"
hashim@Hashim:~$ if [ -e $fpath ]; then
 echo "File exists";
else
 echo "Does not exist";
fi
```

#### Code Explanation

  * `fpath="/etc/passwd"`: This line assigns the string (the path) `/etc/passwd` to a variable named `fpath`.
  * `if [ -e $fpath ]; then`: This line begins the conditional block.
      * `[ ... ]`: This is the `test` command, which evaluates the expression inside it.
      * `-e $fpath`: This is the operator. It checks if the file pointed to by the `$fpath` variable (i.e., `/etc/passwd`) **e**xists.
  * `echo "File exists";`: This command is executed *only if* the test `[ -e $fpath ]` returns **true**.
  * `else`: This keyword specifies what to do if the `if` test returns **false**.
  * `echo "Does not exist";`: This command is executed *only if* the file `/etc/passwd` is not found.
  * `fi`: This keyword marks the end of the `if...else` block.

#### Expected Output

```
File exists
```


### 🔗 Compound Operators and File Operators

You can combine Boolean operators and file-related operators using the `&&` ("AND") or `||` ("OR") operators. This allows for more complex and specific tests.

The following example checks if a file **both exists AND is writable**.

#### Practical Example (From Scratch)

Let's first create a file at `/tmp/somedata` but make it *read-only* to see how the script handles this case.

```bash
hashim@Hashim:~$ fpath="/tmp/somedata"
hashim@Hashim:~$ touch $fpath
hashim@Hashim:~$ chmod -w $fpath
hashim@Hashim:~$ ls -l $fpath
-r--r--r-- 1 hashim hashim 0 Nov  5 09:05 /tmp/somedata
```

Now, we run the script to test this file:

```bash
hashim@Hashim:~$ if [ -e $fpath ] && [ -w $fpath ]
then
 echo "File $fpath exists and is writable"
else
 if [ ! -e $fpath ]
 then
echo "File $fpath does not exist "
 else
 echo "File $fpath exists but is not writable"
 fi
fi
```

#### Code Explanation

  * `touch $fpath`: Creates an empty file at `/tmp/somedata`.
  * `chmod -w $fpath`: Removes (`-`) the **w**rite permission from the file.
  * `if [ -e $fpath ] && [ -w $fpath ]`: This is the primary conditional test.
      * `[ -e $fpath ]`: Checks if `/tmp/somedata` exists. This is **true**.
      * `&&`: The logical **AND** operator. It will only proceed to the next test if the first one was true.
      * `[ -w $fpath ]`: Checks if `/tmp/somedata` is writable. This is **false** (because we ran `chmod -w`).
      * Since the full condition (true AND false) is **false**, the `then` block is skipped.
  * `else`: The code block after `else` is executed.
  * `if [ ! -e $fpath ]`: A new, nested `if` statement begins.
      * `!`: The logical **NOT** operator. It inverts the result of the test that follows.
      * `[ -e $fpath ]`: Checks if the file exists. This is **true**.
      * `[ ! -e $fpath ]`: The `!` inverts the result, making this expression **false**.
  * `then ...`: This block is **skipped**.
  * `else`: The `else` block of the *inner* `if` statement is executed.
  * `echo "File $fpath exists but is not writable"`: This line is printed to the terminal.
  * `fi`: Closes the inner `if...else` block.
  * `fi`: Closes the outer `if...else` block.

#### Expected Output

```
File /tmp/somedata exists but is not writable
```


### ⚠️ A Note on Compound Syntax

The syntax for compound operators can be tricky and varies between file, string, and numeric tests.

#### Incorrect Syntax

Notice the use of the `&&` operator **between** two separate `[ ... ]` test commands in the example above. The following syntax is **incorrect** because the Bash shell will not interpret it correctly:

```bash
# This is NOT valid
if [ -e $fpath -a -w $fpath ]
```

(Note: The original text contained a typo `–w`; the correct operator is `-w`. The original also used `-a`, which is a deprecated "AND" operator for `[ ]`. Using separate `[ ]` blocks with `&&` is the modern, preferred method.)

#### String Operator Syntax

Compound operators with **string** operators (using `[[ ... ]]`) also use the `&&` or `||` operators *between* the test blocks:

```bash
if [[ -n $str1 ]] && [[ -z $str2 ]] ;
```

#### Numeric Operator Syntax

However, compound operators with **numeric** operators *can* use the `-a` (AND) or `-o` (OR) flags within a *single* `[ ... ]` block.

```bash
[ $a -lt 20 -a $b -gt 100 ]
```

  * This single test checks if `$a` is **l**ess **t**han 20 **AND** (`-a`) if `$b` is **g**reater **t**han 100.
  * Even though this syntax is valid for numbers, many programmers still prefer the `[ $a -lt 20 ] && [ $b -gt 100 ]` syntax for better readability.

---

# 🧠 Conditional Logic with `if/else/fi` Statements

Bash supports conditional logic, allowing your scripts to make decisions. The syntax is a bit different from other programming languages, most notably using `then` and `fi` (which is just `if` spelled backward... clever, right?).


### 💡 Basic `if/else` Example

The following example shows how to use a basic `if/else/fi` statement. We will initialize a variable `x` with the value 25. The script will then check if `x` is less than 30 and print a different message depending on the result.

#### Practical Example

```bash
hashim@Hashim:~$ x=25
hashim@Hashim:~$ if [ $x -lt 30 ]
then
 echo "x is less than 30"
else
 echo "x is at least 30"
fi
```

#### ⚙️ Code Explanation (Line-by-Line)

  * `x=25`: Assigns the numeric value `25` to the variable `x`.
  * `if [ $x -lt 30 ]`: This is the conditional test.
      * `[ ... ]`: This is the `test` command.
      * `$x`: The variable's value (`25`) is substituted here.
      * `-lt`: This is a **numeric** comparison operator meaning "**l**ess **t**han".
      * The test becomes `[ 25 -lt 30 ]`, which evaluates to **true**.
  * `then`: Since the test was true, the code block after `then` is executed.
  * `echo "x is less than 30"`: Prints this message to the console.
  * `else`: This block is skipped because the `if` test was true.
  * `echo "x is at least 30"`: This line is *not* executed.
  * `fi`: This keyword marks the end of the `if` statement.

#### 📤 Output

```
x is less than 30
```


### 📜 Listing 4.2: `testvars.sh` (Checking Defined Variable)

This script, `testvars.sh`, checks if a variable `x` is defined (specifically, if it has a non-zero length).

```bash
#!/bin/bash
# testvars.sh

x="abc"
if [ -n "$x" ]
then
 echo "x is defined: $x"
else
 echo "x is not defined"
fi
```

#### ⚙️ Code Explanation (Line-by-Line)

Listing 4.2 initializes the variable `x` with the value "abc", and then uses the `if/else/fi` construct to determine whether `x` is initialized.

  * `x="abc"`: Assigns the string value "abc" to the variable `x`.
  * `if [ -n "$x" ]`: This is the conditional test.
      * `-n`: This is a **string** test operator that checks if the string has a **n**on-zero length.
      * `"$x"`: The quotes are important. They ensure the script works correctly even if the variable contains spaces or is empty. The variable `$x` ("abc") is substituted here.
      * The test becomes `[ -n "abc" ]`, which is **true** because "abc" has a length of 3.
  * `then`: Since the test was true, this block is executed.
  * `echo "x is defined: $x"`: Prints the message, replacing `$x` with its value.
  * `else`: This block is skipped.
  * `echo "x is not defined"`: This line is *not* executed.
  * `fi`: Marks the end of the `if` statement. (Note: The original text's `f` was corrected to `fi`).

#### 📤 Output

Launch the shell script in Listing 4.2, and you will see the following output:

```
X is defined: abc
```


### 📜 Listing 4.3: `testvars2.sh` (Checking Undefined Variable)

This script, `testvars2.sh`, checks if the variable `y` is undefined (or, more accurately, if it's an empty string).

```bash
#!/bin/bash
# testvars2.sh

if [ -z "$y" ]
then
 y="def"
 echo "y is defined: $y"
else
 echo "y is defined: $y"
fi
```

#### ⚙️ Code Explanation (Line-by-Line)

Listing 4.3 first checks whether the variable `y` is defined.

  * `if [ -z "$y" ]`: This is the conditional test.
      * `$y`: Since `y` has not been defined yet, Bash substitutes an empty string (`""`).
      * `-z`: This is a **string** test operator that checks if the string has a **z**ero length.
      * The test becomes `[ -z "" ]`, which evaluates to **true**.
  * `then`: Since the test was true, the `then` block is executed.
  * `y="def"`: The variable `y` is assigned the string value "def".
  * `echo "y is defined: $y"`: This command is executed. It prints the message, replacing `$y` with its *new* value ("def").
  * `else`: This block is skipped.
  * `echo "y is defined: $y"`: This line is *not* executed.
  * `fi`: Marks the end of the `if` statement.

#### 📤 Output

Launch the shell script in Listing 4.3 and you will see the following output:

```
y is defined: def
```


---

# 🔄 The `case/esac` Statement

The `case/esac` statement is Bash's counterpart to the `switch` statement found in many other programming languages. It's a clean way to test a single variable against a variety of conditions.

This statement is particularly powerful because the conditions can include metacharacters (wildcards). A common scenario involves testing user input, such as checking if a user entered a string that starts with an uppercase or lowercase "n" (for no) or "y" (for yes).

The statement starts with `case $variable in` and ends with `esac` (which is just `case` backward).


### 📜 Listing 4.4: `case1.sh`

Listing 4.4 displays the content of `case1.sh`, which checks various conditions against a hardcoded variable.

```bash
#!/bin/bash
# case1.sh

x="abc"
case $x in
 a) echo "x is an a" ;;
 c) echo "x is a c" ;;
 a*) echo "x starts with a" ;;
 *) echo "no matches occurred" ;;
esac
```

#### ⚙️ Code Explanation

Listing 4.4 starts by initializing the variable `x` with the value "abc". This is followed by the `case` keyword, which checks the value of `$x` against several patterns.

  * `x="abc"`: Assigns the string "abc" to the variable `x`.
  * `case $x in`: This begins the `case` statement, indicating that the value of `$x` ("abc") will be tested.
  * `a) echo "x is an a" ;;`: This is the first pattern.
      * `a)`: Checks if `$x` is *exactly* equal to "a". It is not.
      * `;;`: This double semicolon acts like a `break` statement. If this pattern had matched, execution would stop here.
  * `c) echo "x is a c" ;;`: This is the second pattern. It checks if `$x` is *exactly* equal to "c". It is not.
  * `a*) echo "x starts with a" ;;`: This is the third pattern.
      * `a*)`: This pattern uses a **metacharacter**. The `*` acts as a wildcard, matching "zero or more of any character."
      * The pattern "a\*" therefore means "anything that starts with the letter `a`".
      * The value "abc" matches this pattern.
      * `echo "x starts with a"`: This line is executed.
  * `*) echo "no matches occurred" ;;`: This is the fourth pattern.
      * `*)`: The `*` by itself is the **default case**. It matches *anything*.
      * This pattern is only executed if no other pattern above it matches. Since `a*` already matched, this line is skipped.
  * `esac`: This keyword ends the `case` statement.

#### 📤 Output

When you launch the shell script in Listing 4.4, you will see the following output:

```
x starts with a
```


### 📜 Listing 4.5: `UserInfo.sh`

Listing 4.5 shows you how to prompt users for an input string and then process that input using a `case/esac` statement.

```bash
#!/bin/bash
# UserInfo.sh

echo -n "Please enter your first name: "
read fname
echo -n "Please enter your last name: "
read lname
echo -n "Please enter your city: "
read city
fullname="$fname $lname"
echo "$fullname lives in $city"
case $city in
 San*) echo "$fullname lives in California " ;;
 Chicago) echo "$fullname lives in the Windy City " ;;
 *) echo "$fname lives in la-la land " ;;
esac
```

#### ⚙️ Code Explanation

Listing 4.5 starts by prompting users for their first name, last name, and city, and then assigning those values to the variables `fname`, `lname`, and `city`, respectively.

  * `echo -n "..."`: The `-n` flag tells `echo` **not** to print a newline character at the end. This keeps the cursor on the same line as the prompt, which is where the user will type.
  * `read fname`: This command pauses the script and waits for the user to type something and press Enter. Whatever they type is stored in the `fname` variable.
  * `(commands repeated for lname and city)`
  * `fullname="$fname $lname"`: This line defines a new variable, `fullname`, by concatenating (joining) the values of `$fname` and `$lname` with a space in between.
  * `echo "$fullname lives in $city"`: This prints a summary.
  * `case $city in`: This begins the `case` statement, testing the value stored in the `$city` variable.
  * `San*) ... ;;`: This pattern checks if the `$city` variable **starts with** "San" (e.g., "San Francisco", "San Diego").
  * `Chicago) ... ;;`: This pattern checks if the `$city` variable is **exactly** "Chicago".
  * `*) ... ;;`: This is the default "catch-all" option, which is true if both of the preceding conditions are false.

#### 🚀 Practical Example (From Scratch)

Let's make the script executable and run it twice to see different outputs.

```bash
hashim@Hashim:~$ chmod +x UserInfo.sh

# --- First Run (matching the wildcard) ---
hashim@Hashim:~$ ./UserInfo.sh
Please enter your first name: Jane
Please enter your last name: Doe
Please enter your city: San Diego
Jane Doe lives in San Diego
Jane Doe lives in California 

# --- Second Run (matching the default) ---
hashim@Hashim:~$ ./UserInfo.sh
Please enter your first name: John
Please enter your last name: Smith
Please enter your city: New York
John Smith lives in New York
John lives in la-la land 
```


### 📜 Listing 4.6: `StartChar.sh`

Listing 4.6 displays the content of `StartChar.sh`, which checks the type of the first character of a user-provided string.

```bash
#!/bin/bash
# StartChar.sh

while (true)
do
 echo -n "Enter a string: "
 read var
 case ${var:0:1} in
 [0-9]*) echo "$var starts with a digit" ;;
 [A-Z]*) echo "$var starts with an uppercase letter" ;;
 [a-z]*) echo "$var starts with a lowercase letter" ;;
 *) echo "$var starts with another symbol" ;;
 esac
done
```

#### ⚙️ Code Explanation

Listing 4.6 starts by prompting users for a string and then initializes the variable `var` with that input string.

  * `while (true) do ... done`: This creates an **infinite loop**. The code inside this loop will repeat forever until you manually stop the script (e.g., by pressing `Ctrl+Z`).
  * `echo -n "Enter a string: "`: Prompts the user for input.
  * `read var`: Reads the user's input into the `var` variable.
  * `case ${var:0:1} in`: This is the key part of the script.
      * `${var:0:1}`: This is **Bash Substring Expansion**. It extracts a substring from the `$var` variable. The `0` is the starting position (the first character), and the `1` is the length (one character).
      * This expression effectively gives the `case` statement *only the first character* of the string.
  * `[0-9]*) ... ;;`: This pattern checks if the first character is a digit.
      * `[0-9]`: This is a **character class** that matches any single digit from 0 through 9.
      * `*`: The wildcard is included, so this pattern technically means "starts with a digit."
  * `[A-Z]*) ... ;;`: This pattern checks if the first character is an uppercase letter (A through Z).
  * `[a-z]*) ... ;;`: This pattern checks if the first character is a lowercase letter (a through z). (The typo "lowe" from the original text has been corrected).
  * `*) ... ;;`: The default condition, which is executed if none of the preceding conditions (digit, uppercase, lowercase) is true.

#### 🚀 Sample Interaction

```
hashim@Hashim:~$ ./StartChar.sh
Enter a string: Hello
Hello starts with an uppercase letter
Enter a string: world
world starts with a lowercase letter
Enter a string: 99problems
99problems starts with a digit
Enter a string: !Stop
!Stop starts with another symbol
Enter a string: ^Z
hashim@Hashim:~$ 
```


### 📜 Listing 4.7: `StartChar2.sh`

Listing 4.7 displays the content of `StartChar2.sh`, which checks the type of the first *pair* of characters of a user-provided string.

```bash
#!/bin/bash
# StartChar2.sh

while (true)
do
 echo -n "Enter a string: "
 read var
 case ${var:0:2} in
 [0-9][0-9]) echo "$var starts with two digits" ;;
 [A-Z][A-Z]) echo "$var starts with two uppercase letters" ;;
 [a-z][a-z]) echo "$var starts with two lowercase letters" ;;
 *) echo "$var starts with another pattern" ;;
 esac
done
```

#### ⚙️ Code Explanation

Listing 4.7 starts with a `while` loop whose contents are similar to Listing 4.6.

  * `while (true)`: This creates an infinite loop. The script repeats indefinitely, and you can press `Ctrl+Z` to terminate its execution.
  * `case ${var:0:2} in`: This is the main difference. It uses substring expansion to get the first **two** characters (`0` = start, `2` = length).
  * `[0-9][0-9]) ... ;;`: This pattern checks for two consecutive digits (e.g., "12", "99").
  * `[A-Z][A-Z]) ... ;;`: This pattern checks for two consecutive uppercase letters (e.g., "HE", "OK").
  * `[a-z][a-z]) ... ;;`: This pattern checks for two consecutive lowercase letters (e.g., "he", "ok").
  * `*) ... ;;`: This default pattern will match *any* other two-character combination (e.g., "a1", "1a", "A-", or even a string with only one character).

#### 🚀 Sample Interaction

```
hashim@Hashim:~$ ./StartChar2.sh
Enter a string: 12monkeys
12monkeys starts with two digits
Enter a string: 1hello
1hello starts with another pattern
Enter a string: GOteam
GOteam starts with two uppercase letters
Enter a string: myScript
myScript starts with two lowercase letters
Enter a string: ^Z
hashim@Hashim:~$ 
```


### 📜 Listing 4.8: `StartChar3.sh`

Listing 4.8 displays the content of `StartChar3.sh`, which checks the type of the first character using an alternative, more robust syntax.

```bash
#!/bin/bash
# StartChar3.sh

while (true)
do
 echo -n "Enter a string: "
 read var
 case ${var:0:1} in
 [0-9]*) echo "$var starts with a digit" ;;
 [[:upper:]]) echo "$var starts with an uppercase letter" ;;
 [[:lower:]]) echo "$var starts with a lowercase letter" ;;
 *) echo "$var starts with another symbol" ;;
 esac
done
```

#### ⚙️ Code Explanation

Listing 4.8 also starts with a `while` loop that contains a `case/esac` statement. The conditions again test the first character (`${var:0:1}`).

  * `[0-9]*) ... ;;`: This is the same digit check as in Listing 4.6.
  * `[[:upper:]]) ... ;;`: This is the key difference. `[[:upper:]]` is a **POSIX character class**. It is generally preferred over `[A-Z]` because it is **locale-aware**. This means it will correctly identify uppercase letters in other languages (like `É` or `Ü`), not just the 26 letters of the English alphabet.
  * `[[:lower:]]) ... ;;`: Similarly, `[[:lower:]]` is the POSIX class for lowercase letters and is also locale-aware.
  * `*) ... ;;`: The default "catch-all" case.
  * This code sample also repeats indefinitely, and you can press `Ctrl+Z` to terminate the execution of the shell script.

---

# ✂️ Working with Strings in Shell Scripts

When working with Bash, you often need to manipulate strings—specifically, to extract parts of them. Bash provides a powerful "curly brackets" syntax (`{...}`) for this purpose, which is known as **Substring Expansion**.

Listing 4.9 displays the content of `substrings.sh` to illustrate several examples of this syntax.

### 📜 Listing 4.9: `substrings.sh`

This script demonstrates how to find substrings of a given string.

```bash
#!/bin/bash
# substrings.sh

x="abcdefghij"

echo ${x:0}
echo ${x:1}
echo ${x:5}
echo ${x:7:3}
echo ${x:-4}
echo ${x:(-4)}
echo ${x: -4}
```

### 📤 Script Output

Running the preceding script will produce this output:

```
abcdefghij
bcdefghij
fghij
hij
abcdefghij
ghij
ghij
```

### ⚙️ Code Explanation (Line-by-Line)

Listing 4.9 first initializes the variable `x` with the string "abcdefghij". Let's break down each `echo` statement to understand the syntax `${variable:offset:length}`.

  * `echo ${x:0}`

      * **Syntax:** `${variable:offset}`
      * **Explanation:** This extracts a substring from `$x` starting at index `0` (the very beginning) and continuing to the end of the string.
      * **Result:** `abcdefghij`

  * `echo ${x:1}`

      * **Syntax:** `${variable:offset}`
      * **Explanation:** This extracts a substring starting at index `1` (the second character, 'b') and continuing to the end.
      * **Result:** `bcdefghij`

  * `echo ${x:5}`

      * **Syntax:** `${variable:offset}`
      * **Explanation:** This extracts a substring starting at index `5` (the sixth character, 'f') and continuing to the end.
      * **Result:** `fghij`

  * `echo ${x:7:3}`

      * **Syntax:** `${variable:offset:length}`
      * **Explanation:** This is the first example with a specified length. It starts at index `7` (the eighth character, 'h') and extracts a substring that is `3` characters long.
      * **Result:** `hij`

### ⚠️ A Note on Negative Offsets

The next portion of Listing 4.9 contains three `echo` statements that specify negative values for the offset. A negative offset means the index position is calculated from the **right** (the end of the string) instead of the left.

However, the syntax is very sensitive and has a "gotcha" you must know.

  * `echo ${x:-4}`

      * **Explanation:** This **does not** work as intended. Bash interprets `${variable:-word}` as a different kind of expansion: "if `$variable` is unset or null, use the default value `word`."
      * Because our variable `$x` *is* set, Bash just returns the entire string. The "missing whitespace" after the colon (`:`) is the problem.
      * **Result:** `abcdefghij`

  * `echo ${x:(-4)}`

      * **Explanation:** This is one correct way to specify a negative offset. By wrapping the negative number in parentheses, you are telling Bash to treat it as a calculation (an arithmetic expression) and not as the "default value" syntax.
      * It starts `4` characters from the right ('g') and extracts to the end.
      * **Result:** `ghij`

  * `echo ${x: -4}`

      * **Explanation:** This is the *other* correct way to specify a negative offset. Adding a **space** between the colon (`:`) and the negative sign (`-`) distinguishes it from the `${var:-default}` syntax.
      * This expression also refers to the right-most 4 characters in the variable `x`.
      * **Result:** `ghij`

---

# ⚙️ Working with Loops

The Bash shell provides powerful constructs for iteration. These tools allow you to repeat actions on a set of values, such as items in an array or a list of file names.

Several loop constructs are available in Bash:

  * `for` loop
  * `while` loop
  * `until` loop

The following sections will illustrate how to use each of these constructs.

## 🔄 Using a `for` Loop

The Bash shell supports the **for loop**, which has a syntax that is slightly different from other programming languages like JavaScript or Java.

### Example 1: Basic File Iteration

The following code block demonstrates how to use a `for` loop to iterate through a set of files in the current directory.

#### 🛠️ Setup (From Scratch)

Before running the script, we need some files to work with. Let's create a few sample `.txt` files and one `.pdf` file.

```bash
# Create three text files
touch report_final.txt
touch user_list.txt
touch notes.txt

# Create one PDF file (which the loop should ignore)
touch manual.pdf
```

#### 📜 Script

This script will find all files ending in `.txt` and print their names.

```bash
#!/bin/bash

# This loop finds all files ending in .txt
# The `ls *txt` command is executed first.
# The loop then iterates over the output of that command.

for f in `ls *txt`
do
 echo "file: $f"
done
```

#### 🔍 Code Explanation

  * `for f in `ls *txt: This line initiates the loop.
      * `ls *txt`: This command lists all files in the current directory that end with the `.txt` extension.
      * **(Backticks)**: These are used for **command substitution**. The shell first executes the `ls *txt` command. The output (e.g., `notes.txt report_final.txt user_list.txt`) becomes the list of items the loop will iterate over.
      * `for f in ...`: For each item in that generated list, the shell assigns the item (e.g., `notes.txt`) to the variable `f` and executes the loop body.
  * `do`: This keyword marks the beginning of the block of commands that will be executed in each iteration.
  * `echo "file: $f"`: This command prints the literal string "file: " followed by the value currently stored in the `$f` variable (the filename for the current iteration).
  * `done`: This keyword marks the end of the loop's body.

#### 🖥️ Output

When you execute the script in the directory containing the setup files, the output will be:

```plaintext
file: notes.txt
file: report_final.txt
file: user_list.txt
```

> **Note:** The output of this `for` loop depends on the files with the suffix `txt` that are in the directory. If there are no such files, the `ls *txt` command might return an error or empty string (depending on shell configuration), and the `for` loop will do nothing.

### Example 2: Renaming Files (Listing 4.10: renamefiles.sh2)

This listing provides a practical example of how to iterate through files and rename them by changing their suffix.

#### 🛠️ Setup (From Scratch)

Let's create some script files (ending in `.sh`) that we intend to rename to `.txt`.

```bash
# Create three shell script files
touch deploy_app.sh
touch backup_db.sh
touch check_disk.sh

# Create one text file (which the loop will ignore)
touch readme.txt
```

#### 📜 Script (renamefiles.sh2)

```bash
#!/bin/bash

# Define the new suffix we want to use
newsuffix="txt"

# Loop through all files ending in .sh
for f in `ls *sh`
do
 # Extract the base name (part before the dot)
 base=`echo $f | cut -d"." -f1`
 
 # Extract the suffix (part after the dot)
 suffix=`echo $f | cut -d"." -f2`
 
 # Create the new filename by combining the base name and new suffix
 newfile="${base}.${newsuffix}"
 
 # Print the old and new names
 echo "file: $f new: $newfile"
 
 # You can uncomment one of the following lines
 # to actually perform the file operation.
 
 # either 'cp' or 'mv' the file
 # mv $f $newfile
 # cp $f $newfile
done
```

#### 🔍 Code Explanation (Line-by-Line)

  * `newsuffix="txt"`: This line initializes a variable named `newsuffix` and assigns it the string value `txt`.
  * `for f in \`ls \*sh\`` : This loop finds and iterates through all files in the current directory ending with the  `.sh`suffix. In each iteration, the variable`f`will hold one filename (e.g.,`deploy\_app.sh\`).
  * `do`: Signals the beginning of the loop's body.
  * `base=\`echo $f | cut -d"." -f1\`` : This line assigns the "base" part of the filename to the  `base\` variable.
      * `echo $f`: Prints the current filename (e.g., `deploy_app.sh`).
      * `|` (Pipe): Sends the output of the `echo` command as the input to the `cut` command.
      * `cut -d"." -f1`: This is the `cut` utility.
          * `-d"."`: Sets the **d**elimiter (the separator) to a period (`.`).
          * `-f1`: Selects **f**ield number 1 (the part *before* the first delimiter).
      * The result (e.g., `deploy_app`) is assigned to the `base` variable.
  * `suffix=\`echo $f | cut -d"." -f2\`\`: This line is similar to the previous one, but it extracts the suffix.
      * `-f2`: Selects **f**ield number 2 (the part *after* the first delimiter).
      * The result (e.g., `sh`) is assigned to the `suffix` variable. (Note: This variable is not actually used in the rest of this script, but it shows how to isolate the original extension).
  * `newfile="${base}.${newsuffix}"`: This line constructs the new filename.
      * It concatenates (joins) the value of `$base` (`deploy_app`), a literal period (`.`), and the value of `$newsuffix` (`txt`).
      * The final string (e.g., `deploy_app.txt`) is assigned to the `newfile` variable.
  * `echo "file: $f new: $newfile"`: This command displays the value of the original filename (`$f`) and the newly constructed filename (`$newfile`).
  * `#mv $f $newfile`: This is a **commented-out** command. If you remove the `#`, the `mv` (move) command would **rename** the original file to the new name.
  * `#cp $f $newfile`: This is also a comment. If you remove the `#`, the `cp` (copy) command would create a **new file** with the new name, leaving the original file intact.

#### 🖥️ Output

Running the script as-is (without uncommenting `mv` or `cp`) only prints what it *would* do:

```plaintext
file: backup_db.sh new: backup_db.txt
file: check_disk.sh new: check_disk.txt
file: deploy_app.sh new: deploy_app.txt
```

---

# 📁 Checking Files in a Directory

This code sample features a `for` loop designed to examine and report various properties of the files located within the current directory.

## 📜 Listing 4.11: `checkdir.sh` (Modern Version)

This script, `checkdir.sh`, is built to count the number of directories, executable files, readable files, and ASCII files in the current directory. This version uses modern, preferred Bash syntax.

```bash
#!/bin/bash
# initialize 'counter' variables
TOTAL_FILES=0
ASCII_FILES=0
NONASCII_FILES=0
READABLE_FILES=0
EXEC_FILES=0
DIRECTORIES=0

# Use $(ls) for modern command substitution
for f in $(ls)
do
 # Use modern arithmetic ((...))
 ((TOTAL_FILES++))
 
 if [ -d "$f" ]
 then
  ((DIRECTORIES++))
 fi
 
 if [ -x "$f" ]
 then
  ((EXEC_FILES++))
 fi
 
 if [ -r "$f" ]
 then
  ((READABLE_FILES++))
 fi
 
 # Store command output in variables
 readable=$(file "$f")
 ascii=$(echo "$readable" | grep ASCII)
 
 if [ "$ascii" != "" ]
 then
  ((ASCII_FILES++))
 else
  #echo "readable: $readable"
  ((NONASCII_FILES++))
 fi
done

# results:
echo "TOTAL_FILES: $TOTAL_FILES"
echo "DIRECTORIES: $DIRECTORIES"
echo "EXEC_FILES: $EXEC_FILES"
echo "ASCII_FILES: $ASCII_FILES"
echo "NON-ASCII_FILES: $NONASCII_FILES"
```


### 👨‍💻 Code Explanation

Here is a detailed breakdown of the **modern** script:

  * `#!/bin/bash`: This is the "shebang." It tells the system to execute this script using the Bash shell.
  * `# initialize 'counter' variables`: A comment indicating the purpose of the following lines.
  * `TOTAL_FILES=0` ... `DIRECTORIES=0`: These lines initialize six variables as counters, setting them all to `0`.
  * `for f in $(ls)`: This line starts a `for` loop.
      * `$(ls)`: This is **modern command substitution**. It executes the `ls` command and returns the list of files and directories. It is preferred over the old backticks `` `ls` ``.
      * The loop then iterates one by one through each item (`f`) returned by `ls`.
  * `do`: Marks the beginning of the code block that will execute for each file.
  * `((TOTAL_FILES++))`: This is **modern Bash arithmetic**. It's a cleaner and more efficient way to increment the `TOTAL_FILES` counter by one. It replaces the old `expr` command.
  * `if [ -d "$f" ]`: This is a conditional test.
      * `[ -d "$f" ]` checks if the current item (`$f`) is a directory. (Quoting `"$f"` is good practice to handle filenames with spaces).
  * `((DIRECTORIES++))`: If the test is true (the file is a directory), this line increments the `DIRECTORIES` counter.
  * `if [ -x "$f" ]`: This test checks if the current file (`$f`) is executable.
  * `((EXEC_FILES++))`: If true, the `EXEC_FILES` counter is incremented.
  * `if [ -r "$f" ]`: This test checks if the current file (`$f`) is readable.
  * `((READABLE_FILES++))`: If true, the `READABLE_FILES` counter is incremented.
  * `readable=$(file "$f")`: This line executes the `file` command on the current item. The `file` command analyzes a file and reports its type (e.g., "ASCII text"). The output text is stored in the `readable` variable.
  * `ascii=$(echo "$readable" | grep ASCII)`: This line processes the output from the previous step.
      * `echo "$readable"` prints the content of the `readable` variable.
      * `|` (pipe) sends that output as input to the `grep` command.
      * `grep ASCII` searches the input for the literal string "ASCII".
      * If "ASCII" is found, `grep` outputs that line, which is then stored in the `ascii` variable. If not, the `ascii` variable will be empty.
  * `if [ "$ascii" != "" ]`: This test checks if the `ascii` variable is **not** empty.
  * `((ASCII_FILES++))`: If the variable is not empty (meaning "ASCII" was found), the `ASCII_FILES` counter is incremented.
  * `else`: If the `if` test was false (meaning "ASCII" was not found).
  * `((NONASCII_FILES++))`: The `NONASCII_FILES` counter is incremented.
  * `done`: Marks the end of the `for` loop. The script will return to the `for` line to process the next file until all files are done.
  * `# results:`: A comment indicating the final output section.
  * `echo "TOTAL_FILES: $TOTAL_FILES"` ... `echo "NON-ASCII_FILES: $NONASCII_FILES"`: These lines print the final values of all the counters to the console.


### 📊 Script Analysis

Listing 4.11 first initializes several count-related variables. This is followed by a `for` loop that iterates through all the files present in the current directory.

The body of this loop contains multiple conditional code blocks. These blocks are used to determine specific properties of the current file being processed:

  * Is the file a directory?
  * Is the file executable?
  * Is the file readable?
  * Is the file an ASCII file?

The last portion of Listing 4.11 is responsible for displaying the final values of these count-related variables after the loop has finished processing all files.


### 🚀 How to Create and Run This Script

Here’s how you can create and execute the `checkdir.sh` script on your system.

**1. Create the Script File using `nano`**

Open your terminal and type the following command. This will create and open a new file named `checkdir.sh` in the `nano` text editor:

```bash
nano checkdir.sh
```

**2. Paste the Code**

Copy the entire **modern** shell script code from **Listing 4.11** (above) and paste it into the `nano` editor window.

To save and exit `nano`:

  * Press **`Ctrl + O`** (Write Out) and then press **`Enter`** to save the file.
  * Press **`Ctrl + X`** to exit `nano`.

**3. Make the Script Executable**

By default, the new file does not have permission to be executed. You must grant this permission using the `chmod` command:

```bash
chmod +x checkdir.sh
```

  * `chmod`: The command to "change mode" (permissions).
  * `+x`: This part **adds** the **executable** permission.
  * `checkdir.sh`: The file you are applying the permission to.

**4. Run the Script**

Now you can run the script. Make sure you are in the same directory where you saved the file. Type the following in your terminal:

```bash
./checkdir.sh
```

  * `./`: This tells the shell to look for the script in the **current directory**.


### 🖥️ Example Output

When you run the script, it will analyze the files in its *current directory*. Your output will look something like this. The exact numbers will change depending on what files and directories you have in that location.

```bash
TOTAL_FILES: 33
DIRECTORIES: 1
EXEC_FILES: 26
ASCII_FILES: 29
NON-ASCII_FILES: 4
```

---

Here is the text converted into a professional Markdown file, complete with detailed explanations and output as requested.


# ➿ Working with Nested Loops

This section is mainly for fun and demonstrates how to use a nested loop to display a "triangular" output. This code sample uses a Bash array with a pair of values to create the pattern.

(Note: Arrays in Bash are a larger topic, discussed in detail in the final section of this chapter.)

## 📜 Listing 4.12: `nestedloops.sh`

This script illustrates how to display an alternating set of symbols in a triangular fashion, first growing and then shrinking.

```bash
#!/bin/bash

outermax=10
symbols[0]="#"
symbols[1]="@"

# First loop: The "growing" triangle
for (( i=1; i<${outermax}; i++ ));
do
 for (( j=1; j<${i}; j++ ));
 do
  # Print the symbol
  printf "%-2s" ${symbols[($i+$j)%2]}
 done
 # At the end of the inner loop, print a new line
 printf "\n"
done

# Second loop: The "shrinking" triangle
for (( i=1; i<${outermax}; i++ ));
do
 for (( j=${i}+1; j<${outermax}; j++ ));
 do
  # Print the symbol
  printf "%-2s" ${symbols[($i+$j)%2]}
 done
 # At the end of the inner loop, print a new line
 printf "\n"
done
```


### 👨‍💻 Code Explanation

This script initializes some variables and then uses two main `for` loops, each containing a nested (inner) loop.

  * `#!/bin/bash`: The shebang, telling the system to use the Bash shell.
  * `outermax=10`: Initializes a variable that controls the maximum "width" or number of rows for the triangles.
  * `symbols[0]="#"`: Initializes the first element (at index 0) of an array named `symbols`.
  * `symbols[1]="@"`: Initializes the second element (at index 1) of the `symbols` array.

#### The First Loop (Growing Triangle)

  * `for (( i=1; i<${outermax}; i++ ))`: This is the **outer loop**, controlled by the variable `i`. It will run 9 times (for `i` = 1, 2, 3... 9).
  * `for (( j=1; j<${i}; j++ ))`: This is the **inner loop**, controlled by `j`. Critically, its end point depends on the *current value of `i`*.
      * When `i` is 1, `j` runs from 1 to 0 (it doesn't run).
      * When `i` is 2, `j` runs for `j=1`.
      * When `i` is 3, `j` runs for `j=1` and `j=2`.
      * This is what makes the triangle "grow" wider with each line.
  * `printf "\n"`: After the inner loop finishes (a full row is printed), this command prints a newline character, moving the cursor to the next line.

#### The Second Loop (Shrinking Triangle)

  * `for (( i=1; i<${outermax}; i++ ))`: The **outer loop**, identical to the first, `i` runs from 1 to 9.
  * `for (( j=${i}+1; j<${outermax}; j++ ))`: The **inner loop**. The logic here is reversed. `j` starts *one value higher* than `i` and runs up to `outermax` (10).
      * When `i` is 1, `j` runs from 2 to 9 (8 symbols).
      * When `i` is 2, `j` runs from 3 to 9 (7 symbols).
      * ...
      * When `i` is 9, `j` runs from 10 to 9 (it doesn't run).
      * This is what makes the triangle "shrink" with each line.


### 🔣 How the Alternating Symbol Works

The key to the script is this line:

```bash
printf "%-2s" ${symbols[($i+$j)%2]}
```

Let's break it down from the inside out:

1.  **`($i+$j)%2`**: This is the core logic.

      * `$i+$j`: The sum of the outer and inner loop counters.
      * `%2`: This is the **modulo operator**. It finds the remainder after dividing by 2.
      * If `$i+$j` is an **even** number, the result is **`0`**.
      * If `$i+$j` is an **odd** number, the result is **`1`**.

2.  **`${symbols[... ]}`**: This part accesses the `symbols` array.

      * When the result of the modulo is `0`, it becomes `symbols[0]`, which is **`#`**.
      * When the result of the modulo is `1`, it becomes `symbols[1]`, which is **`@`**.

3.  **`printf "%-2s"`**: This is a formatting command.

      * `%s` means "print a string" (our symbol).
      * `-2` means "make the total width 2 characters, and left-justify (the `-` sign)".
      * This ensures each symbol takes up two spaces (e.g., "` @  `"), keeping the columns perfectly aligned.


### 📈 Generalizing the Script

You can easily generalize this code. If your `symbols` array contains `arrlength` elements, you would simply change the modulo operator.

For example, if you had 3 symbols (`arrlength=3`):
`symbols[0]="#"`
`symbols[1]="@"`
`symbols[2]="%"`

You would replace the print snippet with the following:

```bash
printf "%-2s" ${symbols[($i+$j)% $arrlength]}
```

In this case, `printf "%-2s" ${symbols[($i+$j)%3]}`.


### 🚀 How to Create and Run This Script

You can create and execute the `nestedloops.sh` script on your system.

**1. Create the Script File**

Open your terminal and use a text editor like `nano` to create the file:

```bash
nano nestedloops.sh
```

**2. Paste the Code**

Copy the entire script from **Listing 4.12** and paste it into the `nano` editor.

To save and exit `nano`:

  * Press **`Ctrl + O`** (Write Out) and then press **`Enter`**.
  * Press **`Ctrl + X`** to exit.

**3. Make the Script Executable**

You must give the script permission to run using the `chmod` command:

```bash
chmod +x nestedloops.sh
```

**4. Run the Script**

Execute the script by typing its name:

```bash
./nestedloops.sh
```


### 🖥️ Expected Output

When you launch the code, you will see the following triangular output printed to your console:

```bash
@ 
# @ 
@ # @ 
# @ # @ 
@ # @ # @ 
# @ # @ # @ 
@ # @ # @ # @ 
# @ # @ # @ # @ 
@ # @ # @ # @ # 
@ # @ # @ # @ 
@ # @ # @ # 
@ # @ # @ 
@ # @ # 
@ # @ 
@ # 
@ 
```

---

# 🔄 **Using a While Loop**

A `while` loop is a fundamental control structure that continues to execute a block of code *as long as* a specified condition remains true. Let's explore two examples.

## 📜 Listing 4.13: `while1.sh` (Modern Version)

This script illustrates a basic `while` loop that iterates through a set of numbers. It uses a `break` statement to exit what would otherwise be an infinite loop. This version uses modern Bash arithmetic.

```bash
#!/bin/bash
x=0
((x++)) # Modern way to increment x
echo "new x: $x"

while (true)
do
 echo "x = $x"
 ((x++)) # Modern way to increment x
 
 if [ $x -gt 4 ]
 then
  break
 fi
done
```


### 👨‍💻 Code Explanation

This script first initializes the variable `x` to 0.

  * `x=0`: Assigns the integer `0` to the variable `x`.
  * `((x++))`: This is **modern Bash arithmetic**. It's the clean and efficient way to increment the value of `x` by 1.
  * `echo "new x: $x"`: This prints the new value of `x` (which is 1) to the console.

The next portion is the `while` loop.

  * `while (true)`: This starts an **infinite loop** by design, as the condition `true` never changes.
  * `do`: Marks the beginning of the code block to be executed with each loop iteration.
  * `echo "x = $x"`: Inside the loop, this prints the *current* value of `x`.
  * `((x++))`: Just like before, this increments `x` by 1.
  * `if [ $x -gt 4 ]`: This is the escape hatch. It's a conditional test:
      * `[ ... ]` is the `test` command.
      * `$x -gt 4` checks if the value of `x` is **g**reater **t**han 4.
  * `then break fi`: If the test is true (i.e., `x` becomes 5), the `break` command is executed, immediately terminating the `while` loop.


### 🚀 How to Create and Run This Script

**1. Create the Script File using `nano`**

Open your terminal and type:

```bash
nano while1.sh
```

**2. Paste the Code**

Copy the code from **Listing 4.13** and paste it into the `nano` editor.

To save and exit `nano`:

  * Press **`Ctrl + O`** (Write Out) and then press **`Enter`** to save.
  * Press **`Ctrl + X`** to exit.

**3. Make the Script Executable**

Give the script permission to run:

```bash
chmod +x while1.sh
```

**4. Run the Script**

Execute the script from your terminal:

```bash
./while1.sh
```


### 🖥️ Expected Output

When you run the script, you will immediately see the following output printed to your console.

```bash
new x: 1
x = 1
x = 2
x = 3
x = 4
```

Here is a play-by-play of the execution:

1.  **Before Loop:** `x` is set to 0, then 1. The script prints `new x: 1`.
2.  **Loop 1:** `while (true)` is true.
      * Prints `x = 1`.
      * `x` becomes 2.
      * Is 2 \> 4? No.
3.  **Loop 2:** `while (true)` is true.
      * Prints `x = 2`.
      * `x` becomes 3.
      * Is 3 \> 4? No.
4.  **Loop 3:** `while (true)` is true.
      * Prints `x = 3`.
      * `x` becomes 4.
      * Is 4 \> 4? No.
5.  **Loop 4:** `while (true)` is true.
      * Prints `x = 4`.
      * `x` becomes 5.
      * Is 5 \> 4? Yes.
6.  **`break`** is executed. The loop terminates.
7.  The script ends.



## 📜 Listing 4.14: `upperlowercase.sh` (Modern Version)

This second example shows a more practical `while` loop. It iterates through a text file, line by line, and converts the lines to uppercase or lowercase based on whether they have an even or odd number of words.

```bash
#!/bin/bash
infile="wordfile.txt"
outfile="converted.txt"

rm -f $outfile 2>/dev/null

while read line
do
 # word count of current line
 wordcount=$(echo "$line" | wc -w)
 
 # get remainder
 ((modvalue = wordcount % 2))
 
 if [ $modvalue = 0 ]
 then
  # even: convert to uppercase
  echo "$line" | tr '[a-z]' '[A-Z]' >> $outfile
 else
  # odd: convert to lowercase
  echo "$line" | tr '[A-Z]' '[a-z]' >> $outfile
 fi
done < $infile
```

## 📜 Listing 4.15: `wordfile.txt`

This is the **input file** required for `upperlowercase.sh` to work.

```
abc def
def
abc ghi
abc
```


### 👨‍💻 Code Explanation

This script processes one file to create another.

  * `infile="wordfile.txt"`: Sets a variable for the input filename.
  * `outfile="converted.txt"`: Sets a variable for the output filename.
  * `rm -f $outfile 2>/dev/null`: This is a cleanup command.
      * `rm -f $outfile`: Forcibly (`-f`) removes the output file if it already exists. This ensures the script starts fresh every time.
      * `2>/dev/null`: This **error redirection** sends any error messages (like "file not found") to `/dev/null` (the "void"), so you don't see them.
  * `while read line`: This is the core of the script.
      * `read line` reads one line of standard input and stores its content in the variable `line`.
      * The loop continues as long as `read` can successfully read a line.
  * `done < $infile`: This is the other half of the loop.
      * `< $infile` is **input redirection**. It tells the `while` loop to get its input *from* the file `wordfile.txt`.

#### Inside the Loop

For each line read from the file:

1.  `wordcount=$(echo "$line" | wc -w)`: This calculates the number of words.

      * `$(...)` is **modern command substitution**.
      * `echo "$line"` prints the current line (e.g., "abc def").
      * `|` (pipe) sends that output as input to the `wc -w` command.
      * `wc -w`: The **w**ord **c**ount command.
      * The final number (e.g., 2) is stored in the `wordcount` variable.

2.  `((modvalue = wordcount % 2))`: This checks if the word count is even or odd using **modern arithmetic**.

      * `%` is the **modulo** operator (finds the remainder).
      * If `wordcount` is even, `modvalue` will be `0`.
      * If `wordcount` is odd, `modvalue` will be `1`.

3.  `if [ $modvalue = 0 ]`: If the `modvalue` is `0` (even)...

4.  **If Even:**

      * `echo "$line" | tr '[a-z]' '[A-Z]' >> $outfile`
      * `tr '[a-z]' '[A-Z]'`: The **tr**anslate command converts the line to uppercase.
      * `>> $outfile`: This **appends** the uppercase line to the output file.

5.  **If Odd (`else`):**

      * `echo "$line" | tr '[A-Z]' '[a-z]' >> $outfile`
      * `tr '[A-Z]' '[a-z]'`: This converts the line to lowercase.
      * `>> $outfile`: The lowercase line is appended to the output file.


### 🚀 How to Create and Run This Script

This process has two files. You must create the input file first.

**1. Create the Input File (`wordfile.txt`)**

Use `nano` to create the input file:

```bash
nano wordfile.txt
```

Paste the content from **Listing 4.15** into `nano`. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

**2. Create the Shell Script (`upperlowercase.sh`)**

Next, create the script file:

```bash
nano upperlowercase.sh
```

Paste the code from **Listing 4.14** into `nano`. Save and exit.

**3. Make the Script Executable**

Give *only the script file* executable permission. (The `.txt` file doesn't need it).

```bash
chmod +x upperlowercase.sh
```

**4. Run the Script**

Now, run the script from your terminal:

```bash
./upperlowercase.sh
```


### 🖥️ Run & Output

When you run the script, it will produce **no output** in the terminal. This is normal\! It sent all its output to the `converted.txt` file.

To see the result, use the `cat` command to print the contents of the new file:

```bash
cat converted.txt
```

You will then see the following output:

```
ABC DEF
def
ABC GHI
abc
```

This output is generated as follows:

1.  **"abc def"**: 2 words (even). Converted to `ABC DEF`.
2.  **"def"**: 1 word (odd). Converted to `def`.
3.  **"abc ghi"**: 2 words (even). Converted to `ABC GHI`.
4.  **"abc"**: 1 word (odd). Converted to `abc`.

---

# 🚦 **Combining While, Case, and If/Elif/Else Statements**

This section demonstrates how to build a robust script by combining the `while`, `case/esac`, and `if/elif/else/fi` statements. This combination is perfect for creating interactive menus or validation loops.

## 📜 Listing 4.16: `yesno.sh`

This script illustrates how to repeatedly prompt a user for input until a valid response ("Y" or "N") is received.

```bash
#!/bin/bash
while(true)
do
 # -n flag removes the trailing newline
 echo -n "Proceed with backup (Y/y/N/n): "
 
 # Read user input and store it in the 'response' variable
 read response
 
 # Evaluate the user's response
 case $response in
  n*|N*) proceed="false" ;;
  y*|Y*) proceed="true" ;;
  *) proceed="unknown" ;;
 esac
 
 # Act based on the 'proceed' variable
 if [[ "$proceed" = "true" ]]
 then
  echo "proceeding with backup"
  break # Exit the while loop
 elif [[ "$proceed" = "false" ]]
 then
  echo "canceling backup"
  break # Exit the while loop
 else
  echo "Invalid response"
  # Loop repeats
 fi
done
```


### 👨‍💻 Code Explanation

This script is a powerful validation loop. Let's break it down.

  * `#!/bin/bash`: The shebang, telling the system to use the Bash shell.
  * `while(true)`: This starts an **infinite loop**. The code inside will repeat forever *until* a `break` command is hit.
  * `echo -n "Proceed with..."`: This prompts the user for input.
      * The `-n` flag is important: it tells `echo` **not** to print a newline character, so the user's cursor stays on the same line as the prompt.
  * `read response`: This command pauses the script and waits for the user to type something and press `Enter`. Whatever they type is stored in the `response` variable.
  * `case $response in ... esac`: This is a `case` statement, which is a cleaner way to handle multiple `if` conditions on a single variable. It checks the value of `$response`:
      * `n*|N*)`: This pattern checks if `$response` starts with (`*`) "n" **or** (`|`) "N". If it matches (e.g., "n", "no", "N", "Never"), it executes the command.
      * `proceed="false" ;;`: It sets the `proceed` variable to the string "false". The `;;` marks the end of this case.
      * `y*|Y*) proceed="true" ;;`: Similarly, this checks for "y" or "Y" and sets the `proceed` variable to "true".
      * `*)`: This is the "wildcard" or "default" case. It matches *anything else* (like "abc", "q", or just pressing Enter).
      * `proceed="unknown" ;;`: It sets the `proceed` variable to "unknown".
  * `if [[ "$proceed" = "true" ]] ... fi`: This `if` block checks the value of the `proceed` variable that we just set.
      * `if [[ "$proceed" = "true" ]]`: If the user entered 'y', this is true.
          * `echo "proceeding with backup"`: Prints a success message.
          * `break`: **This is key.** It terminates the `while(true)` loop, and the script ends.
      * `elif [[ "$proceed" = "false" ]]`: If the user entered 'n', this is true.
          * `echo "canceling backup"`: Prints a cancellation message.
          * `break`: This also terminates the `while` loop, and the script ends.
      * `else`: If `proceed` is not "true" or "false", it must be "unknown".
          * `echo "Invalid response"`: It prints an error.
          * The `if` block ends. The `while` loop has *not* been broken, so it repeats from the top, asking the user the question again.


### 🚀 How to Create and Run This Script

**1. Create the Script File**

Use `nano` (or your favorite text editor) to create the file:

```bash
nano yesno.sh
```

**2. Paste the Code**

Copy the code from **Listing 4.16** and paste it into the `nano` editor.

Save and exit:

  * Press **`Ctrl + O`** and then **`Enter`** to save.
  * Press **`Ctrl + X`** to exit.

**3. Make the Script Executable**

You must give the script permission to be executed:

```bash
chmod +x yesno.sh
```

**4. Run the Script**

Execute the script from your terminal:

```bash
./yesno.sh
```


### 🖥️ Example Output

When you run the script, it will repeatedly ask you for input until you give a valid 'y' or 'n' answer.

Here is an example session:

```bash
$ ./yesno.sh
Proceed with backup (Y/y/N/n): abc
Invalid response
Proceed with backup (Y/y/N/n): 
Invalid response
Proceed with backup (Y/y/N/n): Y
proceeding with backup
$
```

And here is an example of canceling:

```bash
$ ./yesno.sh
Proceed with backup (Y/y/N/n): no
canceling backup
$
```

---

# ⏳ **Using an Until Loop**

The `until` command allows you to execute a series of commands as long as a specific condition tests **false**. Once the condition becomes **true**, the loop stops.

It's the perfect loop for when you're "waiting" for something to happen.

### Basic Syntax

```bash
until command
do
 commands
done
```


## 📜 Listing 4.17: `until1.sh` (Modern Version)

This script illustrates how to use an `until` loop to iterate through a set of numbers. We've updated this version to use modern Bash arithmetic, which is much cleaner than the old `expr` command.

```bash
#!/bin/bash
x=0

until [ "$x" = "5" ]
do
 ((x++)) # Modern, clean way to increment x by 1
 echo "x: $x"
done
```


### 👨‍💻 Code Explanation

Let's break down exactly what this script is doing.

  * `#!/bin/bash`: The shebang. It tells the system this is a Bash script.
  * `x=0`: We initialize a variable named `x` with the value `0`.
  * `until [ "$x" = "5" ]`: This is the loop's control.
      * `until`: This command starts the loop.
      * `[ "$x" = "5" ]`: This is the **condition**. The loop will execute *as long as this condition is FALSE*.
  * `do`: Marks the beginning of the code block to run.
  * `((x++))`: This is **modern Bash arithmetic**. It's a clean and efficient way to take the current value of `x` and add 1 to it.
  * `echo "x: $x"`: This prints the *new*, incremented value of `x` to the console.
  * `done`: Marks the end of the loop. The script will now jump back up to the `until` line and re-check the condition.

#### Script Analysis

Listing 4.17 initializes the variable `x` with the value 0 and then enters the `until` loop. The body of the loop first increments `x` and then prints its new value. This cycle repeats *until* `x` finally equals 5. At that point, the condition `[ "$x" = "5" ]` becomes **true**, and the loop terminates execution.


### 🚀 How to Create and Run This Script

**1. Create the Script File**

Use `nano` to create and open a new file named `until1.sh`:

```bash
nano until1.sh
```

**2. Paste the Code**

Copy the **modern version** of the code from **Listing 4.17** above and paste it directly into the `nano` editor.

To save and exit `nano`:

  * Press **`Ctrl + O`** (Write Out) and then press **`Enter`** to save.
  * Press **`Ctrl + X`** to exit.

**3. Make the Script Executable**

You need to give the file permission to run:

```bash
chmod +x until1.sh
```

**4. Run the Script**

Execute the script from your terminal:

```bash
./until1.sh
```


### 🖥️ Expected Output

When you run the script, you will see the following output printed to your console.

```bash
x: 1
x: 2
x: 3
x: 4
x: 5
```

Here is a step-by-step trace of the loop's execution:

1.  **Start:** `x` is `0`.
2.  **Check 1:** Is `x` (0) equal to 5? **No** (Condition is **false**). So, run the loop.
      * `x` becomes 1.
      * Print: `x: 1`
3.  **Check 2:** Is `x` (1) equal to 5? **No** (Condition is **false**). So, run the loop.
      * `x` becomes 2.
      * Print: `x: 2`
4.  **Check 3:** Is `x` (2) equal to 5? **No** (Condition is **false**). So, run the loop.
      * `x` becomes 3.
      * Print: `x: 3`
5.  **Check 4:** Is `x` (3) equal to 5? **No** (Condition is **false**). So, run the loop.
      * `x` becomes 4.
      * Print: `x: 4`
6.  **Check 5:** Is `x` (4) equal to 5? **No** (Condition is **false**). So, run the loop.
      * `x` becomes 5.
      * Print: `x: 5`
7.  **Check 6:** Is `x` (5) equal to 5? **Yes** (Condition is **true**). Stop the loop.
8.  **End.**
  
---

# 🛠️ **User-Defined Functions in Bash**

The Bash shell provides many built-in commands, but its real power comes from letting you define your own functions. This means you can create custom, reusable blocks of code specific to your needs.

You can define these functions directly in your terminal for a quick test, or (more commonly) place them inside a shell script to be used over and over.

## 📝 Basic Syntax

There are two simple rules to define a function in a shell script.

  * **Rule 1:** Function blocks can begin with the function name followed by parentheses `()`.
  * **Rule 2:** The code block *must* be enclosed in curly braces `{ ... }`.

> **Note:** You may also see the keyword `function` used, like `function Hello()`. The version without `function` (e.g., `Hello()`) is more common and what we'll use here.

### 1\. A Simple "Hello" Function

This is the most basic example. We define a function and then call it by its name.

#### 📜 Script: `simple_hello.sh`

```bash
#!/bin/bash

# 1. Define the function
# This block of code is now "owned" by the name "Hello"
Hello()
{
 echo "Hello from a function"
}

# 2. Call the function
# The script jumps up to the function, runs its code,
# and then returns here to continue.
Hello
```

#### 🚀 How to Run

1.  **Create the file:**

    ```bash
    nano simple_hello.sh
    ```

2.  **Paste the code** from the block above. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

3.  **Make it executable:**

    ```bash
    chmod +x simple_hello.sh
    ```

4.  **Execute the script:**

    ```bash
    ./simple_hello.sh
    ```

#### 🖥️ Output

The output is exactly what you expect:

```bash
Hello from a function
```

### 2\. Making Functions Useful with Parameters

A function that does the same thing every time isn't very flexible. We can make it more useful by passing it **arguments**, also known as **parameters**.

Inside a function, you can access these parameters using special variables:

  * `$1` = The first argument passed to the function.
  * `$2` = The second argument.
  * `...and so on.`

#### 📜 Script: `hello_param.sh`

```bash
#!/bin/bash

Hello()
{
 # $1 is a placeholder for the first argument
 # passed when the function is called.
 echo "Hello $1 how are you"
}

# Call the function and pass "Bob" as the first argument.
# "Bob" will become $1 inside the function.
Hello "Bob"
```

#### 🚀 How to Run

1.  **Create the file:**

    ```bash
    nano hello_param.sh
    ```

2.  **Paste the code** above. Save and exit.

3.  **Make it executable:**

    ```bash
    chmod +x hello_param.sh
    ```

4.  **Execute the script:**

    ```bash
    ./hello_param.sh
    ```

#### 🖥️ Output

The `$1` variable was successfully replaced by "Bob":

```bash
Hello Bob how are you
```

### 3\. Adding Logic with `if`

You can use conditional logic (like `if` statements) inside your functions to make them "smarter." This example checks if the user actually provided a parameter.

#### 📜 Script: `hello_check.sh`

```bash
#!/bin/bash

Hello()
{
 # Check if the string $1 is "not equal" (!=) to an empty string ("")
 if [ "$1" != "" ]
 then
  # If it's NOT empty, print the name
  echo "Hello $1"
 else
  # If it IS empty, print an error
  echo "Please specify a name"
 fi
}

# Let's test both conditions:

echo "--- Calling with a name: ---"
Hello "Alice"

echo "" # Adds a blank line for readability

echo "--- Calling without a name: ---"
Hello
```

#### 🚀 How to Run

1.  **Create the file:**

    ```bash
    nano hello_check.sh
    ```

2.  **Paste the code** above. Save and exit.

3.  **Make it executable:**

    ```bash
    chmod +x hello_check.sh
    ```

4.  **Execute the script:**

    ```bash
    ./hello_check.sh
    ```

#### 🖥️ Output

You can see the function's `if/else` logic working perfectly in both calls:

```bash
--- Calling with a name: ---
Hello Alice

--- Calling without a name: ---
Please specify a name
```

### 4\. Processing All Parameters with `while` and `shift`

What if you want to pass *many* arguments, not just one? You can use a `while` loop to process them all, one by one, using a special command called `shift`.

#### 📜 Script: `hello_all.sh`

```bash
#!/bin/bash

Hello()
{
 # This loop continues as long as $1 is not empty.
 while [ "$1" != "" ]
 do
  echo "hello $1"
  
  # This is the magic! 'shift' does two things:
  # 1. It deletes the current $1.
  # 2. It "shifts" all other parameters down.
  #    $2 becomes the new $1.
  #    $3 becomes the new $2.
  #    ...and so on.
  #
  # The loop then repeats, checking the NEW $1.
  shift
 done
}

# Call the function with three arguments: "a", "b", and "c"
Hello a b c
```

#### 🚀 How to Run

1.  **Create the file:**

    ```bash
    nano hello_all.sh
    ```

2.  **Paste the code** above. Save and exit.

3.  **Make it executable:**

    ```bash
    chmod +x hello_all.sh
    ```

4.  **Execute the script:**

    ```bash
    ./hello_all.sh
    ```

#### 🖥️ Output

The `while` loop iterated three times, "shifting" the parameters each time until none were left.

```bash
hello a
hello b
hello c
```
---

# 🗂️ **Creating a Simple Menu from Shell Commands**

This guide demonstrates how to build an interactive menu by combining a `while` loop, a `case` statement, and a user-defined function.

## 📜 Listing 4.18: `AppendRow.sh`

This script, `AppendRow.sh`, provides a menu-driven interface to update a set of users stored in a text file.

```bash
#!/bin/bash
DataFile="users.txt"

addUser()
{
 echo -n "First Name: "
 read fname
 echo -n "Last Name: "
 read lname
 
 if [ -n $fname -a -n $lname ]
 then
  # append new line to the file
  echo "$fname $lname" >> $DataFile
 else
  echo "Please enter non-empty values"
 fi
}

while (true)
do
 echo ""
 echo "List of Users"
 echo "============="
 cat $DataFile 2>/dev/null
 echo ----"
 echo "Enter 'a' to add a new user"
 echo "Enter 'd' to delete all users"
 echo "Enter 'x' to exit this menu"
 echo ----"
 echo
 read answer
 
 case $answer in
  a|A) addUser ;;
  d|D) rm -f $DataFile 2>/dev/null ;;
  x|X) break ;;
 esac
done
```


### 👨‍💻 Code Explanation

Listing 4.18 starts by initializing a variable and defining a function. It then enters an infinite loop to display the menu.

#### Initialization

  * `DataFile="users.txt"`: This line initializes a variable named `DataFile` and assigns it the value `users.txt`. This variable is used throughout the script to reference the user data file, making it easy to change the filename in one place.

#### The `addUser()` Function

This code block defines a custom function that will be executed when the user wants to add a new name.

  * `echo -n "First Name: "`: This prompts the user for a first name. The `-n` flag tells `echo` **not** to add a newline, so the user's cursor stays on the same line as the prompt.
  * `read fname`: This command pauses the script and waits for the user to type a value. Whatever they type is stored in the `fname` variable.
  * `read lname`: This does the same for the `lname` (last name) variable.
  * `if [ -n $fname -a -n $lname ]`: This is the core logic.
      * `[ -n $fname ]`: This test checks if the `$fname` variable is **n**on-empty.
      * `-a`: This is a logical **AND** operator.
      * `[ -n $lname ]`: This test checks if the `$lname` variable is also **n**on-empty.
      * The entire `if` statement reads: "If the first name is not empty AND the last name is not empty..."
  * `echo "$fname $lname" >> $DataFile`: If the test is true, this command executes.
      * `echo "$fname $lname"`: Prints the first and last name, separated by a space.
      * `>> $DataFile`: This is the **append operator**. It redirects the output and adds it as a new line to the *end* of the file specified in `$DataFile` (i.e., `users.txt`), without deleting the file's previous contents.
  * `else`: If either `fname` or `lname` was empty, this block runs.
  * `echo "Please enter non-empty values"`: An appropriate message is displayed to the user.

#### The Main `while (true)` Loop

This is an infinite loop that keeps the menu running until the user explicitly chooses to exit.

  * `echo "List of Users"`: This section displays the menu.
  * `cat $DataFile 2>/dev/null`: This is a clever command.
      * `cat $DataFile`: This attempts to print the contents of `users.txt` to the screen.
      * `2>/dev/null`: This is **error redirection**. If the `users.txt` file does not exist (e.g., on the first run, or after deleting it), `cat` would normally print an ugly error. This command redirects "Standard Error" (file descriptor `2`) to `/dev/null` (a "black hole" that discards all input), effectively hiding the error and failing silently.
  * `echo "..."`: These lines print the menu options (a, d, x).
  * `read answer`: This pauses the script and waits for the user to enter their choice, which is stored in the `answer` variable.

#### The `case` Statement (Menu Logic)

This block checks the value of `$answer` and performs the correct action.

  * `case $answer in`: The start of the case block.
  * `a|A) addUser ;;`: This pattern matches if the user entered `a` **or** (`|`) `A`.
      * `addUser`: It calls the function we defined earlier.
      * `;;`: Marks the end of this option.
  * `d|D) rm -f $DataFile 2>/dev/null ;;`: This matches `d` or `D`.
      * `rm -f $DataFile`: This command forcibly (`-f`) **r**e**m**oves (deletes) the `users.txt` file. The `2>/dev/null` is used again to hide any "file not found" errors if the user tries to delete an already-deleted file.
  * `x|X) break ;;`: This matches `x` or `X`.
      * `break`: This command immediately terminates the `while (true)` loop. Since there is no code after the loop, the shell script ends.
  * `esac`: Ends the `case` statement.


### 🚀 How to Create and Run This Script

**1. Create the Script File**

Use `nano` to create the file:

```bash
nano AppendRow.sh
```

**2. Paste the Code**

Copy the entire script from **Listing 4.18** and paste it into the `nano` editor.

Save and exit:

  * Press **`Ctrl + O`** and then **`Enter`** to save.
  * Press **`Ctrl + X`** to exit.

**3. Make the Script Executable**

You must give the script permission to run:

```bash
chmod +x AppendRow.sh
```

**4. Run the Script**

Execute the script from your terminal:

```bash
./AppendRow.sh
```


### 🖥️ Example Session

Here is a sample invocation showing how the script works from the very beginning.

**(First run, file is empty)**

```bash
$ ./AppendRow.sh

List of Users
=============----
Enter 'a' to add a new user
Enter 'd' to delete all users
Enter 'x' to exit this menu----

a
First Name: abc
Last Name: def

List of Users
=============
abc def----
Enter 'a' to add a new user
Enter 'd' to delete all users
Enter 'x' to exit this menu----

a
First Name: 123
Last Name: 456

List of Users
=============
abc def
123 456----
Enter 'a' to add a new user
Enter 'd' to delete all users
Enter 'x' to exit this menu----

a
First Name: 888
Last Name: 777

List of Users
=============
abc def
123 456
888 777----
Enter 'a' to add a new user
Enter 'd' to delete all users
Enter 'x' to exit this menu----
```

**(This matches the "sample invocation" state from the text. Let's test the other options.)**

```bash
a
First Name: test
Last Name: 
Please enter non-empty values

List of Users
=============
abc def
123 456
888 777----
Enter 'a' to add a new user
Enter 'd' to delete all users
Enter 'x' to exit this menu----

d

List of Users
=============----
Enter 'a' to add a new user
Enter 'd' to delete all users
Enter 'x' to exit this menu----

x
$ 
```

---

# 📦 **Arrays in Bash**

Arrays are available in most (if not all) programming languages, and they are also available in Bash.

It's worth noting some terminology:

  * A one-dimensional array is known as a **vector** in mathematics.
  * A two-dimensional array is called a **matrix**.

However, in the world of shell scripts, you will almost always just see the word "array."

## 📜 Listing 4.19: `Array1.sh`

This script illustrates the most basic way to define an array and access its individual elements.

```bash
#!/bin/bash
# initialize the names array
names[0]="john"
names[1]="nancy"
names[2]="jane"
names[3]="steve"
names[4]="bob"

# display the first and second entries
echo "First Index: ${names[0]}"
echo "Second Index: ${names[1]}"
```


### 👨‍💻 Code Explanation

This script defines an array called `names` and initializes it with five strings.

  * `names[0]="john"`: This line assigns the string "john" to the *first position* (index `0`) of the `names` array. Bash arrays are **zero-indexed**, meaning they start counting from 0, not 1.
  * `${names[0]}`: This is how you read a value from an array. The `${...}` curly braces are important for clarity and to prevent errors, especially when next to other text. This command reads the value at index `0`, which is "john".
  * `${names[1]}`: This reads the value at index `1`, which is "nancy".


### 🚀 How to Create and Run This Script

**1. Create the File**

```bash
nano Array1.sh
```

**2. Paste the Code**
Copy the code from **Listing 4.19** above and paste it into the `nano` editor. Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

**3. Make it Executable**

```bash
chmod +x Array1.sh
```

**4. Run the Script**

```bash
./Array1.sh
```


### 🖥️ Expected Output

The output from Listing 4.19 is here:

```bash
First Index: john
Second Index: nancy
```


## 🌐 Accessing All Array Items

If you need to access *all* the items in an array at once (for example, to loop through them), you can use either of the following code snippets.

  * `${array_name[*]}`: Expands to all items as a single string.
  * `${array_name[@]}`: Expands to all items as separate, individual items.

> **Pro-tip:** Always use `"${array_name[@]}"` (with the quotes\!) when looping. This correctly handles items that have spaces in them.


## 📜 Listing 4.20: `loadarray.sh`

This script initializes an array from a string of numbers and then prints its contents, demonstrating a common trap when adding numbers in Bash.

*(Note: We have updated the code to use backticks `` `...` ``, as described in the text, to make it runnable.)*

```bash
#!/bin/bash
numbers="1 2 3 4 5 6 7 8 9 10"

# Note: The backticks ` ` execute the command.
# The ( ... ) capture the space-separated output into an array.
array1=( `echo "$numbers"` )

total1=0
total2=0

for num in "${array1[@]}"
do
 #echo "array item: $num"
 
 # This performs STRING concatenation
 total1+=$num
 
 # 'let' forces ARITHMETIC evaluation
 let total2+=$num
done

echo "Total1: $total1"
echo "Total2: $total2"
```


### 👨‍💻 Code Explanation

  * `numbers="..._"`: This defines a single string variable.
  * ` array1=(  `echo "$numbers"`  ) `: This is the tricky part.
    1.  `` `echo "$numbers"` ``: The backticks execute the `echo` command, which prints the string `1 2 3 4 5 6 7 8 9 10`.
    2.  `array1=( ... )`: The outer parentheses capture this output. Because the output is separated by spaces, Bash splits it on those spaces and creates an array with 10 elements: `("1", "2", "3", ... "10")`.
  * `total1=0` / `total2=0`: Initializes two "counter" variables.
  * `for num in "${array1[@]}"`: This loops through every item in `array1`.
  * `total1+=$num`: **This is the trap\!** By default, Bash treats `+` as *string concatenation* (joining strings) unless told otherwise. It joins "0" + "1" + "2" + "3"...
  * `let total2+=$num`: The `let` keyword explicitly tells Bash to perform an *arithmetic* operation. This correctly *adds* the numbers: 0 + 1 + 2 + 3...

As you can see from the output, `total1` is the result of appending all the elements into a single string, whereas `total2` is the correct numeric sum.


### 🚀 How to Create and Run This Script

**1. Create the File**

```bash
nano loadarray.sh
```

**2. Paste the Code**
Copy the code from **Listing 4.20** into the editor. Save and exit.

**3. Make it Executable**

```bash
chmod +x loadarray.sh
```

**4. Run the Script**

```bash
./loadarray.sh
```


### 🖥️ Expected Output

Launch the shell script in Listing 4.20. The output is as follows:

```bash
Total1: 012345678910
Total2: 55
```


## 📜 Listing 4.21: `update-array.sh`

This script shows you some common operations you can perform on an array *after* it has been initialized, like reading specific elements, deleting elements, and adding new ones.

```bash
#!/bin/bash
array=("I" "love" "deep" "dish" "pizza")

#the first array element:
echo "First Element: ${array[0]}"

#all array elements:
echo "All Elements: ${array[@]}"

#all array indexes:
echo "All Indexes: ${!array[@]}"

#Remove array element at index 3:
unset array[3]

#add new array element with index 1234:
array[1234]="in Chicago"

#all array elements:
echo "After Changes: ${array[@]}"

#all array indexes after changes:
echo "Indexes After Changes: ${!array[@]}"
```

*(Note: I added two extra `echo` statements to the original script to make the changes clearer in the output.)*


### 👨‍💻 Code Explanation

  * `array=(...)`: This is a more direct way to initialize a whole array at once.
  * `${array[0]}`: Accesses the first element (at index 0), which is "I".
  * `${array[@]}`: Accesses all elements in the array.
  * `${!array[@]}`: This is a special one\! The `!` accesses the **indexes** (or "keys") of the array, not the values.
  * `unset array[3]`: This command **deletes** the element at index 3 (which was "dish"). The array now has a "hole" in it.
  * `array[1234]="in Chicago"`: This adds a new element. Bash arrays can be **sparse**, meaning you can have indexes 0, 1, 2, and 1234 with nothing in between.
  * `echo ${array[@]}` (After Changes): Notice "dish" is gone.
  * `echo ${!array[@]}` (After Changes): Notice the indexes are now `0 1 2 4 1234`. The index `3` is gone.


### 🚀 How to Create and Run This Script

**1. Create the File**

```bash
nano update-array.sh
```

**2. Paste the Code**
Copy the code from **Listing 4.21** into the editor. Save and exit.

**3. Make it Executable**

```bash
chmod +x update-array.sh
```

**4. Run the Script**

```bash
./update-array.sh
```


### 🖥️ Expected Output

Launch the code in Listing 4.21, and you will see the following output (our modified version is slightly more descriptive):

```bash
First Element: I
All Elements: I love deep dish pizza
All Indexes: 0 1 2 3 4
After Changes: I love deep pizza in Chicago
Indexes After Changes: 0 1 2 4 1234
```


## 🧠 Working with Arrays: Key Concepts

Arrays enable you to "group together" related data elements as rows, and then each row contains logically related data values. As a simple example, the following array could define three fields for a customer (obviously not a complete set of fields):

```bash
cust[0] = name
cust[1] = Address
cust[2] = phone number
```

Customer records can be saved in a text file that can be read later by a shell script. If you are unfamiliar with text files, common types include:

  * **CSV (Comma-Separated Values):** `value1,value2,value3`
  * **TSV (Tab-Separated Values):** `value1  value2  value3`
  * **Other Delimiters:** Files can use any delimiter, like a colon (`:`) or a pipe (`|`).

The character that separates these values is called the **IFS (Internal Field Separator)**. This is a special Bash variable that controls how the shell splits strings.

This section contains several scripts that illustrate more useful features of arrays in Bash. The syntax is different enough from other programming languages that it is worthwhile to see several examples.


## 📜 Listing 4.22: `fruits-array1.sh`

This script illustrates two different ways to create an array and some advanced operations you can perform, like slicing and substrings.

```bash
#!/bin/bash
# method #1: One by one
fruits[0]="apple"
fruits[1]="banana"
fruits[2]="cherry"
fruits[3]="orange"
fruits[4]="pear"
echo "first fruit: ${fruits[0]}"

# method #2: Declare all at once
declare -a fruits2=(apple banana cherry orange pear)
echo "first fruit: ${fruits2[0]}"

# range of elements:
echo "last two: ${fruits[@]:3:2}"

# substring of element:
echo "substring: ${fruits[1]:0:3}"

arrlength=${#fruits[@]}
echo "length: ${#fruits[@]}"
```


### 👨‍💻 Code Explanation

  * **Method 1:** This is the same method from `Array1.sh`, assigning elements one by one.
  * **Method 2:** `declare -a fruits2=(...)` is a more modern, explicit way to **d**eclare an **a**rray. The `(...)` syntax initializes it with a space-separated list of values.
  * **Range (Slicing):** `${fruits[@]:3:2}`
      * `[@]` means "from all elements..."
      * `:3` means "...starting at index 3" ("orange").
      * `:2` means "...take 2 elements."
      * Result: "orange" and "pear".
  * **Substring:** `${fruits[1]:0:3}`
      * `[1]` means "on the element at index 1" ("banana").
      * `:0` means "...starting at character 0" (the 'b').
      * `:3` means "...take 3 characters."
      * Result: "ban".
  * **Length:** `${#fruits[@]}`
      * The `#` in front of the array name returns its **length**, or the total number of elements it contains.


### 🚀 How to Create and Run This Script

**1. Create the File**

```bash
nano fruits-array1.sh
```

**2. Paste the Code**
Copy the code from **Listing 4.22** into the editor. Save and exit.

**3. Make it Executable**

```bash
chmod +x fruits-array1.sh
```

**4. Run the Script**

```bash
./fruits-array1.sh
```


### 🖥️ Expected Output

Launch the code in Listing 4.22, and you will see the following output:

```bash
first fruit: apple
first fruit: apple
last two: orange pear
substring: ban
length: 5
```


## 📜 Listing 4.23 & 4.24: `array-from-file.sh`

This example demonstrates how to load an array from a file and how the **$IFS** variable drastically changes the result.

First, you must create the input file.

#### LISTING 4.23: `names.txt`

```
Jane Smith
John Jones
Dave Edwards
```


#### LISTING 4.24: `array-from-file.sh`

```bash
#!/bin/bash
names="names.txt"

echo "--- First loop (Default IFS) ---"
contents1=( `cat "$names"` )
for w in "${contents1[@]}"
do
 echo "$w"
done

# Set Internal Field Separator to empty
# The *intent* is to split only on newlines
# (The modern, correct way is IFS=$'\n')
IFS="" 

names="names.txt"
echo
echo "--- Second loop (Modified IFS) ---"
contents1=( `cat "$names"` )
for w in "${contents1[@]}"
do
 echo "$w"
done
```

*(Note: I've added `echo` headers to make the output clearer.)*


### 👨‍💻 Code Explanation

  * ` contents1=(  `cat "$names"`  ) `: This command does two things:

    1.  `` `cat "$names"` `` reads the *entire content* of `names.txt`.
    2.  The `(...)` tries to turn that content into an array. It splits the content based on the **$IFS** variable.

  * **First Loop:** By default, `$IFS` contains spaces, tabs, and newlines. Therefore, "Jane Smith" is split into "Jane" and "Smith". The array `contents1` becomes `("Jane", "Smith", "John", "Jones", "Dave", "Edwards")`. The loop prints each word on its own line.

  * `IFS=""`: This line changes the Internal Field Separator. The *intent* described in the text is to make the newline character the *only* separator. (While `IFS=$'\n'` is the more robust way to do this, we are using the code as written).

  * **Second Loop:** The ` contents1=(  `cat "$names"` )` command is run again. Because `$IFS`has been changed to no longer include spaces, the shell *only* splits on the newlines. This time, "Jane Smith" is treated as a *single* element. The array`contents1`becomes`("Jane Smith", "John Jones", "Dave Edwards")\`. The loop now prints each *line*.


### 🚀 How to Create and Run This Script

This requires **two** files.

**1. Create the Data File (`names.txt`)**

```bash
nano names.txt
```

Paste the content from **Listing 4.23**. Save and exit.

**2. Create the Script File (`array-from-file.sh`)**

```bash
nano array-from-file.sh
```

Paste the code from **Listing 4.24**. Save and exit.

**3. Make the Script Executable**

```bash
chmod +x array-from-file.sh
```

**4. Run the Script**

```bash
./array-from-file.sh
```


### 🖥️ Expected Output

Launch the code in Listing 4.24, and you will see the following output:

```bash
--- First loop (Default IFS) ---
Jane
Smith
John
Jones
Dave
Edwards

--- Second loop (Modified IFS) ---
Jane Smith
John Jones
Dave Edwards
```


## 📜 Listing 4.25: `array-function.sh`

This script illustrates how to pass an entire array to a user-defined function.

```bash
#!/bin/bash

# This is a user-defined function
items() {
 # "$@" represents all arguments passed to the function
 for line in "${@}"
 do
  printf "%s\n" "${line}"
 done
}

# Define an array
arr=( 123 -abc 'my data' )

# Call the function, passing all array elements as arguments
items "${arr[@]}"
```

*(Note: The original text also included a "compact version" comment, which I've removed from the main code block for clarity, but the function is the same.)*


### 👨‍💻 Code Explanation

  * `items() { ... }`: This defines a function named `items`.
  * `for line in "${@}"`: This is the key. `"${@}"` is a special variable that expands to *all arguments passed to the function*, with each argument correctly quoted.
  * `arr=( ... )`: An array `arr` is defined with three elements: "123", "-abc", and "my data".
  * `items "${arr[@]}"`: This is how you pass an array.
      * `"${arr[@]}"` expands the `arr` array into separate, quoted arguments: `"123"`, `"-abc"`, `"my data"`.
      * The `items` function receives these three strings as its arguments (`$1`, `$2`, `$3`).
      * The function's internal loop then iterates through `"${@}"` (all its arguments) and prints them one by one.


### 🚀 How to Create and Run This Script

**1. Create the File**

```bash
nano array-function.sh
```

**2. Paste the Code**
Copy the code from **Listing 4.25** into the editor. Save and exit.

**3. Make it Executable**

```bash
chmod +x array-function.sh
```

**4. Run the Script**

```bash
./array-function.sh
```


### 🖥️ Expected Output

The output is shown here:

```bash
123
-abc
my data
```


## 📜 Listing 4.26: `array-loops1.sh`

This script illustrates how to get the length of an array and then use a classic, C-style `for` loop to iterate through it by its index number.

```bash
#!/bin/bash
fruits[0]="apple"
fruits[1]="banana"
fruits[2]="cherry"
fruits[3]="orange"
fruits[4]="pear"

# array length:
arrlength=${#fruits[@]}
echo "length: ${#fruits[@]}"

# print each element via a loop:
for (( i=1; i<${arrlength}+1; i++ ));
do
 # Note: We use $i-1 because the loop starts at 1
 # while the array index starts at 0
 echo "element $i of ${arrlength} : ${fruits[$i-1]}"
done
```


### 👨‍💻 Code Explanation

  * `arrlength=${#fruits[@]}`: As seen before, this gets the total number of elements (5) and stores it in a variable called `arrlength`.
  * `for (( i=1; i<${arrlength}+1; i++ ))`: This is a C-style `for` loop.
      * `i=1`: It initializes a counter `i` at 1.
      * `i<${arrlength}+1`: It continues the loop as long as `i` is less than the length+1 (i.e., less than 6). It will run for `i = 1, 2, 3, 4, 5`.
      * `i++`: It increments `i` by one after each loop.
  * `echo ... ${fruits[$i-1]}`: This line prints the element.
      * Because our loop counter `i` runs from 1 to 5, but our array indexes run from 0 to 4, we must use `$i-1` to access the correct element.
      * Loop 1: `i` is 1. Access `fruits[1-1]` -\> `fruits[0]` ("apple").
      * Loop 2: `i` is 2. Access `fruits[2-1]` -\> `fruits[1]` ("banana").
      * ...and so on.


### 🚀 How to Create and Run This Script

**1. Create the File**

```bash
nano array-loops1.sh
```

**2. Paste the Code**
Copy the code from **Listing 4.26** into the editor. Save and exit.

**3. Make it Executable**

```bash
chmod +x array-loops1.sh
```

**4. Run the Script**

```bash
./array-loops1.sh
```


### 🖥️ Expected Output

Launch the code in Listing 4.26, and you will see the following output:

```bash
length: 5
element 1 of 5 : apple
element 2 of 5 : banana
element 3 of 5 : cherry
element 4 of 5 : orange
element 5 of 5 : pear
```


That covers all the array listings\! You've learned how to create, read, update, delete, slice, and loop through arrays in just about every way Bash allows.

---