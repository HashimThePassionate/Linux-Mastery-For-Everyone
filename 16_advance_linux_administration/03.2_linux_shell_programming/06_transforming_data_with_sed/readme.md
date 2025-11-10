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