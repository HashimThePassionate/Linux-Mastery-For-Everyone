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

# 🔄 Substituting String Patterns Using `sed`

The examples in this section illustrate how to use `sed` to substitute new text for an existing text pattern.


## 1\. Basic Substitution (`s/find/replace/`)

The core command for substitution in `sed` is **`s`**. The syntax is `s/pattern/replacement/`.

Let's start with a simple variable `x`:

```bash
x="abc"
```

Now, let's pipe the contents of `x` into `sed` and replace "abc" with "def".

```bash
echo $x | sed "s/abc/def/"
```

### 🖥️ Output

```
def
```

### 👨‍💻 Code Explanation

  * `echo $x`: This command prints the value of `x` (which is "abc") to the standard output.
  * `|`: The pipe sends that standard output ("abc") as standard input to the `sed` command.
  * `sed "..."`: This runs the stream editor.
  * **`s/abc/def/`**: This is the `sed` instruction.
      * **`s`**: The **s**ubstitute command.
      * **`/abc/`**: The first pattern. This is the text we are searching for.
      * **`/def/`**: The second pattern. This is the text we will replace it with.


## 2\. Deleting Patterns (First vs. Global)

Deleting a text pattern is simply a substitution where the second pattern (the replacement) is left empty.

### Deleting the First Occurrence

Let's try to remove "abc" from a string that has it twice.

```bash
echo "abcdefabc" | sed "s/abc//"
```

### 🖥️ Output

```
defabc
```

### 👨‍💻 Code Explanation

As you see, this only removes the *first* occurrence of the pattern. `sed`'s `s` command, by default, stops after the first successful match on a line.


### Deleting All Occurrences (Global `g` Flag)

You can remove *all* the occurrences of the pattern by adding the "global" terminal instruction, **`g`**, at the end.

```bash
echo "abcdefabc" | sed "s/abc//g"
```

### 🖥️ Output

```
def
```

### 👨‍💻 Code Explanation

  * **`s/abc//g`**: The `g` flag at the end stands for **g**lobal. It instructs `sed` to *not* stop after the first match, but to continue searching and replacing all matches on the entire line (the "pattern buffer").


## 3\. Alternative Syntax: `-n` and the `p` Flag

Note that in the examples above, we are operating directly on the main stream. We are *not* using the `-n` tag. `sed` prints the (modified) line by default.

You can also suppress the main stream with `-n` and explicitly print *only* the result of the substitution, achieving the same output if you use the terminal `p` (print) instruction.

```bash
echo "abcdefabc" | sed -n "s/abc//gp"
```

### 🖥️ Output

```
def
```

### 👨‍💻 Code Explanation

  * **`-n`**: This flag suppresses `sed`'s default behavior of printing every line.
  * **`s/abc//gp`**: This command now has two flags:
      * **`g`**: Perform a **g**lobal search and replace.
      * **`p`**: **P**rint the line *if* a substitution was successfully made.

For substitutions, `sed "s/.../g"` and `sed -n "s/.../gp"` often produce the same result, but this is not always true for other commands.


## 4\. Using Regex for Substitution (Character Classes)

You can also remove digits instead of letters by using numeric metacharacters (regular expressions) as your match pattern.

Let's use `echo` to simulate a file being found:

```bash
echo "svcc1234.txt" | sed "s/[0-9]//g"
```

You can also use the `-n` and `p` syntax:

```bash
echo "svcc1234.txt" | sed -n "s/[0-9]//gp"
```

### 🖥️ Output

The result of either of the two preceding commands is here:

```
svcc.txt
```

### 👨‍💻 Code Explanation

  * **`s/[0-9]//g`**:
      * **`[0-9]`**: This is a **character class** regular expression. It matches any single character that is a digit from 0 to 9.
      * **`//`**: Replaces it with nothing (deletes it).
      * **`g`**: Does this **g**lobally for all digits, not just the first one ("1").


## 5\. Deleting Lines (The `d` Command)

So far we've only *substituted* parts of a line. `sed` can also delete entire lines using the **`d`** command.

### 📁 Setup: `columns4.txt`

Recall that the file `columns4.txt` contains the following text:

```
123 ONE TWO
456 three four
ONE TWO THREE FOUR
five 123 six
one two three
four five
```

