# 📜 Introduction to `sed` (Stream Editor)

In the prior section, we learned how to reduce a stream of data to only the contents that interested us. In this section, we learn how to **transform** that data using the Unix `sed` utility, which is an acronym for “stream editor.”

## 📚 What You Will Learn

* **Part 1: Basic Examples**
    The first part of this section contains basic examples of the `sed` command, such as replacing and deleting strings, numbers, and letters.

* **Part 2: Switches and Delimiters**
    The second part of this section discusses various switches (options) that are available for the `sed` command, along with an example of replacing multiple delimiters with a single delimiter in a dataset.

* **Part 3: Stream-Oriented Processing**
    In the final section, you will see a number of examples of how to perform stream-oriented processing on datasets. This brings the capabilities of `sed` together with the commands and regular expressions from prior sections to accomplish difficult tasks with relatively simple code.

---

## ❓ What is the `sed` Command?

The name `sed` is an acronym for “**stream editor**.” This utility derives many of its commands from the `ed` line editor (`ed` was the first Unix text editor).

The `sed` command is a **“non-interactive” stream-oriented editor**. It is primarily used to automate editing tasks via shell scripts.

This ability to modify an entire stream of data (which can be the contents of multiple files, in a manner similar to how `grep` behaves) as if you were inside an editor is not common in modern programming languages.

This behavior allows for some capabilities not easily duplicated elsewhere. At the same time, `sed` behaves exactly like any other command (such as `grep`, `cat`, `ls`, `find`, and so forth) in how it can accept data, output data, and pattern match with regular expressions.

Some of the more common uses for `sed` include:
* Printing matching lines.
* Deleting matching lines.
* Finding and replacing matching strings or regular expressions.

### 🔄 The `sed` Execution Cycle

Whenever you invoke the `sed` command, it begins an **execution cycle**. This cycle refers to the various options that are specified and executed for every single line of input, continuing until the end of the file/input stream is reached.

Specifically, *for each line of input*, an execution cycle performs the following steps:

1.  **Reads** an entire line from the input (stdin or a file).
2.  **Removes** any trailing newline character.
3.  **Places** the line into its **pattern buffer** (a temporary holding space).
4.  **Modifies** the pattern buffer according to the supplied commands (e.g., "substitute 'A' with 'B'").
5.  **Prints** the (possibly modified) pattern buffer to standard output (stdout).


---

# 🎯 Matching String Patterns Using `sed`

The `sed` command requires you to specify a string to match the lines in a file. This guide will explain how to do this from scratch.

-----

## 📁 Setup: Create the Example File

First, we need the `numbers.txt` file that is used in these examples.

**1. Create the file:**
Open your terminal and use `nano` (or your preferred text editor) to create the file:

```bash
nano numbers.txt
```

**2. Add Content:**
Paste the following lines into the editor:

```
1
2
123
3
five
4
```

**3. Save and Exit:**
Press **`Ctrl + O`** (Write Out), press **`Enter`** to save, and then **`Ctrl + X`** to exit `nano`.

-----

## 1\. Basic Pattern Matching (Printing Matches)

The most common use of `sed` is to find and print lines that match a specific pattern. Let's find all lines that contain the string "3".

Here is the most common and efficient way to write the command:

```bash
sed -n "/3/p" numbers.txt
```

### 🖥️ Output

```
123
3
```

### 👨‍💻 Code Explanation

Let's break down this command piece by piece:

  * **`sed`**: This invokes the **s**tream **ed**itor.
  * **`-n`**: This is a very important option. By default, `sed`'s execution cycle prints *every single line* it reads. The `-n` option **suppresses** this automatic, "default" printing.
  * **`"/3/p"`**: This is the instruction or "script" we give to `sed`.
      * **`/3/`**: This is the **pattern** to search for. The slashes (`/`) are delimiters, and `3` is the string we want to match.
      * **`p`**: This is the **p**rint command. It tells `sed` to print the line if, and only if, the pattern (`/3/`) was found.
  * **`numbers.txt`**: This is the input file `sed` will read from.

So, the command tells `sed`: "Do not print anything by default (`-n`). Read `numbers.txt` line by line. If you find a line that matches the pattern `/3/`, execute the `p`rint command for that line."

-----

## 2\. Efficiency: `cat` vs. Direct File Read

You will often see the same command written a different way, using `cat` and a pipe (`|`).

**The Inefficient Way:**

```bash
cat numbers.txt | sed -n "/3/p"
```

This produces the exact same result as our first command. However, as noted, it is less efficient.

  * The **`cat | sed`** method uses **two processes**. `cat`'s only job is to read the file and "pipe" its contents into `sed`.
  * The **`sed ... numbers.txt`** method uses **one process**. `sed` is perfectly capable of reading a file on its own.

As we saw earlier with other commands, it is always more efficient to just read in the file using the `sed` command than to pipe it in. You should only "feed" data from another command if that command *adds value* (such as `cat -n` adding line numbers or `grep` filtering the file first).

-----

## 3\. What Happens If You Omit the `-n`? (Duplication)

This is a very common point of confusion. What happens if you forget the `-n` option?

Let's run the command:

```bash
sed "/3/p" numbers.txt
```

### 🖥️ Annotated Output

The text provides an excellent, annotated explanation of this output. `sed`'s default behavior is to print **every line** *before* it even checks your command.

Here is a step-by-step breakdown of the execution cycle:

| Line Read | `sed`'s Default Print | Pattern `/3/` Match? | Run `p` Command? | Final Output |
| :--- | :--- | :--- | :--- | :--- |
| `1` | `1` | No | No | `1` |
| `2` | `2` | No | No | `2` |
| `123` | `123` | **Yes** | **Yes** -\> `123` | `123`<br>`123` |
| `3` | `3` | **Yes** | **Yes** -\> `3` | `3`<br>`3` |
| `five` | `five` | No | No | `five` |
| `4` | `4` | No | No | `4` |

If you omit the `-n` option, *every line is printed once by default*. The `p` option then causes *any matching line to be printed a second time*.

-----

## 4\. Matching a Range of Lines

It is also possible to match two patterns and print everything *between* the lines that match. This is a very powerful feature.

The command finds the line with "123", starts printing, and continues printing every line until it finds the line with "five".

```bash
sed -n "/123/,/five/p" numbers.txt
```

### 🖥️ Output

```
123
3
five
```

### 👨‍💻 Code Explanation

  * **`sed -n ...`**: We use `-n` again because we *only* want to print the lines specified by our command.
  * **`"/123/,/five/p"`**: This is a **range** instruction.
      * **`/123/`**: This is the **START** pattern.
      * **`/five/`**: This is the **END** pattern.
      * **`,p`**: This applies the `p`rint command to every line *within that range* (inclusive).

This tells `sed`: "Start printing when you see a line that matches `/123/`, and continue printing every line until you see a line that matches `/five/`."

---