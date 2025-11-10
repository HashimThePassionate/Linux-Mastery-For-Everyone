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