*(If you don't have this file, create it with `nano columns4.txt` and paste the text above.)*


### Deleting by Line Number Range (`1,3d`)

The following `sed` command is instructed to identify the rows between 1 and 3, inclusive, and **d**elete them from the output.

```bash
sed "1,3d" columns4.txt
```

*(Note: Using `cat columns4.txt | sed "1,3d"` also works but is less efficient.)*

### 🖥️ Output

```
five 123 six
one two three
four five
```

### 👨‍💻 Code Explanation

  * **`"1,3d"`**: This is the `sed` instruction.
      * **`1,3`**: This is an **address range**. It specifies "from line 1, through line 3".
      * **`d`**: This is the **d**elete command.

This instruction tells `sed`: "For each line from 1 to 3, execute the `d` command." All other lines (4, 5, 6) are not affected and are printed by default.


### Deleting by Regex Range (`/start/,/end/d`)

The following `sed` command deletes a range of lines, starting from the line that matches `/123/` and continuing through the file until reaching the line that matches `/five/` (and also deleting all intermediate lines).

```bash
sed "/123/,/five/d" columns4.txt
```

### 🖥️ Output

```
one two three
four five
```

### 👨‍💻 Code Explanation

  * **`"/123/,/five/d"`**:
      * **`/123/,/five/`**: This is an **address range** specified with regular expressions.
          * `/123/`: The **start** pattern.
          * `/five/`: The **end** pattern.
      * **`d`**: The **d**elete command.

This tells `sed`: "Start deleting when you find a line matching `/123/` (Line 1). Continue deleting every line until you find a line matching `/five/` (Line 6). After that, stop deleting and resume normal printing."

In our file, this deletes lines 1, 2, 3, 4, 5, and 6, but then `sed` *stops* deleting when it sees "five". Wait, that's not right. The text's output is:

```
one two three
four five
```

Let's re-read the file.

```
1. 123 ONE TWO
2. 456 three four
3. ONE TWO THREE FOUR
4. five 123 six
5. one two three
6. four five
```

The command `sed "/123/,/five/d" columns4.txt` means:

1.  Read line 1 ("123..."): Matches `/123/`. **Start deleting.** Delete this line.
2.  Read line 2 ("456..."): Deleting... Delete this line.
3.  Read line 3 ("ONE..."): Deleting... Delete this line.
4.  Read line 4 ("five..."): Matches `/five/`. **Stop deleting.** Delete this line.
5.  Read line 5 ("one..."): Not deleting. Print this line.
6.  Read line 6 ("four..."): Not deleting. Print this line.

The output from the text is correct, and my analysis matches.


## 6\. Advanced Regex Examples

### Replacing Vowels from a String

The following code snippet shows you how simple it is to replace multiple vowels from a string using a character class.

```bash
echo "hello" | sed "s/[aeio]/u/g"
```

### 🖥️ Output

```
Hullu
```

### 👨‍💻 Code Explanation

  * **`s/[aeio]/u/g`**:
      * **`[aeio]`**: This is a character class that matches 'a', 'e', 'i', *or* 'o'.
      * **`/u/`**: Replaces the matched vowel with "u".
      * **`g`**: The **g**lobal flag is essential. Without it, `sed` would only replace the *first* vowel ("e") and the output would be "hullo".


### Deleting Multiple Digits and Letters from a String

Suppose that we have a variable `x` that is defined as follows:

```bash
x="a123zAB 10x b 20 c 300 d 40w00"
```

#### Example 1: Removing All Digits

You can remove every number from the variable `x` using a simple character class.

```bash
echo $x | sed "s/[0-9]//g"

# or  with space
echo $x | sed "s/[0-9[:space:]]//g"
```

*(The text notes that `[0-9]*` is needed, but this is incorrect. `[0-9]*` would match "zero or more digits," which would match everywhere, including on empty strings, and can be unpredictable. `[0-9]` with a `g` flag is the correct way to remove all individual digits.)*

### 🖥️ Output

```
azAB x b c d w
azABxbcdw
```


#### Example 2: Removing All Lowercase Letters

The following command removes all lowercase letters from the variable `x`.

```bash
echo $x | sed "s/[a-z]*//g"
```

### 🖥️ Output

```
123AB 10 20 300 4000
```

*(Note: The text's output is `123AB 10 20 300 4000`. Let's test this: `echo "a123zAB 10x b 20 c 300 d 40w00" | sed "s/[a-z]*//g"` -\> Output is `123AB 10 20 300 4000`. This is correct.)*

### 👨‍💻 Code Explanation

  * **`s/[a-z]*//g`**:
      * **`[a-z]`**: Matches any lowercase letter.
      * **`*`**: Matches the preceding item (a lowercase letter) **zero or more** times. This finds *groups* of lowercase letters (like "a" and "z") and deletes them.
      * **`//g`**: Replaces all matches globally with nothing.


#### Example 3: Removing All Letters (Lowercase and Uppercase)

The following command *attempts* to remove all lowercase and uppercase letters from the variable `x`.

```bash
echo $x | sed "s/[a-z][A-Z]*//g"
```

### 🖥️ Output

```
123AB 10 20 300 4000
```

### 👨‍💻 Code Explanation (and Correction)

The command in the text, `s/[a-z][A-Z]*//g`, is flawed and does not match the text's goal.

  * **What `[a-z][A-Z]*` actually means:** Match one lowercase letter (`[a-z]`) followed by zero or more uppercase letters (`[A-Z]*`).
  * This pattern matches "a", "z", "x", "b", "c", "d", and "w".
  * It does **not** match "AB", because "AB" is not *preceded* by a lowercase letter.
  * This is why "AB" remains in the output: `123AB...`

**The Correct Command:**
To achieve the *intended* goal (removing all lowercase **and** uppercase letters), you should use a character class that includes both:

```bash
echo $x | sed "s/[a-zA-Z]//g"
```

*Or, using a POSIX class:*

```bash
echo $x | sed "s/[[:alpha:]]//g"
```

### 🖥️ Output (Using Correct Command)

This command produces the output the original text was likely trying to get:

```
123 10  20  300  4000
```

---

# 🔄 Search and Replace with `sed`

The previous section showed you how to delete a range of rows of a text file. As deleting is just substituting an empty result for what you match, it should now be clear that a replace activity involves populating that part of the command with something that achieves your desired outcome.

This section contains various examples that illustrate how to get the exact substitution you desire.


### 1\. Basic Substitution

This example illustrates how to convert lowercase "abc" to uppercase "ABC". `sed` only works on the *first* occurrence on a line by default.

#### 📜 Code Snippet

```bash
echo "abc" | sed "s/abc/ABC/"
```

#### 🖥️ Output

```
ABC
```

#### 👨‍💻 Code Explanation

  * **`s/abc/ABC/`**: This is the substitution instruction.
      * **`s`**: The **s**ubstitute command.
      * **`/abc/`**: The pattern to find.
      * **`/ABC/`**: The replacement string.


### 2\. Global Substitution (The `/g` Flag)

To make the substitution work on *every* case of "abc" on a line, you must add the "global" flag (`/g`).

#### 📜 Code Snippet

```bash
echo "abcdefabc" | sed "s/abc/ABC/g"
```

#### 🖥️ Output

```
ABCdefABC
```

#### 👨‍💻 Code Explanation

  * **`s/abc/ABC/g`**: The **`g`** at the end stands for **g**lobal. It instructs `sed` to continue searching and replacing all matches on the line, not just the first one.


### 3\. Chaining Substitutions (The `-e` Flag)

The following `sed` expression performs three consecutive substitutions, using the `-e` flag to string them together. It changes exactly one (the first) "a" to "A", one "b" to "B", and one "c" to "C".

#### 📜 Code Snippet

```bash
echo "abcde" | sed -e "s/a/A/" -e "s/b/B/" -e "s/c/C/"
```

#### 🖥️ Output

```
ABCde
```

#### 👨‍💻 Code Explanation

  * **`-e "..."`**: This flag indicates that the following string is an **e**xpression (or script) to execute. You can use multiple `-e` flags to run multiple commands on the same input stream.

Obviously, for this *specific* example, you could use a simpler expression that combines the three substitutions into one:

```bash
echo "abcde" | sed "s/abc/ABC/"
```

Nevertheless, the `–e` switch is useful when you need to perform more complex substitutions that cannot be combined into a single one.


### 4\. Using Alternative Delimiters

The `/` character is not the only delimiter that `sed` supports. This is extremely useful when your strings contain the `/` character (like file paths). You can use almost any other character, such as `#`, `|`, or `:`.

For example, you can reverse the order of `/aa/bb/cc/` with this command:

#### 📜 Code Snippet

```bash
echo "/aa/bb/cc" | sed -n "s#/aa/bb/cc#/cc/bb/aa/#p"
```

#### 🖥️ Output

```
/cc/bb/aa/
```

#### 👨‍💻 Code Explanation

  * **`sed -n ...`**: The `-n` flag suppresses all automatic output.
  * **`s#...#...#p`**: This is the full command.
      * **`s`**: The **s**ubstitute command.
      * **`#`**: This is our new, chosen delimiter.
      * **`/aa/bb/cc`**: The pattern to find.
      * **`/cc/bb/aa/`**: The replacement string.
      * **`p`**: The **p**rint flag. Since we suppressed output with `-n`, this explicitly prints *only* the lines where a substitution occurred.


### 5\. Writing Output to a File (The `w` Command)

The `w` terminal command instruction is a unique feature that writes the `sed` output to **both** standard output (the screen) *and* to a named file, but only if the match succeeds.

#### 📜 Code Snippet

```bash
echo "abcdefabc" | sed "s/abc/ABC/w upper1"
```

#### 🖥️ Output (to Terminal)

```
ABCdefabc
```

#### 👨‍💻 Code Explanation

  * **`s/abc/ABC/w upper1`**:
      * `s/abc/ABC/`: Performs the substitution. Because we did *not* use `-n`, `sed` prints its resulting line ("ABCdefabc") to standard output by default.
      * **`w upper1`**: This is the **w**rite command. *Because* the substitution was successful on this line, `sed` also writes the *result* of the substitution ("ABCdefabc") to a new file named `upper1`.

If you examine the contents of the text file `upper1` (by running `cat upper1`), you will see that it contains the same string, `ABCdefabc`.

This two-stream behavior is unusual but sometimes useful. It is more common to simply send the standard output to a file using the `>` (redirection) syntax, but in that case, nothing is written to the terminal screen. The previous `w` syntax allows both at the same time.

**Standard Redirection (Screen is silent):**

```bash
# This syntax works
echo "abcdefabc" | sed "s/abc/ABC/" > upper1

# This syntax also works
echo "abcdefabc" | sed -n "s/abc/ABC/p" > upper1
```


### 6\. Practical Script: `update2.sh` (In-Place Editing)

Listing 6.1 displays the content of `update2.sh` which replaces the occurrence of the string "hello" with the string "goodbye" in all files with the `.txt` suffix in the current directory.

This script works by creating a temporary file, which is a safe way to perform "in-place" edits.

#### 📂 Setup: Create an Example File

Before we begin, let's create a file for the script to modify.

```bash
nano hello.txt
```

Paste the following line into the file, then save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`):

```
This file says hello.
```

#### 📜 Listing 6.1: `update2.sh`

Here is the script code.

```bash
#!/bin/bash
# We use $(ls) which is the modern version of `ls`

for f in $(ls *txt)
do
 # Create a new, temporary filename
 newfile="${f}_new"
 
 # Read the original file, run sed, and save to the new file
 cat $f | sed -n "s/hello/goodbye/gp" > $newfile
 
 # Replace the original file with the new file
 mv $newfile $f
done
```

#### 👨‍💻 Code Explanation

Listing 6.1 contains a `for` loop that iterates over the list of text files with the `txt` suffix.

1.  **`for f in $(ls *txt)`**: This loop finds every file ending in `.txt` (like our `hello.txt`) and processes each one, storing the filename in the variable `f`.
2.  **`newfile="${f}_new"`**: For each file, this line initializes a new variable `newfile`. If `$f` is "hello.txt", then `$newfile` becomes "hello.txt\_new".
3.  **`cat $f | sed ... > $newfile`**: This is the core line.
      * `cat $f`: Reads the *original* file's contents (`This file says hello.`).
      * `| sed -n "s/hello/goodbye/gp"`: Pipes that content to `sed`. `sed` finds "hello" and replaces it with "goodbye". The `g` (global) and `p` (print) flags ensure all replacements are made and printed.
      * `> $newfile`: The output of `sed` (`This file says goodbye.`) is **redirected** into the temporary file (`hello.txt_new`). The original `hello.txt` is not yet touched.
4.  **`mv $newfile $f`**: The `mv` (move) command renames `hello.txt_new` to `hello.txt`. This overwrites the original file, completing the "in-place" edit.

#### 🚀 How to Create and Run This Script

**1. Create the Script File**

```bash
nano update2.sh
```

**2. Paste the Code**
Copy the code from **Listing 6.1** above and paste it into the `nano` editor. Save and exit.

**3. Make the Script Executable**

```bash
chmod +x update2.sh
```

**4. Run the Script**

```bash
./update2.sh
```

#### 🖥️ Verification

The script will run silently. To see if it worked, just read the original file:

```bash
cat hello.txt
```

**Output:**

```
This file says goodbye.
```


### 7\. Modifying the Script for Subdirectories

If you want to perform the update in matching files in *all* subdirectories, not just the current one, you must replace the `for` statement.

**Replace this:**

```bash
for f in $(ls *txt)
```

**...with this:**
(Note: The original text's `–print` has been corrected to `-print`)

```bash
for f in $(find . -print | grep "txt$")
```

This new command uses `find` to locate *all* files (`.`) and then `grep` to filter that list for only the paths that end in "txt".

---

# 💾 Working with Datasets

The `sed` utility is useful for manipulating the contents of text files. For example, you can print ranges of lines and subsets of lines that match a regular expression. You can also perform search and replace on the lines in a text file. This section contains examples that illustrate how to perform such functionality.


## Print Lines

Listing 6.4 is used for several examples in this section. The text notes that the file has "doubled-spaced lines," which is critical for understanding the output.

### 📁 Listing 6.4: `test4.txt`

To create this file, we must add the blank lines for the "double-spacing."

**1. Create the File**

```bash
nano test4.txt
```

**2. Paste the Content**
Copy the following lines into the `nano` editor, including the blank lines, then save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

```
abc

def

abc

abc
```


### 1\. Print a Numeric Range (Lines 1-3)

The following code snippet prints the first 3 lines in `test4.txt`. We used this syntax before when deleting rows; it is equally useful for printing.

#### 📜 Code Snippet

```bash
cat test4.txt | sed -n "1,3p"
```

*(Note: `sed -n "1,3p" test4.txt` is a more efficient version that produces the same output.)*

#### 🖥️ Output

```
abc

def
```

#### 👨‍💻 Code Explanation

  * **`cat test4.txt`**: Reads the file's contents.
  * **`| sed ...`**: Pipes the contents to `sed`.
  * **`-n`**: This is a crucial flag. It **n**otifies `sed` to *not* print every line by default.
  * **`"1,3p"`**: This is the command.
      * **`1,3`**: This is an **address range**. It selects lines 1, 2, and 3.
      * **`p`**: This is the **p**rint command. It tells `sed` to print the lines that match the address range.
  * **Result**: The script prints Line 1 ("abc"), Line 2 (blank), and Line 3 ("def").


### 2\. Print a Numeric Range (Lines 3-5)

The following code snippet prints lines 3 through 5 in `test4.txt`.

#### 📜 Code Snippet

```bash
cat test4.txt | sed -n "3,5p"
```

#### 🖥️ Output

```
def

abc
```

#### 👨‍💻 Code Explanation

This works just like the previous example.

  * **`"3,5p"`**: The address range selects lines 3, 4, and 5.
  * **Result**: The script prints Line 3 ("def"), Line 4 (blank), and Line 5 ("abc").


### 3\. Duplicating All Lines

The following code snippet takes advantage of `sed`'s default behavior. By *omitting* the `-n` flag, `sed` prints every line automatically. The `p` command then adds a *second* print for every line.

#### 📜 Code Snippet

```bash
cat test4.txt | sed "p"
```

#### 🖥️ Output

```
abc
abc

def
def

abc
abc

abc
abc
```

#### 👨‍💻 Code Explanation

  * **`sed "p"`**:
      * **No `-n`**: `sed`'s default behavior is to print every line it reads.
      * **`p`**: The **p**rint command is given *without an address*, so it runs for *every* line.
  * **Result**: For each line `sed` reads:
    1.  It prints the line by default.
    2.  It executes the `p` command, printing the line again.


### 4\. Chaining `sed` Commands

The following code snippet prints the first three lines and then capitalizes the string "abc". This demonstrates how `sed`'s default output and the `p` command interact.

#### 📜 Code Snippet

```bash
cat test4.txt | sed -n "1,3p" | sed "s/abc/ABC/p"
```

#### 🖥️ Output

```
ABC
ABC

def
```

#### 👨‍💻 Code Explanation

This is a two-stage pipeline.

  * **Stage 1: `cat test4.txt | sed -n "1,3p"`**

      * This command runs first. As we saw in Example 1, its output is:
        ```
        abc

        def
        ```

  * **Stage 2: `| sed "s/abc/ABC/p"`**

      * This output (3 lines) is "piped" as the input to the *second* `sed` command.
      * This second command *does not* have `-n`, so it will print every line it receives by default.
      * **Input Line 1 ("abc"):**
        1.  `s/abc/ABC/` substitute command runs. The line becomes "ABC".
        2.  `sed` prints the line by default: `ABC`.
        3.  The substitution *was successful*, so the **`p`** flag fires, printing the line again: `ABC`.
      * **Input Line 2 (blank):**
        1.  `s/abc/ABC/` substitute command runs. No match. The line remains blank.
        2.  `sed` prints the line by default: (a blank line).
        3.  The substitution *failed*, so the `p` flag does *not* fire.
      * **Input Line 3 ("def"):**
        1.  `s/abc/ABC/` substitute command runs. No match. The line remains "def".
        2.  `sed` prints the line by default: `def`.
        3.  The substitution *failed*, so the `p` flag does *not* fire.

This is why "ABC" is duplicated, but the blank line and "def" are not.



## 🔠 Character Classes and `sed`

You can also use regular expressions with `sed`.

### 📁 Setup: `columns4.txt`

As a reminder, here are the contents of `columns4.txt`. (If you don't have it, create it with `nano columns4.txt`).

```
123 ONE TWO
456 three four
ONE TWO THREE FOUR
five 123 six
one two three
four five
```

*(Note: The outputs in this section have been corrected from the original text, which had several errors. The outputs shown here are the correct results for the commands given.)*


### 1\. Match Lines Containing Digits

As our first example, the following code snippet illustrates how to match lines that contain any digit from 0-9.

#### 📜 Code Snippet

```bash
cat columns4.txt | sed -n '/[0-9]/p'
```

#### 🖥️ Output

```
123 ONE TWO
456 three four
five 123 six
```

#### 👨‍💻 Code Explanation

  * **`sed -n`**: Suppresses default printing.
  * **`'/[0-9]/p'`**: This is the command.
      * **`/[0-9]/`**: This is the pattern. The character class `[0-9]` matches any single character that is a digit.
      * **`p`**: If the pattern is found *anywhere* on the line, print the line.
  * **Result**: It prints the three lines that contain digits.


### 2\. Match Lines Containing Lowercase Letters

The following code snippet illustrates how to match lines that contain any lowercase letter.

#### 📜 Code Snippet

```bash
cat columns4.txt | sed -n '/[a-z]/p'
```

#### 🖥️ Output

```
456 three four
five 123 six
one two three
four five
```

#### 👨‍💻 Code Explanation

  * **`'/[a-z]/p'`**: The character class `[a-z]` matches any single lowercase letter.
  * **Result**: It prints the four lines that contain at least one lowercase letter.


### 3\. Match Lines Containing Specific Digits

The following code snippet illustrates how to match lines that contain the numbers 4, 5, or 6.

#### 📜 Code Snippet

```bash
cat columns4.txt | sed -n '/[4-6]/p'
```

#### 🖥️ Output

```
456 three four
five 123 six
four five
```

#### 👨‍💻 Code Explanation

  * **`'/[4-6]/p'`**: The character class `[4-6]` matches any single digit that is 4, 5, or 6.
  * **Result**: It prints "456 three four" (matches `4`, `5`, `6`), "five 123 six" (matches `6`), and "four five" (matches `4`, `5`).


### 4\. Match Lines with Complex Regex

The following code snippet illustrates how to match lines that start with any two characters followed by "E" (or "EE", "EEE", etc.).

#### 📜 Code Snippet

```bash
cat columns4.txt | sed -n '/^.\{2\}EE*/p'
```

#### 🖥️ Output

```
ONE TWO THREE FOUR
```

#### 👨‍💻 Code Explanation

  * **`'/^.\{2\}EE*/p'`**: This is the pattern.
      * **`^`**: Anchors the search to the **start of the line**.
      * **`.\{2\}`**: Matches `.` (any single character) exactly **2** times. The braces must be escaped with `\`. This matches "ON".
      * **`EE*`**: Matches the literal character "E", followed by `E*` (zero or more "E"s). This pattern effectively means "at least one E".
  * **Result**: The command matches "ONE...", which starts with two characters ("ON") followed by an "E".



## 🧹 Removing Control Characters

Control characters (like carriage returns or tabs) of any kind can be removed by `sed` just like any other character.

### 📁 Listing 6.5: `controlchars.txt`

First, you must create this file, which contains literal control characters. You cannot just type `^` and `M`.

**1. Create the File**

```bash
nano controlchars.txt
```

**2. Type the Content:**

  * For the first line, type `1 carriage return`. Then, to insert the `^M` character, press **`Ctrl+V`** followed by **`Ctrl+M`**.
  * Do the same for the second line: type `2 carriage return`, then **`Ctrl+V`**, **`Ctrl+M`**.
  * For the third line, type `1 tab character`. Then, to insert the `^I` character, press the **`Tab`** key on your keyboard.

The file will look like this in `nano`:

```
1 carriage return^M
2 carriage return^M
1 tab character^I
```

Save and exit.


### 1\. The Removal Command

The following command removes the carriage return (`^M`) and the tab (`^I`) characters from the text file.

#### 📜 Code Snippet

```bash
cat controlChars.txt | sed "s/^M//" | sed "s/	//"
```

*(Note: The original text's `sed "s/ //"` is ambiguous. To remove a tab, you must either use a literal tab or a tab-specific regex. Here, we are inserting a literal tab character inside the quotes, as the text implies.)*

#### 👨‍💻 Code Explanation

This is a two-stage `sed` pipeline:

1.  **`sed "s/^M//"`**: The first `sed` command reads the file. To type the `^M`, you must press **`Ctrl+V`**, **`Ctrl+M`** in your terminal. This command finds and removes all carriage return characters.
2.  **`| sed "s/	//"`**: The "cleaned" output is piped to a second `sed`. To type the tab character here, you must press **`Ctrl+V`**, then **`Tab`**. This command finds and removes all tab characters.


### 2\. Verifying the Removal

The text notes that if you redirect the output to a file, you can verify that the control characters are gone.

**1. Run the Command and Redirect**

```bash
# To type ^M: Ctrl+V, Ctrl+M
# To type the tab: Ctrl+V, Tab
cat controlChars.txt | sed "s/^M//" | sed "s/	//" > nocontrol1.txt
```

**2. Verify with `cat -t`**
The `cat -t` command (or `-T` on some systems) is used to **t**est and display tabs as `^I` and other non-printing characters. If the file is clean, the output will have no `^I` or `^M` symbols.

#### 📜 Code Snippet

```bash
cat -t nocontrol1.txt
```

#### 🖥️ Output

```
1 carriage return
2 carriage return
1 tab character
```

The file is now clean of all control characters.

---


# 📊 Counting Words in a Dataset

Listing 6.6 displays the content of `WordCountInFile.sh`, which illustrates how to combine various bash commands to count the words (and their occurrences) in a file.

## 📜 Listing 6.6: `wordcountinfile.sh`

```bash
#!/bin/bash
# How this code sample works:
# The file is fed to the "tr" command,
# the "tr" command changes uppercase to lowercase
# sed removes commas and periods, then changes whitespace to newlines
# uniq needs each word on its own line to count the words properly
# uniq converts data to unique words
# uniq also displays the number of times each word appeared
# The final sort orders the data by the wordcount.
cat "$1" | xargs -n1 | tr A-Z a-z | \
sed -e 's/\.//g' -e 's/\,//g' -e 's/ /\ /g' | \
sort | uniq -c | sort -nr
```

## 🚀 How to Create and Run This Script

To make this "complete," you need two files: the script itself and a sample text file to analyze.

**1. Create a Sample Text File (`testfile.txt`)**

First, let's create a file to test our word counter.

```bash
nano testfile.txt
```

Paste the following text into `nano`, then save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`):

