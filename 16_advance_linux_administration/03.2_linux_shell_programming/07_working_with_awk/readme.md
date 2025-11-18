# 🤖 Introducing `awk`: The Data Manipulation Powerhouse

This section introduces you to the `awk` command, a versatile utility for manipulating data and restructuring datasets. In fact, this utility is so versatile that entire books have been written about it.

`awk` is essentially an entire programming language in a single command. It accepts standard input, provides standard output, and uses regular expressions and metacharacters in the same way other Unix commands do. This capability allows you to "plug" it into other command-line expressions and accomplish almost anything. However, this power comes at the cost of adding significant complexity to a command string that may already be doing quite a lot.

> **Note:** It is useful to add a comment when using `awk` in a script. Because it is so versatile, it will not be clear at a glance which of its many features you are using.


## 📚 What You Will Learn in This section

This section is broken down into several parts to guide you from basic concepts to complex data manipulation.

### 1. The Basics of `awk`
The first part of this section provides a brief introduction to the `awk` command. You will learn about:
* Some of the most important built-in variables `awk` provides.
* How to manipulate string variables using `awk` (though some of these examples can also be handled using other bash commands).

### 2. Logic, Loops, and Dataset Manipulation
The second part of this section shows you how to use conditional logic (`if`), `while` loops, and `for` loops within `awk` to manipulate the rows and columns in datasets. This section covers:
* Deleting lines from a dataset.
* Merging lines together.
* Printing the entire contents of a file as a single line of text.
* How to "join" lines and groups of lines in datasets.

### 3. `awk` with Regular Expressions
The third section contains code samples that involve metacharacters (introduced in section 1) and character sets within `awk` commands. You will also see how to use conditional logic to determine whether to print a line of text.

### 4. Numeric Calculations and String Splitting
The fourth section illustrates:
* How to split a text string that contains multiple `.` characters as a delimiter.
* Examples of `awk` performing numeric calculations (such as addition, subtraction, multiplication, and division) in files containing numeric data.
* Various numeric functions that are available in `awk`.
* How to print text in a fixed set of columns.

### 5. Advanced Columnar Data and One-Liners
The fifth section explains how to:
* Align columns in a dataset.
* Align and merge columns in a dataset.
* Delete specific columns.
* Select a subset of columns from a dataset.
* Work with multi-line records.
* This section also contains some one-line `awk` commands that can be useful for manipulating the contents of datasets.

### 6. Practical Use Cases
The final section of this section provides a pair of use cases involving nested quotes and date formats in structured datasets.

---

# 🤖 The `awk` Command

The `awk` command (named after its three authors: **A**ho, **W**einberger, and **K**ernighan) is a powerful utility and programming language for manipulating data and restructuring datasets. It has a C-like syntax and can be used to perform very complex operations on numbers and text strings.

As a side comment, `gawk` is the GNU version of `awk` (this is the version found on most Linux systems). `nawk` is the "new" `awk` (neither command is discussed in this section).


## 📊 Built-in Variables that Control `awk`

The `awk` command provides variables that you can change from their default values to control how it performs operations.

Think of **records** as **lines** (rows) and **fields** as **columns** (words).

  * **Field Separators (FS/OFS):** Define what separates columns (e.g., a space, a comma).
  * **Record Separators (RS/ORS):** Define what separates rows (e.g., a newline).

Here are the most common built-in variables:

| Variable | Name | Default Value | Description |
| :--- | :--- | :--- | :--- |
| **`FS`** | Field Separator | `" "` (a space) | The character(s) `awk` uses to split an input record into fields. |
| **`RS`** | Record Separator | `"\n"` (a newline) | The character(s) `awk` uses to separate input records. |
| **`OFS`** | Output Field Separator | `" "` (a space) | The character(s) `awk` uses to join fields when you print. |
| **`ORS`** | Output Record Separator | `"\n"` (a newline) | The character(s) `awk` prints at the end of every `print` command. |
| **`NF`** | Number of Fields | N/A | A variable that holds the *count* of fields in the current record. |
| **`NR`** | Number of Records | N/A | A *total count* of records (lines) processed since the program started. |
| **`FNR`** | File Number of Records | N/A | The current record number *in the current file*. (Useful when reading multiple files). |
| **`FILENAME`** | Filename | N/A | The name of the file `awk` is currently reading. |
| **`IGNORECASE`** | Ignore Case | `0` (false) | If set to `1`, `awk` performs case-insensitive pattern matching. |

### 🚀 Example: Changing the Output Record Separator (`ORS`)

