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