```
Hello world.
This is a test.
Hello again, world!
Test, test, test.
```

**2. Create the Script File (`wordcountinfile.sh`)**

Next, create the script file.

```bash
nano wordcountinfile.sh
```

Paste the code from **Listing 6.6** into the `nano` editor. Save and exit.

**3. Make the Script Executable**

You must give the script permission to run.

```bash
chmod +x wordcountinfile.sh
```

**4. Run the Script**

Now, run the script and provide `testfile.txt` as the first argument (`$1`).

```bash
./wordcountinfile.sh testfile.txt
```

### 🖥️ Example Output

The script will process `testfile.txt` and give you a sorted list of word counts, from most frequent to least frequent.

```bash
      3 test
      2 world
      2 hello
      1 this
      1 is
      1 again
      1 a
```

### 👨‍💻 Detailed Explanation

The comments in the script provide a good overview. Here is a more detailed, step-by-step breakdown of the pipeline:

1.  **`cat "$1"`**: Reads the content of the file provided as the first argument (`testfile.txt`) to the script.
2.  **`| xargs -n1`**: This is a way to split the text. `xargs -n1` takes all the input and outputs it one "word" (argument) per line.
3.  **`| tr A-Z a-z`**: **Tr**anslates all uppercase characters (A-Z) to their lowercase equivalents (a-z). This ensures that "Hello" and "hello" are counted as the same word.
4.  **`| sed ...`**: This stage runs multiple `sed` expressions:
      * `-e 's/\.//g'`: **S**ubstitutes (s) all literal periods (`.`) with nothing, **g**lobally (g).
      * `-e 's/\,//g'`: **S**ubstitutes all commas (`,`) with nothing, globally.
      * `-e 's/ /\ /g'`: This command, `s/ /\ /g`, attempts to replace a space with a space. It is redundant and has no effect, as `xargs -n1` already handled the spaces.