As a simple example, you can print a blank line after each line of a file by changing `ORS` from its default of one newline (`\n`) to two newlines (`\n\n`).

**1. Create a File (`columns.txt`)**

```bash
nano columns.txt
```

Paste the following lines and save the file:

```
This is line one.
This is line two.
```

**2. Run the `awk` Command**

```bash
cat columns.txt | awk 'BEGIN { ORS ="\n\n" } ; { print $0 }'
```

**3. Output (Double-Spaced)**

```
This is line one.

This is line two.

```

**4. Code Explanation**

  * **`BEGIN { ORS ="\n\n" }`**: The `BEGIN` block runs *before* `awk` reads any lines. We use it to set the `ORS` variable to two newlines.
  * **`;`**: A semicolon separates commands.
  * **`{ print $0 }`**: This is the "action." It runs for every line. `$0` is a special variable that means "the entire record" (the whole line).


## How Does the `awk` Command Work?

The `awk` command reads its input one **record** at a time (by default, one record is one line). For each record, it checks a series of **patterns**. If a record matches a pattern, a corresponding **action** is performed.

The basic syntax is: `awk 'pattern { action }'`

### The `awk` Execution Cycle

1.  **`BEGIN` Block (Optional):**

      * A block of code prefixed with `BEGIN` runs *once* before any records are read.
      * This is the perfect place to set variables like `FS` or `ORS` and print headers.

2.  **`pattern { action }` Blocks (The Main Loop):**

      * `awk` reads a record.
      * It checks the record against each `pattern` you've defined.
      * If a `pattern` matches, the `{ action }` (the code in curly braces) is executed.
      * This repeats for *every single record* in the input.

3.  **`END` Block (Optional):**

      * A block of code prefixed with `END` runs *once* after all records have been processed.
      * This is the perfect place to print totals, averages, or a final summary.

### Default Behaviors

  * **If a `pattern` is not given:** The `{ action }` is performed for *every* record.
  * **If an `action` is not given:** The default action is `{ print }` (or `{ print $0 }`), which prints the entire matching record.
  * **Empty braces `{}`:** An empty action does nothing and overrides the default `print` action.


## 🚀 Simple `awk` Examples (Field Printing)

Let's use a simple string to see how `awk` processes fields.

**Setup:**

```bash
x="a b c d e"
```

In the examples below, the **`-F" "`** switch is a shortcut. It sets the **F**ield **S**eparator (`FS`) to a single space before the script begins.

### Example 1: Print the First Field (`$1`)

```bash
echo $x | awk -F" " '{print $1}'
```

**Output:**

```
a
```

**Explanation:** `awk` received the record "a b c d e". It split the record by spaces. The first field (`$1`) was "a".

### Example 2: Print the Number of Fields (`NF`)

```bash
echo $x | awk -F" " '{print NF}'
```

**Output:**

```
5
```

**Explanation:** `awk` split the record into "a", "b", "c", "d", and "e". The **N**umber of **F**ields (`NF`) is 5.

### Example 3: Print the Entire Record (`$0`)

```bash
echo $x | awk -F" " '{print $0}'
```

**Output:**

```
a b c d e
```

**Explanation:** The `$0` variable always holds the entire, unmodified record.

### Example 4: Reorder Fields

```bash
echo $x | awk -F" " '{print $3, $1}'
```

**Output:**

```
c a
```

**Explanation:** This prints the third field (`$3`), followed by a comma, followed by the first field (`$1`). When you use a comma in a `print` statement, `awk` inserts the **O**utput **F**ield **S**eparator (`OFS`), which is a space by default.


## 🚀 Using `BEGIN` to Change Variables

Let’s change the `FS` (Field Separator) to calculate the length of a string. This is a common `awk` trick.

```bash
echo "abc" | awk 'BEGIN { FS = "" } ; { print NF }'
```

**Output:**

```
3
```

**Explanation:**

  * **`BEGIN { FS = "" }`**: Before reading any input, we set the `FS` to an empty string (`""`).
  * This is a special case: it tells `awk` to treat *every single character* as a separate field.
  * **`{ print NF }`**: When `awk` reads "abc", it splits it into three fields: "a", "b", and "c". Therefore, the **N**umber of **F**ields (`NF`) is 3.


## 🚀 Four Ways to Feed Data to `awk`

This simple example of four ways to do the same task should illustrate why including comment statements in `awk` commands of any complexity is almost always a good idea.

**Setup:**
First, create `test.txt`:

```bash
nano test.txt
```

Paste this content and save:

```
col1 col2
col3 col4
```

All four of these commands produce the **exact same output**:

