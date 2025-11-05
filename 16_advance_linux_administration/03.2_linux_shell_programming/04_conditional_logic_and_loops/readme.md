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