5.  **`| sort`**: Sorts the resulting list of words alphabetically. This is a **crucial** prerequisite for `uniq`.
6.  **`| uniq -c`**: `uniq` (unique) filters the sorted list.
      * `-c`: This flag tells `uniq` to **c**ount how many times each line (word) was repeated consecutively. It prefixes each unique word with its count.
7.  **`| sort -nr`**: This final sort organizes the counted output.
      * `-n`: Sorts **n**umerically (based on the count).
      * `-r`: **R**everses the sort, putting the highest numbers (most common words) at the top.


## 🧠 Back References in `sed`

In the section describing `grep`, you learned about backreferences. Similar functionality is available with the `sed` command. The main difference is that the backreferences can also be used in the **replacement** section of the command.

A backreference "remembers" text captured inside escaped parentheses `\( ... \)`. The first captured group is `\1`, the second is `\2`, and so on.

*(These examples are "complete" and can be run directly in your terminal.)*

### 1\. Duplicating Characters

This `sed` command matches two consecutive "a" letters and replaces them with four "a"s.

#### 📜 Code Snippet

```bash
echo "aa" | sed -n "s#\([a-z]\)\1#\1\1\1\1#p"
```

#### 🖥️ Output

```
aaaa
```

#### 👨‍💻 Code Explanation

  * **`sed -n "..."`**: Uses `-n` (no default output) and `p` (print) to only show the result.
  * **`s#...#...#p`**: Uses `#` as a delimiter instead of `/`.
  * **Pattern: `\([a-z]\)\1`**
      * `\([a-z]\)`: This is **Group 1**. It matches any single lowercase letter. In this case, it matches the first `a`.
      * `\1`: This is a backreference to **Group 1**. It matches the text *captured* by the first group. So, it matches the second `a`.
  * **Replacement: `\1\1\1\1`**
      * Replaces the matched text ("aa") with the content of Group 1 ("a") four times.