```
col1
col3
```

### Method 1: As a File Argument (Preferred)

```bash
awk '{ print $1 }' test.txt
```

### Method 2: Input Redirection (Before)

```bash
awk < test.txt '{ print $1 }'
```

### Method 3: Input Redirection (After)

```bash
awk '{ print $1 }' < test.txt
```

### Method 4: Using `cat` and a Pipe

```bash
cat test.txt | awk '{ print $1 }'
```

As we have discussed, this method can be inefficient as it uses an extra process (`cat`). Only do this if the `cat` command (or another command) is adding value in some way.

Keep in mind that the next person to examine your code may not be familiar with your coding style, so clarity is key.

---

# 🖨️ Aligning Text with the `printf()` Command

Since `awk` is a programming language inside a single command, it also has its own way of producing formatted output via the `printf()` statement. This is a much more powerful and precise alternative to the standard `print` command.


### 📁 Listing 7.1: `columns2.txt`

First, let's create the sample dataset. This file is intentionally "messy," with a different number of words (fields) on each line.

**1. Create the File**

```bash
nano columns2.txt
```

**2. Paste the Content**
Copy the following lines into the `nano` editor, then save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

```
one two
three four
one two three four
five six
one two three
four five
```


### 📜 Listing 7.2: `AlignColumns1.sh`

Next, here is the `awk` script that will read `columns2.txt` and align its contents into fixed-width columns.

**1. Create the Script**

```bash
nano AlignColumns1.sh
```

**2. Paste the Code**
Copy the following code into `nano`, then save and exit.

```bash
#!/bin/bash
awk '
{
 # left-align $1 on a 10-char column
 # right-align $2 on a 10-char column
 # right-align $3 on a 10-char column
 # right-align $4 on a 10-char column
 printf("%-10s*%10s*%10s*%10s*\n", $1, $2, $3, $4)
}
' columns2.txt
```

**3. Make the Script Executable**

```bash
chmod +x AlignColumns1.sh
```


### 👨‍💻 Detailed Code Explanation

Listing 7.2 contains a single `awk` command that runs the script block `{...}` for *every line* in `columns2.txt`.

The core of this script is the `printf()` statement:

```awk
printf("%-10s*%10s*%10s*%10s*\n", $1, $2, $3, $4)
```

Let's break this down piece by piece:

  * **`printf(...)`**: This stands for "print formatted." Unlike `print`, it does **not** automatically add a newline character.
  * **`"..."`**: This is the **format string**. It's a template that tells `printf` *how* to display the data.
  * **`$1, $2, $3, $4`**: These are the **arguments** (the data) that will be fed into the template. `$1` is the first word on the line, `$2` is the second, and so on.

#### Breakdown of the Format String: `%-10s*%10s*%10s*%10s*\n`

  * **`%-10s`**: This is the first placeholder, for `$1`.
      * `%s`: Stands for "string."
      * `10`: Pads the string with spaces to be **10 characters wide**.
      * `-`: This is the **left-alignment** flag. Without it, the text would be right-aligned.
  * **`*`**: This is a **literal character**. It is printed exactly as shown, acting as a visual separator for our columns.
  * **`%10s`**: This is the placeholder for `$2`, `$3`, and `$4`.
      * `%s`: Stands for "string."
      * `10`: Pads the string to be **10 characters wide**.
      * *(No `-` flag)*: By default, `printf` uses **right-alignment**.
  * **`\n`**: This is the **newline character**. We must add this manually, or all the output would print on a single, long line.

**What happens to missing fields?**
On lines like "one two", the variables `$3` and `$4` are just empty strings (`""`). `printf` still pads these empty strings to be 10 characters wide, giving you 10 spaces.


### 🚀 Run and Analyze Output

When you launch the script, `awk` processes each line of `columns2.txt` and formats it.

**Run the script:**

```bash
./AlignColumns1.sh
```

**Output:**
*(This output is formatted in a code block to preserve the exact spacing)*

```
one       *       two*          *          *
three     *      four*          *          *
one       *       two*     three*      four*
five      *       six*          *          *
one       *       two*     three*          *
four      *      five*          *          *
```

