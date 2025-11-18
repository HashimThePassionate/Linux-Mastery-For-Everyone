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