### 2\. Replacing with a Captured Group

This command replaces all duplicate pairs of letters with the letters "aa".

#### 📜 Code Snippet

```bash
echo "aa/bb/cc" | sed -n "s#\(aa\)/\(bb\)/\(cc\)#\1/\1/\1/#p"
```

#### 🖥️ Output

```
aa/aa/aa/
```

#### 👨‍💻 Code Explanation

  * **Pattern: `\(aa\)/\(bb\)/\(cc\)`**
      * `\(aa\)`: Captures "aa" as **Group 1** (`\1`).
      * `\(bb\)`: Captures "bb" as **Group 2** (`\2`).
      * `\(cc\)`: Captures "cc" as **Group 3** (`\3`).
  * **Replacement: `\1/\1/\1/`**
      * Replaces the entire matched string with the content of **Group 1** three times, separated by slashes.

### 3\. Inserting a Comma in a Number

This command inserts a comma in a four-digit number.

#### 📜 Code Snippet

```bash
echo "1234" | sed -n "s@\([0-9]\)\([0-9]\)\([0-9]\)\([0-9]\)@\1,\2\3\4@p"
```

#### 🖥️ Output

```
1,234
```

#### 👨‍💻 Code Explanation

  * **`s@...@...@p`**: Uses `@` as a delimiter.
  * **Pattern: `\([0-9]\)\([0-9]\)\([0-9]\)\([0-9]\)`**
      * It runs the `[0-9]` (one single digit) command four times, capturing each digit into its own group (`\1`, `\2`, `\3`, `\4`).
  * **Replacement: `\1,\2\3\4`**
      * It rebuilds the string using the captured digits, inserting a literal comma (`,`) after the first one.