**Output Analysis (Line by Line):**

  * **`one * two* * *`**

      * `%-10s` ($1="one") -\> `"one       "` (left-aligned)
      * `%10s` ($2="two") -\> `"       two"` (right-aligned)
      * `%10s` ($3="") -\> `"          "` (right-aligned)
      * `%10s` ($4="") -\> `"          "` (right-aligned)

  * **`one * two* three* four*`**

      * `%-10s` ($1="one") -\> `"one       "` (left-aligned)
      * `%10s` ($2="two") -\> `"       two"` (right-aligned)
      * `%10s` ($3="three") -\> `"     three"` (right-aligned)
      * `%10s` ($4="four") -\> `"      four"` (right-aligned)

  * **`one * two* three* *`**

      * `%-10s` ($1="one") -\> `"one       "` (left-aligned)
      * `%10s` ($2="two") -\> `"       two"` (right-aligned)
      * `%10s` ($3="three") -\> `"     three"` (right-aligned)
      * `%10s` ($4="") -\> `"          "` (right-aligned)

Keep in mind that `printf` is reasonably powerful, and as such, it has its own complex syntax, which is beyond the scope of this chapter. A search online can find the manual pages and also discussions of “how to do X with printf().”

---

# 🧠 Conditional Logic and Control Statements in `awk`

Like other programming languages, `awk` provides support for conditional logic (**`if/else`**) and control statements (**`for/while`** loops).

The unique power of `awk` is that it allows you to place this complex logic *inside* a piped command stream. This avoids the need to create, install, and add a custom executable shell script to your path for many common tasks.

## 1\. The `if/else` Statement

The following code block shows you how to use `if/else` logic.

*(Note: The `if/else` syntax in the original text was invalid. The code below has been corrected to be fully runnable.)*

#### 📜 Code Snippet

This script runs in the main action block, which executes once for the empty string provided by `echo`.

```bash
echo "" | awk '
BEGIN { x = 10 }
{
 if (x % 2 == 0) {
    print "x is even"
 } else {
    print "x is odd"
 }
}
'
```

#### 🖥️ Output

```
x is even
```

#### 👨‍💻 Code Explanation

  * **`BEGIN { x = 10 }`**: The `BEGIN` block runs *once* before any input is processed. It initializes a variable `x` with the value `10`.
  * **`{ ... }`**: This is the main action block. It runs for each line of input (in this case, it runs once for the single, empty line from `echo ""`).
  * **`if (x % 2 == 0)`**: This is the condition.
      * `x % 2`: The `%` (modulo) operator calculates the remainder of `x` divided by 2.
      * `== 0`: It checks if the remainder is equal to 0.
  * **`{ print "x is even" }`**: This action block is executed if the condition is **true**.
  * **`else { print "x is odd" }`**: This action block is executed if the condition is **false**.

Since `x` is 10, the remainder is 0, so the `if` block is executed, printing "x is even".


## 2\. The `while` Statement

The following code block illustrates how to use a `while` loop in `awk`. A `while` loop will continue to run as long as its condition is true.

#### 📜 Code Snippet

```bash
echo "" | awk '
{
 x = 0
 while(x < 4) {
    print "x:",x
    x = x + 1
 }
}
'
```

#### 🖥️ Output

```
x: 0
x: 1
x: 2
x: 3
```

#### 👨‍💻 Code Explanation

1.  **`x = 0`**: A variable `x` is initialized to 0.
2.  **`while(x < 4)`**: The loop checks the condition. Is 0 less than 4? Yes.
3.  **`print "x:",x`**: Prints `x: 0`.
4.  **`x = x + 1`**: Increments `x` to 1.
5.  The loop repeats. Is 1 less than 4? Yes. It prints `x: 1` and increments `x`.
6.  This continues until `x` becomes 4. The condition `(4 < 4)` is now **false**, and the loop terminates.


## 3\. The `do-while` Statement

The following code block illustrates how to use a `do-while` loop. This type of loop is very similar to a `while` loop, with one key difference: the code block is executed *first*, and the condition is checked *afterward*. This guarantees the loop will run at least once.

#### 📜 Code Snippet

```bash
echo "" | awk '
{
 x = 0
 do {
    print "x:",x
    x = x + 1
 } while(x < 4)
}
'
```

#### 🖥️ Output

```
x: 0
x: 1
x: 2
x: 3
```

#### 👨‍💻 Code Explanation

The execution flow is almost identical to the `while` loop. The main difference would be if the condition was initially false (e.g., `x = 5`). A `while` loop would run zero times, but a `do-while` loop would run once, print `x: 5`, and then terminate.


## 4\. A `for` Loop in `awk`

Listing 7.3 displays the content of `Loop.sh` that illustrates how to print a list of numbers in a loop.

#### 📜 Listing 7.3: `Loop.sh`

```bash
echo "" | awk '
BEGIN {}
{
 for(i=0; i<5; i++) {
    printf("%3d", i)
 }
}
END { print "\n" }
'
```

#### 🖥️ Output

