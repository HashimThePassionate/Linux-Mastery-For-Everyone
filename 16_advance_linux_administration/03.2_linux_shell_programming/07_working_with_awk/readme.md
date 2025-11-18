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