### 4\. A More General Comma Insertion

This `sed` expression can insert a comma in a five-digit number.

#### 📜 Code Snippet

```bash
echo "12345" | sed 's/\([0-9]\{3\}\)$/,\1/g;s/^,//'
```

#### 🖥️ Output

```
12,345
```

#### 👨‍💻 Code Explanation

This is a more advanced, two-part command separated by a semicolon (`;`).

1.  **`s/\([0-9]\{3\}\)$/,\1/g`**: This part finds the last 3 digits.
      * `\([0-9]\{3\}\)`: Matches and **captures (`\1`)** a sequence of **exactly 3 digits**.
      * `$`: **Anchors** this match to the **end of the line**.
      * `/,\1/g`: Replaces that 3-digit group with a comma *before* it.
      * After this command, "12345" becomes "12,345".
2.  **`s/^,//`**: This is a cleanup command. If the number was just "345", the first command would have turned it into ",345". This second command, `s/^,//`, searches for a comma at the **start of the line (`^`)** and removes it.


##  Displaying Only "Pure" Words in a Dataset

In the previous section, we solved this task using `egrep`. This section shows you how to solve the same task using `sed`. For simplicity, let’s work with a text string. The approach is similar to the word-counting script.

*(These examples are also "complete" and can be run directly in your terminal.)*

#### 📜 Setup

```bash
x="ghi abc Ghi 123 #def5 123z"
```

### Step 1: Split into Words

First, split the variable `x` into one word per line by replacing spaces with newlines (`\n`).

```bash
echo $x | tr -s ' ' '\n'
```

#### 🖥️ Output

```
ghi
abc
Ghi
123
#def5
123z
```

### Step 2: Filter for "Pure" Alphabetic Words

Second, we invoke `sed` to filter this list.

#### 📜 Code Snippet

```bash
echo $x | tr -s ' ' '\n' | sed -nE "/(^[a-zA-Z]+$)/p"
```

#### 🖥️ Output

```
ghi
abc
Ghi
```

#### 👨‍💻 Code Explanation

  * **`| sed -nE "..."`**:
      * **`-n`**: Suppresses default printing. We only want to print matches.
      * **`-E`**: Enables **E**xtended **R**egular **E**xpressions. This allows us to use `+` (one or more) without escaping it.
      * **`"/(^[a-zA-Z]+$)/p"`**: This is a "print" command with a regex address.
          * `^`: Matches the **start** of the line.
          * `[a-zA-Z]+`: Matches **one or more** alphabetic characters.
          * `$`: Matches the **end** of the line.
          * This pattern *only* matches lines that contain *nothing but* letters.
          * `p`: **P**rints the lines that match this pattern.