```
  0  1  2  3  4
```

#### 👨‍💻 Code Explanation

  * **`for(i=0; i<5; i++)`**: This is a C-style `for` loop with three parts:
    1.  **Initialization (`i=0`)**: A counter `i` is set to 0 before the loop starts.
    2.  **Condition (`i<5`)**: The loop will run as long as `i` is less than 5.
    3.  **Increment (`i++`)**: After each loop iteration, `i` is incremented by 1. (`i++` is a shortcut for `i=i+1`).
  * **`printf("%3d", i)`**: This is the **print formatted** command.
      * `printf` does **not** add a newline after it prints, unlike `print`.
      * `"%3d"`: This is the format specifier.
          * `d`: Print a `d`ecimal integer (a number).
          * `3`: Pad the number with spaces to be 3 characters wide.
  * **`END { print "\n" }`**: The `END` block runs *after* all input is processed. This is used to print a single newline character (`\n`) to move the command prompt to the next line after the loop's output.


## 5\. A `for` Loop with a `break` Statement

The following code block illustrates how to use a **`break`** statement. `break` will exit a loop immediately, regardless of the loop's condition.

#### 📜 Code Snippet

```bash
echo "" | awk '
{
 for(x=1; x<4; x++) {
    print "x:",x
    if(x == 2) {
       break;
    }
 }
}
'
```

#### 🖥️ Output

*(Note: The typo `X:2` from the original text has been corrected to `x:2`.)*

```
x: 1
x: 2
```

#### 👨‍💻 Code Explanation

1.  The loop starts with `x=1`. It prints `x: 1`. The `if` condition (`1 == 2`) is false.
2.  `x` is incremented to 2. The loop runs.
3.  It prints `x: 2`.
4.  The `if` condition (`2 == 2`) is **true**.
5.  The **`break;`** statement is executed.
6.  `awk` immediately exits the `for` loop. The loop *does not* run for `x=3`.


## 6\. The `next` and `continue` Statements

The `next` and `continue` statements are powerful control commands, but they do very different things.

  * **`next`**: This command stops processing the *current line* of input and tells `awk` to immediately move to the **next line** of the input file.
  * **`continue`**: This command is used *inside a loop* (`for` or `while`). It stops the *current loop iteration* and tells `awk` to immediately start the **next iteration** of the loop.

The conceptual example in the text is difficult to run. Here is a complete, practical example that demonstrates both commands.

### 📜 Complete Example: `next` vs. `continue`

First, let's create a file to process.

**1. Create `records.txt`**

```bash
nano records.txt
```

Paste the following content and save:

```
USER:admin
# This is a comment, skip it
USER:guest
```

**2. Create the `awk` Command**
This script will skip any line containing `#` and will also skip iteration 2 of its internal loop.

```bash
awk '
BEGIN { FS=":" }

# 1. Check for a comment. If found, stop processing this line.
/#/ { 
    print "--> Found a comment, skipping line."
    next 
}

# 2. This block only runs if "next" was NOT called.
{
    print "Processing record for user:", $2
    
    # 3. A loop to demonstrate "continue"
    for(i=1; i<4; i++) {
        if (i == 2) {
            print "  - Skipping iteration 2 with (continue)..."
            continue # Skips the rest of this loop iteration
        }
        print "  - Iteration", i
    }
}
' records.txt
```

#### 🖥️ Output

```
Processing record for user: admin
  - Iteration 1
  - Skipping iteration 2 with (continue)...
  - Iteration 3
--> Found a comment, skipping line.
Processing record for user: guest
  - Iteration 1
  - Skipping iteration 2 with (continue)...
  - Iteration 3
```

#### 👨‍💻 Code Explanation

1.  **Line 1 ("USER:admin"):**
      * The `/#/` pattern is not found.
      * The main block runs. It prints `Processing record for user: admin`.
      * The `for` loop starts.
      * `i=1`: Prints `Iteration 1`.
      * `i=2`: The `if` is true. It prints `Skipping iteration...` and `continue` is called. The `print " - Iteration", i` line is skipped.
      * `i=3`: Prints `Iteration 3`.
2.  **Line 2 ("\# This is a comment..."):**
      * The `/#/` pattern **is found**.
      * It prints `--> Found a comment, skipping line.`.
      * The **`next`** command is called. `awk` *immediately* stops processing this line and moves to the next line of input. The main block and `for` loop are never run for this line.
3.  **Line 3 ("USER:guest"):**
      * The `/#/` pattern is not found.
      * The main block runs, and the `for` loop logic repeats, just as it did for the "admin" user.

---