### Step 3: Sort and Uniq

If you also want to sort the output and print only the unique words, pipe the result to `sort` and `uniq`.

#### 📜 Code Snippet

```bash
echo $x | tr -s ' ' '\n' | sed -nE "/(^[a-zA-Z]+$)/p" | sort | uniq
```

#### 🖥️ Output

```
Ghi
abc
ghi
```

### Step 4: Extract Only Integers

If you want to extract only the integers, you use the same logic but change the regex to match digits.

#### 📜 Code Snippet

```bash
echo $x | tr -s ' ' '\n' | sed -nE "/(^[0-9]+$)/p" | sort | uniq
```

#### 🖥️ Output

```
123
```

### Step 5: Extract Alphanumeric Words

If you want to extract words with letters *and* numbers, you expand the character class.

#### 📜 Code Snippet

```bash
echo $x | tr -s ' ' '\n' | sed -nE "/(^[0-9a-zA-Z]+$)/p" | sort | uniq
```

#### 🖥️ Output

```
123
123z
Ghi
abc
ghi
```

Now you can replace `echo $x` with a dataset (like `cat file.txt`) to retrieve only alphabetic strings from that dataset.


## ⚡ One-Line `sed` Commands

This section shows useful problems you can solve with a single line of `sed` and exposes you to more switches and arguments.

### 📁 Listing 6.7: `data4.txt` (Setup)

To run these "complete examples," you must first create the file `data4.txt`.

**1. Create the File**
(Note: The file has leading spaces, which are important for the examples).

```bash
nano data4.txt
```

**2. Paste the Content**
Paste the following text into `nano`, then save and exit:

```
 hello world4
 hello world5 two
 hello world6 three
 hello world4 four
line five
line six
line seven
```

**3. Run the Commands**
You can now run each of the following commands in your terminal to test them.


#### Print the first line

```bash
sed q < data4.txt
```

  * **`q`**: The **q**uit command. `sed` prints the first line (default behavior) and then immediately quits.
  * **Output:** `  hello world4 `


#### Print the first three lines

```bash
sed 3q < data4.txt
```

  * **`3q`**: An address (`3`) combined with the **q**uit command. `sed` prints lines 1 and 2. On line 3, it prints, then quits.
  * **Output:**
    ```
     hello world4
     hello world5 two
     hello world6 three
    ```


#### Print the last line

```bash
sed '$!d' < data4.txt
```

  * **`$!d`**:
      * `$!`: An address that means "any line that is **not** (`!`) the **last line** (`$`)".
      * `d`: The **d**elete command.
  * This command deletes every line *except* the last one, which is then printed by default.
  * **Output:** `line seven`


#### Print the last line (alternate)

```bash
sed -n '$p' < data4.txt
```

  * **`-n`**: Suppresses default output.
  * **`$p`**: An address (`$`) for the **last line**, and the **p**rint command.
  * **Output:** `line seven`


#### Print the last two lines

```bash
sed '$!N;$!D' < data4.txt
```

  * This is a complex command that creates a "sliding window" of text.
  * **`$!N`**: For every line *except* the last one (`$!`), append the **N**ext line into the pattern space.
  * **`$!D`**: For every line *except* the last one (`$!`), **D**elete the *first line* from the pattern space and restart the loop (without reading a new line).
  * **Result:** The script continuously deletes the top line and adds a new line to the bottom until it hits the last two lines. The `D` command isn't run, and the two lines in the pattern space are printed at the end.
  * **Output:**
    ```
    line six
    line seven
    ```


#### Print lines that do not contain "world"

```bash
sed '/world/d' < data4.txt
```

  * **`/world/d`**: An address (`/world/`) and a command (`d`). This finds any line containing "world" and **d**eletes it. All other lines are printed by default.
  * **Output:**
    ```
    line five
    line six
    line seven
    ```


#### Print duplicates of lines that do contain "world"

```bash
sed '/world/p' < data4.txt
```

  * **No `-n`**: `sed` prints every line by default.
  * **`/world/p`**: An address (`/world/`) and a command (`p`). If a line contains "world", it is **p**rinted a *second time*.
  * **Output:**
    ```
     hello world4
     hello world4
     hello world5 two
     hello world5 two
     hello world6 three
     hello world6 three
     hello world4 four
     hello world4 four
    line five
    line six
    line seven
    ```


#### Print only the fifth line

```bash
sed -n '5p' < data4.txt
```

  * **`-n`**: Suppresses default output.
  * **`5p`**: An address (`5`) for line 5, and the **p**rint command.
  * **Output:** `line five`


#### Print all lines, duplicating line five

```bash
sed '5p' < data4.txt
```

  * **No `-n`**: Prints every line by default.
  * **`5p`**: On line 5, **p**rint it a second time.
  * **Output:**
    ```
     hello world4
     hello world5 two
     hello world6 three
     hello world4 four
    line five
    line five
    line six
    line seven
    ```


#### Print lines 4 through 6

```bash
sed -n '4,6p' < data4.txt
```

  * **`-n`**: Suppresses default output.
  * **`4,6p`**: A numeric range (`4,6`) and the **p**rint command.
  * **Output:**
    ```
     hello world4 four
    line five
    line six
    ```


#### Delete lines 4 through 6

```bash
sed '4,6d' < data4.txt
```

  * **`4,6d`**: A numeric range (`4,6`) and the **d**elete command. These lines are deleted; all others are printed.
  * **Output:**
    ```
     hello world4
     hello world5 two
     hello world6 three
    line seven
    ```


#### Delete a range of lines by pattern

```bash
sed '/world6/,/six/d' < data4.txt
```

  * **`/world6/,/six/d`**: A regex range address. Deletes all lines starting from the one matching "world6" (line 3) through the one matching "six" (line 6).
  * **Output:**
    ```
     hello world4
     hello world5 two
    line seven
    ```


#### Print a range of lines by pattern

```bash
sed -n '/world6/,/six/p' < data4.txt
```

  * **`-n`**: Suppresses default output.
  * **`/world6/,/six/p`**: Prints *only* the lines in the range from "world6" to "six".
  * **Output:**
    ```
     hello world6 three
     hello world4 four
    line five
    line six
    ```


#### Duplicate a range of lines by pattern

```bash
sed '/world6/,/six/p' < data4.txt
```

  * **No `-n`**: Prints all lines by default.
  * **`/world6/,/six/p`**: **P**rints a *second* copy of all lines in the range.
  * **Output:**
    ```
     hello world4
     hello world5 two
     hello world6 three
     hello world6 three
     hello world4 four
     hello world4 four
    line five
    line five
    line six
    line six
    line seven
    ```


#### Delete the even-numbered lines

```bash
sed 'n;d;' < data4.txt
```

  * **`n;d;`**: A two-command script.
      * **`n`**: Prints the current pattern space (line 1), then fetches the **n**ext line (line 2).
      * **`d`**: **D**eletes the pattern space (which now holds line 2).
  * This cycle repeats, printing odd lines and deleting even lines.
  * **Output:**
    ```
     hello world4
     hello world6 three
    line five
    line seven
    ```


#### Replace a range of letters

```bash
sed "s/[a-m]/,/g" < data4.txt
```

  * **`s/[a-m]/,/g`**: **S**ubstitutes any letter in the range `a` through `m` with a comma (`,`), **g**lobally.
  * **Output:**
    ```
    ,,o wor,,4
    ,,o wor,,5 two
    ,,o wor,,6 t,r,,
    ,,o wor,,4 ,our
    ,,n, ,,v,
    ,,n, s,x
    ,,n, s,v,n
    ```


#### Replace a range of letters with multiple characters

```bash
sed "s/[a-m]/,@#/g" < data4.txt
```

  * **`s/[a-m]/,@#/g`**: Same as above, but replaces with the literal string `,@#`.
  * **Output:**
    ```
    ,@#,@#,@#,@#o wor,@#,@#4
    ,@#,@#,@#,@#o wor,@#,@#5 two
    ,@#,@#,@#,@#o wor,@#,@#6 t,@#r,@#,@#
    ,@#,@#,@#,@#o wor,@#,@#4 ,@#our
    ,@#,@#n,@# ,@#,@#v,@#
    ,@#,@#n,@# s,@#x
    ,@#,@#n,@# s,@#v,@#n
    ```


#### Delete tab characters

  * **Note:** The `sed` command does not recognize escape sequences such as `\t`. You must insert a **literal tab**.
  * **How to type:** In the bash shell, press **`Ctrl+V`**, then press the **`<TAB>`** key.

<!-- end list -->

```bash
sed 's/	//g' < data4.txt
```

*(Where `	` is a literal tab you inserted with `Ctrl+V`, `<TAB>`)*


#### Delete tab characters and blank spaces

*(Note: The text's command `sed 's/ //g'` only deletes spaces. To delete *both* tabs and spaces, you must include both in a character class `[ ]`)*.

**Command to delete spaces (as written in text):**

```bash
sed 's/ //g' < data4.txt
```

  * **Output:**
    ```
    helloworld4
    helloworld5two
    helloworld6three
    helloworld4four
    linefive
    linesix
    lineseven
    ```

**Command to delete spaces AND tabs:**

```bash
sed 's/[ 	]//g' < data4.txt
```

*(Where `	` is a literal tab inserted with `Ctrl+V`, `<TAB>`)*


#### Replace every line with "pasta"

```bash
sed 's/.*/pasta/' < data4.txt
```

  * **`s/.*/pasta/`**:
      * `.*`: A regex that matches "any character (`.`), zero or more times (`*`)". This matches the entire line.
      * `/pasta/`: Replaces the entire line with "pasta".
  * **Output:**
    ```
    pasta
    pasta
    pasta
    pasta
    pasta
    pasta
    pasta
    ```


#### Insert blank lines

```bash
sed '3G;3G;5G' < data4.txt
```

  * **`G`**: Appends a newline, followed by the contents of the "hold space" (which is empty by default), to the current pattern space.
  * **`3G;3G;`**: On line 3, do this *twice*, adding two blank lines.
  * **`5G`**: On line 5, do this *once*, adding one blank line.
  * **Output:**
    ```
     hello world4
     hello world5 two
     hello world6 three


     hello world4 four
    line five

    line six
    line seven
    ```


#### Insert a blank line after every line (double-space)

```bash
sed G < data4.txt
```

  * **`G`**: No address is given, so the `G` command runs on *every* line.
  * **Output:**
    ```
     hello world4

     hello world5 two

     hello world6 three

     hello world4 four

    line five

    line six

    line seven

    ```


#### Insert a blank line after every other line

```bash
sed 'n;G' < data4.txt
```

  * **`n;G`**:
      * `n`: Print the current pattern space (line 1), then fetch the **n**ext line (line 2).
      * `G`: Append a newline to the *new* pattern space (line 2).
  * This cycle repeats, adding a blank line after every *even-numbered* line.
  * **Output:**
    ```
     hello world4
     hello world5 two

     hello world6 three
     hello world4 four

    line five
    line six

    line seven
    ```


#### Reverse the lines in the file

```bash
sed '1!G;h;$!d' < data4.txt
```

  * This is the most famous `sed` one-liner. It works by building up the reversed file in the hold space.
  * **`1!G`**: If it's **not** (`!`) line 1 (`1`), **G**et (append) the hold space to the pattern space.
  * **`h`**: **H**old. Copy the (possibly new) pattern space over the hold space.
  * **`$!d`**: If it's **not** (`!`) the last line (`$`), **d**elete the pattern space.
  * **Trace:**
      * Line 1: `1!G` (skip). `h` (HS="L1"). `$!d` (delete).
      * Line 2: `1!G` (run). PS="L2\\nL1". `h` (HS="L2\\nL1"). `$!d` (delete).
      * ...
      * Last Line (7): `1!G` (run). PS="L7\\nL6...L1". `h` (HS="L7...L1"). `$!d` (skip).
      * End of file: `sed` prints the final pattern space.
  * **Output:**
    ```
    line seven
    line six
    line five
     hello world4 four
     hello world6 three
     hello world5 two
     hello world4
    ```


---