# 📘 **Text File Manipulation and Bash Utilities**
This section covers essential bash commands for working with text files — from editing and searching, to sorting, compressing, and automating tasks with shell scripts. You’ll also learn how to use the pipe (|) command to redirect output from one command as input to another.


# 🤝 The `join` Command

The `join` command allows you to merge two files in a meaningful fashion, which essentially creates a simple version of a relational database.

The `join` command operates on two files. It pastes together only those lines that have a common **tagged field** (the "join key"), which is usually a numerical label. The command writes the merged result to standard output.

**Crucial Rule:** The files to be joined **must be sorted** according to the tagged field for the matchups to work properly. By default, `join` uses the first field of each line as the key.


## 🚀 Practical Example from Scratch

Let's use the files from the text to demonstrate.

### Listing 3.1: `1.data` (Product IDs and Names)

First, create a file that maps product IDs to their names.

```bash
hashim@Hashim:~$ nano 1.data
# Inside nano, add these lines:
100 Shoes
200 Laces
300 Socks
```

### Listing 3.2: `2.data` (Product IDs and Prices)

Second, create a file that maps product IDs to their prices.

```bash
hashim@Hashim:~$ nano 2.data
# Inside nano, add these lines:
100 $40.00
200 $1.00
300 $2.00
```

### Run the `join` Command

Now, launch the following command from the command line:

```bash
hashim@Hashim:~$ join 1.data 2.data
```

The preceding code snippet generates the following output:

```
100 Shoes $40.00
200 Laces $1.00
300 Socks $2.00
```

### 📜 Code Explanation

  * `join 1.data 2.data`: This command compares `1.data` and `2.data`.
  * The `join` command automatically looks at the **first field** of each file.
  * It finds "100" in `1.data` and "100" in `2.data`. It merges these lines, printing the common field (`100`) followed by the rest of the line from file 1 (`Shoes`) and the rest of the line from file 2 (`$40.00`).
  * It repeats this process for the common keys "200" and "300".
  * This only worked because both `1.data` and `2.data` were already **sorted** by their first field.


# 📰 The `fold` Command

The `fold` command enables you to display a set of lines with a fixed column width. This section contains a few more examples.

Note that this command **does not** take into account spaces between words; it simply breaks the line at the specified column. The output is displayed in columns that resemble a "newspaper" style (where words can be split).

### 🚀 Practical Example 1

The following command displays a set of lines with ten characters in each line:

```bash
hashim@Hashim:~$ x="aa bb cc d e f g h i j kk ll mm nn"
hashim@Hashim:~$ echo $x | fold -10
```

The output of the preceding code snippet is here:

```
aa bb cc d
 e f g h i
 j kk ll m
m nn
```

#### 📜 Code Explanation

  * `x=...`: A long string is assigned to the variable `x`.
  * `echo $x`: This prints the string to standard output.
  * `| fold -10`: The output of `echo` is piped into the `fold` command. The `-10` flag instructs `fold` to break the text at a maximum width of **10 characters** and wrap it to the next line. Notice it broke the word "mm" in the middle.

### 🚀 Practical Example 2

As another example, consider the following code snippet:

```bash
hashim@Hashim:~$ x="The quick brown fox jumps over the fat lazy dog."
hashim@Hashim:~$ echo $x | fold -10
```

The output of the preceding code snippet is here:

```
The quick
brown fox
jumps over
 the fat l
azy dog.
```

#### 📜 Code Explanation

This example clearly shows the "newspaper" style break. The line `the fat l` is 10 characters long, and the rest of the word, `azy dog.`, is wrapped to the next line.


# 🔪 The `split` Command

The `split` command is useful when you want to create a set of sub-files from a single large file.

By default, the sub-files are named using a standard pattern: `xaa`, `xab`, `xac`, ..., `xaz`, `xba`, `xbb`, ..., `xbz`, ..., `xza`, `xzb`, ..., and `xzz`. This naming scheme allows the `split` command to create a maximum of 676 files (= 26 \* 26).

The default size for each of these files is **1,000 lines**.

### 🚀 Practical Example from Scratch

Let's create a large file and split it.

1.  **Create a large file** (e.g., 1,500 lines):

    ```bash
    hashim@Hashim:~$ seq 1 1500 > big_file.txt
    ```

      * `seq 1 1500`: Generates a sequence of numbers from 1 to 1500.
      * `> big_file.txt`: Saves that sequence into a file.

2.  **Split the file using a specific line count:**
    The following snippet illustrates how to invoke the `split` command to split the file `big_file.txt` into files with 500 lines each:

    ```bash
    hashim@Hashim:~$ split -l 500 big_file.txt
    ```

      * `-l 500`: This flag specifies a limit of **500 lines** per file.

3.  **Check the output files:**

    ```bash
    hashim@Hashim:~$ ls
    big_file.txt  xaa  xab  xac
    ```

      * **Result**: The command created three files (`xaa`, `xab`, `xac`) because 1500 lines / 500 lines per file = 3 files.

4.  **Split the file with a custom prefix:**
    You can also specify a file prefix for the created files, as shown here:

    ```bash
    hashim@Hashim:~$ split -l 500 big_file.txt shorter_
    ```

      * `shorter_`: This is the prefix to be used for the new filenames.

5.  **Check the new output files:**

    ```bash
    hashim@Hashim:~$ ls
    big_file.txt  shorter_aa  shorter_ab  shorter_ac  xaa  xab  xac
    ```

      * **Result**: The preceding command creates the new files: `shorter_aa`, `shorter_ab`, and `shorter_ac`.


# 🔀 The `sort` Command

The `sort` command sorts the lines in a text file. By default, it arranges lines of text **alphabetically**.

### 🚀 Practical Example 1: Alphabetical Sort

1.  **Create a test file `test2.txt`:**

    ```bash
    hashim@Hashim:~$ nano test2.txt
    # Inside nano, add these lines:
    cc
    aa
    bb
    ```

2.  **Sort the file:**
    Both of the following commands will sort the lines in `test2.txt`:

    ```bash
    hashim@Hashim:~$ cat test2.txt | sort
    hashim@Hashim:~$ sort test2.txt
    ```

    *(Note: `sort test2.txt` is more efficient as it uses only one process.)*

    The output of the preceding commands is here:

    ```
    aa
    bb
    cc
    ```


### ⚙️ `sort` Command Options

Some common options for the `sort` command are here:

  * **`-n`**: Sort **n**umerically. (Without this, `10` will sort *before* `2` because it starts with a "1".)
  * **`-r`**: **R**everse the order of the sort (e.g., Z to A, or 10 to 1).
  * **`-f`**: **F**old case, meaning sort uppercase and lowercase together (e.g., `a` and `A` are treated as the same).
  * **`-k X`**: Sort on field **k**ey `X` (where `X` is the field number).
  * **`+x`**: An older, now-deprecated syntax to ignore the first `x` fields when sorting. You should use `-k` instead.

### 🚀 Practical Example 2: Numeric Sort

1.  **Create a file with numbers:**

    ```bash
    hashim@Hashim:~$ echo -e "10\n2\n1" > numbers.txt
    ```

2.  **Run a default (alphabetical) sort:**

    ```bash
    hashim@Hashim:~$ sort numbers.txt
    1
    10
    2
    ```

      * **Explanation:** This is incorrect. "10" comes before "2" alphabetically.

3.  **Run a numeric sort (`-n`):**

    ```bash
    hashim@Hashim:~$ sort -n numbers.txt
    1
    2
    10
    ```

      * **Explanation:** This is the correct numeric order.


### 🚀 Practical Example 3: Sorting Files by Size (`-k`)

You can use the `sort` command to display the files in a directory based on their file size. The file size is the **5th field** in the `ls -l` output.

```bash
hashim@Hashim:~$ ls -l | sort -k 5n
```

  * **`ls -l`**: Produces the long listing of files.
  * **`|`**: Pipes that output to the `sort` command.
  * **`sort -k 5n`**:
      * `-k 5`: Tells `sort` to look at the **5th field** (key).
      * `n`: Tells `sort` to treat that field as a **n**umber.

The output will be a file listing sorted by the fifth column (file size), from smallest to largest.

```
total 72
-rw-r--r-- 1 ocampesato staff 12 Apr 06 20:46 output.txt
-rw-r--r-- 1 ocampesato staff 14 Apr 06 20:46 outfile.txt
-rw- 1 ocampesato staff 25 Apr 06 20:46 apple-care.txt
...
-rw- 1 ocampesato staff 417 Apr 06 20:46 iphonemeetup.txt
```

You can sort the files in a directory and display them from largest to smallest with this command by adding the `-r` (reverse) flag:

```bash
ls -l | sort -k 5nr
```

*(Note: The text's example `ls -l | sort -n` is ambiguous and may not work correctly, as it tries to sort the entire line numerically, which starts with non-numeric permission strings.)*


### 🚀 Practical Example 4: Sorting File Contents

You can also combine `sort` with other commands in a pipeline. Suppose the file `abc2.txt` contains duplicates:

```
This is line one
This is line two
This is line one
This is line three
Fourth line
Fifth line
The sixth line
The seventh line
```

An example of combining the commands `sort` and `tail` is shown here:

```bash
hashim@Hashim:~$ cat abc2.txt | sort | tail -5
```

#### 📜 Code Explanation

1.  `cat abc2.txt`: Reads the file.
2.  `| sort`: Pipes the content to `sort`, which sorts it alphabetically.
3.  `| tail -5`: Pipes the *sorted list* to `tail`, which outputs only the final 5 lines of that list.

The preceding command sorts the contents and then displays the final five lines:

```
The seventh line
The sixth line
This is line one
This is line one
This is line three
This is line two
```

As you can see, the output contains two duplicate lines. The next section shows you how to use the `uniq` command to remove duplicate lines.


# ☝️ The `uniq` Command

The `uniq` command prints only the unique lines in a text file.

**Crucial Rule:** `uniq` **only removes *consecutive* duplicate lines**. For this reason, you must almost always use `sort` *before* you use `uniq`.

### 🚀 Practical Example from Scratch

1.  **Create a file `test3.txt` with non-consecutive duplicates:**

    ```bash
    hashim@Hashim:~$ echo -e "abc\ndef\nabc\nabc" > test3.txt
    hashim@Hashim:~$ cat test3.txt
    abc
    def
    abc
    abc
    ```

2.  **Run `uniq` *without* `sort` (The wrong way):**

    ```bash
    hashim@Hashim:~$ uniq test3.txt
    abc
    def
    abc
    ```

      * **Explanation:** This failed. It only removed the *second* "abc" because it was consecutive with the first "abc".

3.  **Run `sort` first to group duplicates:**

    ```bash
    hashim@Hashim:~$ cat test3.txt | sort
    abc
    abc
    abc
    def
    ```

4.  **Run `sort` and *then* `uniq` (The correct way):**
    The following command sorts the contents of `test3.txt` and then displays the unique lines:

    ```bash
    hashim@Hashim:~$ cat test3.txt | sort | uniq
    ```

    The output of the preceding code snippet is here:

    ```
    abc
    def
    ```


# 🎎 How to Compare Files (`diff` and `cmp`)

  * **`diff`**: Enables you to compare two text files line by line.
  * **`cmp`**: Compares two files (text or binary) byte by byte and finds the *first* difference.

### 🚀 Practical Example: `diff`

1.  **Create two different files:**

    ```bash
    hashim@Hashim:~$ echo -e "Hello\nWorld" > output.txt
    hashim@Hashim:~$ echo -e "goodbye\nworld" > outfile.txt
    ```

      * Note that `World` and `world` are different (case-sensitive).

2.  **Run the `diff` command:**

    ```bash
    hashim@Hashim:~$ diff output.txt outfile.txt
    ```

    The output is shown here:

    ```
    1,2c1,2
    < Hello
    < World
    ---
    > goodbye
    > world
    ```

#### 📜 Code Explanation

  * `1,2c1,2`: This is `diff`'s notation. It means: "Lines 1 and 2 in the first file (`1,2`) need to be **c**hanged (`c`) to become lines 1 and 2 in the second file (`1,2`)."
  * `< Hello` / `< World`: The `<` symbol indicates lines that are from the first file (`output.txt`).
  * `---`: This is a separator.
  * `> goodbye` / `> world`: The `>` symbol indicates lines that are from the second file (`outfile.txt`).


# 🧐 The `od` (Octal Dump) Command

The `od` command displays an "octal dump" (or other representations) of a file. This is very helpful when you want to see embedded **control characters** (like tabs or newlines) that are not normally visible on the screen.

### 🚀 Practical Example 1: Finding Tabs

1.  **Create a file with tabs.** (Use `printf` to reliably insert `\t` tab characters.)

    ```bash
    hashim@Hashim:~$ printf "a\tb\tc\n" > tabs.txt
    ```

      * On screen, `cat tabs.txt` just looks like: `a   b   c`

2.  **Use `od` to see the *real* characters:**
    The following command displays the tab and newline characters:

    ```bash
    hashim@Hashim:~$ cat tabs.txt | od -tc
    ```

    The command generates the following output:

    ```
    0000000   a  \t   b  \t   c  \n
    0000006
    ```

      * **📜 Code Explanation:**
          * `od -tc`: The `od` command.
              * `-t c`: Specifies the output **t**ype as `c` (printable characters or backslash escapes).
          * `0000000`: This is the byte offset (the file position).
          * `a \t b \t c \n`: This clearly shows the tab (`\t`) and newline (`\n`) characters.
          * `0000006`: This is the total length of the file in bytes.

3.  **Alternative way to see tabs (`cat -t`):**

    ```bash
    hashim@Hashim:~$ cat -t tabs.txt
    a^Ib^Ic
    ```

      * **📜 Code Explanation:** The `-t` flag for `cat` specifically visualizes tabs as `^I`.

### 🚀 Practical Example 2: `echo` vs. `printf`

This proves that `echo` adds a newline and `printf` does not.

1.  **Test `echo`:**

    ```bash
    hashim@Hashim:~$ echo abcde | od -c
    0000000   a   b   c   d   e  \n
    0000006
    ```

      * **Explanation:** As you can see, `od` shows that `echo` added a newline (`\n`) at the end.

2.  **Test `printf`:**

    ```bash
    hashim@Hashim:~$ printf abcde | od -c
    0000000   a   b   c   d   e
    0000005
    ```

      * **Explanation:** `printf` printed *only* the characters specified, with no trailing newline.


# 🔄 The `tr` (Translate) Command

The `tr` command is a versatile command for translating or deleting characters. It supports some useful options for:

  * Removing extraneous whitespaces.
  * Inserting blank lines.
  * Printing words on separate lines.
  * Translating characters from one set to another (e.g., uppercase to lowercase).

### 🚀 Practical Example 1: Change Case

The following command capitalizes every letter in the variable `x`:

```bash
hashim@Hashim:~$ x="abc def ghi"
hashim@Hashim:~$ echo $x | tr [a-z] [A-Z]
ABC DEF GHI
```

  * **📜 Code Explanation:** `tr` reads the string from `echo`. It translates the first set (`[a-z]`) to the second set (`[A-Z]`), character by character (a-\>A, b-\>B, c-\>C, etc.).

Here is another way using POSIX character classes, which are more robust:

```bash
hashim@Hashim:~$ echo $x | tr '[:lower:]' '[:upper:]'
ABC DEF GHI
```

> **POSIX Character Classes:**
>
>   * `[:alnum:]`: Alphanumeric characters
>   * `[:alpha:]`: Alphabetic characters
>   * `[:digit:]`: Numeric characters
>   * `[:lower:]`: Lowercase alphabetic characters
>   * `[:upper:]`: Uppercase characters
>   * `[:space:]`: Whitespace characters
>   * `[:punct:]`: Punctuation characters

### 🚀 Practical Example 2: Delete Characters (`-d`)

The following example removes whitespace characters (`" "`) from the variable `x`:

```bash
hashim@Hashim:~$ x="abc def ghi"
hashim@Hashim:~$ echo $x | tr -d " "
abcdefghi
```

  * **📜 Code Explanation:** The `-d` flag tells `tr` to **d**elete any character found in the set (`" "`).

### 🚀 Practical Example 3: Replace Characters (Newline)

You can replace one character with another.

  * **Replace spaces with newlines (one word per line):**

    ```bash
    hashim@Hashim:~$ echo "a b c" | tr -s " " "\n"
    a
    b
    c
    ```

      * **📜 Code Explanation:** Translates spaces (`" "`) to newlines (`\n`). The `-s` (squeeze) flag compresses multiple spaces into one *before* translating, so `a b` would still become "a", "b", "c".

  * **Replace commas with newlines:**

    ```bash
    hashim@Hashim:~$ echo "a,b,c" | tr -s "," "\n"
    a
    b
    c
    ```

  * **Replace newlines with spaces (join all lines):**

    ```bash
    # 1. Create a file with multiple lines
    hashim@Hashim:~$ echo -e "abc\ndef\nabc\nabc" > test4.txt

    # 2. Run the command
    hashim@Hashim:~$ cat test4.txt | tr '\n' ' '
    abc def abc abc 
    ```

      * **📜 Code Explanation:** This translates every newline (`\n`) into a space (`     `), effectively joining all lines into one.

### 🚀 Practical Example 4: Complex Pipeline

You can combine multiple commands using the bash "pipe" symbol.

1.  **Create the file `abc2.txt`:**

    ```bash
    hashim@Hashim:~$ echo -e "This is line one\nThis is line two\nThis is line one\nThis is line three\nFourth line\nFifth line\nThe sixth line\nThe seventh line" > abc2.txt
    ```

2.  **Run the pipeline:**
    This command sorts the contents, retrieves the "bottom" five lines, retrieves the unique lines from that set, and converts the text to uppercase.

    ```bash
    hashim@Hashim:~$ cat abc2.txt | sort | tail -5 | uniq | tr [a-z] [A-Z]
    ```

    Here is the output from the preceding command:

    ```
    THE SEVENTH LINE
    THE SIXTH LINE
    THIS IS LINE ONE
    THIS IS LINE THREE
    THIS IS LINE TWO
    ```

3.  **📜 Code Explanation (Step-by-Step):**

      * `cat abc2.txt`: Reads the file.
      * `| sort`: Sorts the lines alphabetically.
      * `| tail -5`: Takes only the last 5 lines from the *sorted* output.
      * `| uniq`: Removes any duplicate lines from that 5-line set (in this case, "This is line one" appears twice in the sorted list, so one is removed).
      * `| tr [a-z] [A-Z]`: Translates the final, unique lines to uppercase.

### 🚀 Practical Example 5: First Letter Uppercase

  * **Method 1 (Substring):**

    ```bash
    hashim@Hashim:~$ x="pizza"
    hashim@Hashim:~$ x=`echo ${x:0:1} | tr '[a-z]' '[A-Z]'`${x:1}
    hashim@Hashim:~$ echo $x
    Pizza
    ```

      * **📜 Code Explanation:**
          * `${x:0:1}`: This is **shell parameter expansion**. It takes a substring of `x`, starting at character `0` for a length of `1`. This gets "p".
          * `echo ... | tr ...`: The "p" is piped to `tr`, which converts it to "P".
          * `${x:1}`: This takes a substring of `x` starting from character `1` (the "i") to the end. This gets "izza".
          * `x=\`...\`...` : The command combines the result of the backticks ("P") with "izza" and re-assigns it to  `x\`.

  * **Method 2 (`cut`):**
    A slightly longer way to convert the first letter to uppercase:

    ```bash
    hashim@Hashim:~$ x="pizza"
    hashim@Hashim:~$ first=`echo $x | cut -c1 | tr [a-z] [A-Z]`
    hashim@Hashim:~$ second=`echo $x | cut -c2-`
    hashim@Hashim:~$ echo $first$second
    Pizza
    ```

      * **📜 Code Explanation:**
          * `first=\`...\``: This command cuts (`cut -c1` ) the first character ("p") and translates it to "P". The variable  `first\` becomes "P".
          * `second=\`...\``: This command cuts (`cut -c2-` ) from the 2nd character to the end. The variable  `second\` becomes "izza".
          * `echo $first$second`: This prints the two variables concatenated together.


## 🎯 A Simple Use Case: Fixing Windows Files (`^M`)

This shows how to use `tr` to replace the `^M` (Carriage Return) control character with a linefeed. These `^M` characters often appear in files transferred from Windows and can cause scripts to fail.

### Listing 3.4: `controlm.csv`

This file contains embedded `^M` characters (which represent `\r` or "carriage return").

```
IDN,TEST,WEEK_MINUS1,WEEK0,WEEK1,WEEK2,WEEK3,WEEK4,WE
EK10,WEEK12,WEEK14,WEEK15,WEEK17,WEEK18,WEEK19,WEEK21
^M1,BASO,,1.4,,0.8,,1.2,,1.1,,,2.2,,,1.4^M1,BASOAB,,0-
.05,,0.04,,0.05,,0.04,,,0.07,,,0.05^M...
```

# 📁 Creating the `controlm.csv` Sample File

It is difficult to type the `^M` character directly in `nano`. This character is a special control character called a "Carriage Return" (represented as `\r`).

The most reliable way to create this file is to use the `printf` command, which can interpret and print special characters like `\r`.

## 🚀 Step 1: Create the File Using `printf`

This single command will generate the file exactly as needed for your exercise.

```bash
#!/bin/bash
# We use printf because it processes \r as the actual carriage return character.
# This command creates one long string, with lines separated by \r instead of \n.

printf "IDN,TEST,WEEK_MINUS1,WEEK0,WEEK1,WEEK2,WEEK3,WEEK4,WEEK10,WEEK12,WEEK14,WEEK15,WEEK17,WEEK18,WEEK19,WEEK21\r1,BASO,,1.4,,0.8,,1.2,,1.1,,,2.2,,,1.4\r1,BASOAB,,0.05,,0.04,,0.05,,0.04,,,0.07,,,0.05" > controlm.csv
```

### ⚙️ Code Explanation

  * `printf "..."`: This command prints the long string.
  * `\r`: Every time `printf` sees this, it inserts the Carriage Return (`^M`) control character instead of a normal Newline (`\n`). This is what "breaks" the file for Unix-based systems.
  * `> controlm.csv`: This redirects the entire output from `printf` and saves it into the new file named `controlm.csv`.

## ✅ Step 2: Verify the "Broken" File

After creating the file, you **must** verify that the `^M` characters are present.

### ⚠️ How NOT to Check

If you use the normal `cat` command, the output will look very strange. The `\r` character tells the cursor to go back to the start of the line, so each line will print *over* the previous one.

```bash
hashim@Hashim:~$ cat controlm.csv
```

**Output (will look garbled):**

```
1,BASOAB,,0.05,,0.04,,0.05,,0.04,,,0.07,,,0.054,WEEK10,WEEK12,WEEK14,WEEK15,WEEK17,WEEK18,WEEK19,WEEK21
```

### ✅ How to Correctly See the `^M` Characters

To safely view the control characters, use `cat -v` (which visualizes non-printing characters) or `od` (as seen in your examples).

**Method A: `cat -v` (Recommended)**

This command displays the `^M` characters clearly.

```bash
hashim@Hashim:~$ cat -v controlm.csv
```

**Correct Output:**

```
IDN,TEST,WEEK_MINUS1,WEEK0,WEEK1,WEEK2,WEEK3,WEEK4,WEEK10,WEEK12,WEEK14,WEEK15,WEEK17,WEEK18,WEEK19,WEEK21^M1,BASO,,1.4,,0.8,,1.2,,1.1,,,2.2,,,1.4^M1,BASOAB,,0.05,,0.04,,0.05,,0.04,,,0.07,,,0.05
```

**Method B: `od -tc` (From your text)**

This command gives you an "octal dump" and shows the `\r` (Carriage Return) and `\n` (Newline) characters.

```bash
hashim@Hashim:~$ od -tc controlm.csv
```

**Correct Output (showing the `\r`):**

```
0000000   I   D   N   ,   T   E   S   T   ,   W   E   E   K   _   M   I
0000020   N   U   S   1   ,   W   E   E   K   0   ,   W   E   E   K   1
...
0000140   1   9   ,   W   E   E   K   2   1  \r   1   ,   B   A   S   O
0000160   ,   ,   1   .   4   ,   ,   0   .   8   ,   ,   1   .   2   ,
...
0000240   ,   1   .   4  \r   1   ,   B   A   S   O   A   B   ,   ,   0
0000260   .   0   5   ,   ,   0   .   0   4   ,   ,   0   .   0   5   ,
...
0000340   ,   0   .   0   5
0000345
```

### Listing 3.5: `controlm.sh`

This script illustrates how to remove the control characters.

```bash
hashim@Hashim:~$ nano controlm.sh
# Inside nano, add these lines:
#!/bin/bash
inputfile="controlm.csv"
removectrlmfile="removectrlmfile"

tr -s '\r' '\n' < $inputfile > $removectrlmfile
```

#### 📜 Code Explanation

  * `tr -s '\r' '\n'`: This is the core command.
      * `'\r'`: This is the **carriage return** character (what you see as `^M`).
      * `'\n'`: This is the **newline** character (the proper line ending for Unix/Linux).
      * `tr ... '\r' '\n'`: This translates every `\r` into a `\n`.
      * `-s`: The **squeeze** flag. It squeezes any repeated `\r` characters into one *before* translation, ensuring you don't get extra blank lines.
  * `< $inputfile`: Reads the *from* the `controlm.csv` file.
  * `> $removectrlmfile`: Writes the *clean* output to the `removectrlmfile`.

### Output

The output from launching the shell script in Listing 3.5 is here (the contents of `removectrlmfile`):

```
IDN,TEST,WEEK_MINUS1,WEEK0,WEEK1,WEEK2,WEEK3,WEEK4,WEEK10,W
EEK12,WEEK14,WEEK15,WEEK17,WEEK18,WEEK19,WEEK21
1,BASO,,1.4,,0.8,,1.2,,1.1,,,2.2,,,1.4
1,BASOAB,,0.05,,0.04,,0.05,,0.04,,,0.07,,,0.05
1,EOS,,6.1,,6.2,,7.5,,6.6,,,7.0,,,6.2
1,EOSAB,,0.22,,0.30,,0.27,,0.25,,,0.22,,,0.21
```

As you can see, the task is easily solved. The file is now properly formatted with newlines.

### Final `tr` Example: Changing the Delimiter

You can also replace the current delimiter (`,`) with a different one, like `|`.

```bash
hashim@Hashim:~$ cat removectrlmfile | tr -s ',' '|' > pipedfile
```

The resulting output is shown here:

```
IDN|TEST|WEEK_MINUS1|WEEK0|WEEK1|WEEK2|WEEK3|WEEK4|WEEK10|W
EEK12|WEEK14|WEEK15|WEEK17|WEEK18|WEEK19|WEEK21
1|BASO||1.4||0.8||1.2||1.1|||2.2|||1.4
1|BASOAB||0.05||0.04||0.05||0.04|||0.07|||0.05
...
```

#### 📜 Code Explanation

  * `tr -s ',' '|'`: This command translates (`tr`) every comma (`,`) into a pipe (`|`).
  * `-s`: The **squeeze** flag is important here. It squeezes any *repeated* commas (e.g., `,,`) into a *single* pipe (`|`). This is why `1,BASO,,1.4` becomes `1|BASO||1.4|...`.

---

# 🔎 The `find` Command

The `find` command is one of the most powerful and essential utilities in Bash. Its purpose is to search for files and directories within a specified directory hierarchy based on a wide array of criteria you provide.

The `find` command supports many options, including one for **printing** (displaying) the files it finds, and another for **executing** a command (like `rm`) on the files it finds.

In addition, you can specify logical operators such as **`-a` (AND)** as well as **`-o` (OR)** to create complex search queries. You can also specify switches to find files based on when they were **created**, **accessed**, or **modified**.


## 🚀 Practical Examples from Scratch

To test the `find` command, let's first create a sample directory structure.

```bash
# 1. Create a sandbox area
mkdir find_sandbox
cd find_sandbox

# 2. Create some files
touch report.txt data.log
touch old-file.txt

# 3. Create a subdirectory with more files
mkdir -p project/src
touch project/main.sh
touch project/README.md
touch project/src/utils.sh

# 4. Create a file with 'abc' in the name
touch project/abc_special.log
```

Now we have a test area. Let's run the `find` commands.

### Example 1: Find All Files (`-print`)

This command displays all files and directories starting from the current location (`.`).

```bash
hashim@Hashim:~/find_sandbox$ find . -print
.
./report.txt
./data.log
./old-file.txt
./project
./project/main.sh
./project/README.md
./project/src
./project/src/utils.sh
./project/abc_special.log
```

  * **📜 Code Explanation:**
      * `find`: The command.
      * `.`: This is the **path** to start searching from. `.` means "the current directory."
      * `-print`: This is the **action** to take. It explicitly prints the full path of every item found. (Note: On many systems, `-print` is the default action if no other action is specified).


### Example 2: Filtering with `grep`

You can pipe the full list of files from `find` to `grep` for simple filtering.

```bash
hashim@Hashim:~/find_sandbox$ find . -print | grep "abc"
./project/abc_special.log
```

  * **📜 Code Explanation:**
      * `find . -print`: This command finds *all* 10 items.
      * `| grep "abc"`: The full list is **piped** to `grep`, which then filters that list and only prints lines that contain the string "abc".

#### A Better Way: Using `find -name`

Piping to `grep` is inefficient. It's better to let `find` do the filtering itself using its own tests.

```bash
hashim@Hashim:~/find_sandbox$ find . -name "*abc*"
./project/abc_special.log
```

  * **📜 Code Explanation:**
      * `-name "*abc*"`: This is a **test** that tells `find` to only return items whose names match the glob pattern `*abc*` (an asterisk `*` means "any or zero characters"). This is much faster.


### Example 3: Finding by Suffix (`.sh`)

**Method 1 (Old Way):**

```bash
hashim@Hashim:~/find_sandbox$ find . -print | grep "sh$"
./project/main.sh
./project/src/utils.sh
```

  * **📜 Code Explanation:** This pipes the full list to `grep`, which uses the regular expression `sh$` to find lines that **end with** "sh".

**Method 2 (Better Way):**

```bash
hashim@Hashim:~/find_sandbox$ find . -name "*.sh"
./project/main.sh
./project/src/utils.sh
```

  * **📜 Code Explanation:** This uses `find`'s built-in `-name` test to find files matching the glob pattern `*.sh`.


### Example 4: Searching by Depth

You can limit how deep `find` will search into subdirectories.

```bash
hashim@Hashim:~/find_sandbox$ find . -maxdepth 2 -print
.
./report.txt
./data.log
./old-file.txt
./project
./project/main.sh
./project/README.md
./project/src
./project/abc_special.log
```

  * **📜 Code Explanation:**
      * `-maxdepth 2`: This **option** tells `find` to not go past a certain depth.
          * Depth 1: `.`
          * Depth 2: `report.txt`, `data.log`, `project`, `src`, etc.
      * The command found items *inside* `project` (like `main.sh`), but it did **not** go deeper to find `project/src/utils.sh`, because that would be at depth 3.


### Example 5: Finding by Time (`-mtime`) and Executing (`-exec`)

You can find files based on access, creation, or modification time (`atime`, `ctime`, `mtime`).

Let's find all files modified in the last 2 days (`-2`) and run the `wc -l` (word count - lines) command on each one.

```bash
hashim@Hashim:~/find_sandbox$ find . -mtime -2 -exec wc -l {} \;
       0 ./report.txt
       0 ./data.log
       0 ./old-file.txt
       0 ./project/main.sh
       0 ./project/README.md
       0 ./project/src/utils.sh
       0 ./project/abc_special.log
       0 .
       0 ./project
       0 ./project/src
```

  * **📜 Code Explanation:**
      * `-mtime -2`: This is a **test** for files with a **m**odification **time** of *less than* (`-`) 2 days (i.e., modified within the last 48 hours). Since we just created all these files, they all match.
      * `-exec wc -l {} \;`: This is the **action**.
          * `-exec`: This flag tells `find` to **exec**ute the following command for every file it finds.
          * `wc -l`: The command to run (count lines).
          * `{}`: This is a special placeholder. `find` replaces it with the full path of the file it just found (e.g., `./report.txt`).
          * `\;`: This marks the end of the `-exec` command. The backslash `\` is required to escape the semicolon so the shell doesn't interpret it.


### Example 6: Removing Files with `find`

You can use `-exec rm` to delete the files you find.

**⚠️ WARNING: Be very careful with this command. It can permanently delete important data.**

Let's remove all files ending in `.log`.

**Step 1: The PREVIEW (Safe)**
First, *always* run your `find` command with `-print` to review the list of files before deleting them.

```bash
hashim@Hashim:~/find_sandbox$ find . -name "*.log" -print
./data.log
./project/abc_special.log
```

**Step 2: The DELETION (Dangerous)**
Once you have confirmed the list is correct, replace `-print` with the `-exec` action.

```bash
hashim@Hashim:~/find_sandbox$ find . -name "*.log" -exec rm {} \;
```

*(No output is shown, but the files are gone.)*

**Step 3: Verify**

```bash
hashim@Hashim:~/find_sandbox$ find . -name "*.log" -print
(No output is shown)
```

#### A Better Way: Using `-delete`

Most modern versions of `find` have a built-in `-delete` action, which is safer and more efficient than using `-exec rm`.

```bash
# This does the same thing as the command above
find . -name "*.log" -delete
```


# 👕 The `tee` Command

The `tee` command is named after a T-splitter in plumbing. It reads from standard input (`stdin`) and writes that input to **two** (or more) places at the same time:

1.  To standard output (`stdout`) (usually your screen).
2.  To a specified file.

This is extremely useful for saving the output of a command to a file *while also seeing it on your screen*.

  * `tee filename`: **Overwrites** the file with the new output.
  * `tee -a filename`: **Appends** the new output to the end of the file.


### 🚀 Practical Example 1: `tee` (Overwrite)

Let's find all `.sh` files and save the list to `/tmp/blue` *while also* seeing the list on our screen.

```bash
hashim@Hashim:~/find_sandbox$ find . -name "*.sh" | tee /tmp/blue
./project/main.sh
./project/src/utils.sh
```

  * **📜 Code Explanation:**
    *(Note: The user text mentions `xargs`, which is not in this command and appears to be an error in the text. The command is a simple `find | grep | tee`.)*

    1.  `find . -name "*.sh"`: This command finds the two shell scripts. Its output is `.\/project\/main.sh\n.\/project\/src\/utils.sh`.
    2.  `| tee /tmp/blue`: This output is piped to `tee`.
    3.  `tee` performs two actions simultaneously:
          * It prints the output to the screen (which you see).
          * It **overwrites** the file `/tmp/blue` with that same output.

  * **Verification:**

    ```bash
    hashim@Hashim:~/find_sandbox$ cat /tmp/blue
    ./project/main.sh
    ./project/src/utils.sh
    ```


### 🚀 Practical Example 2: `tee -a` (Append)

Now, let's find a *different* set of files and **add them** to `/tmp/blue` without deleting the old content.

```bash
hashim@Hashim:~/find_sandbox$ find . -name "*.md" | tee -a /tmp/blue
./project/README.md
```

  * **📜 Code Explanation:**

    1.  `find . -name "*.md"`: This finds the `README.md` file.
    2.  `| tee -a /tmp/blue`: This output is piped to `tee`.
    3.  `-a`: This is the **append** flag.
    4.  `tee` performs two actions:
          * It prints the output to the screen (`./project/README.md`).
          * It **appends** that output to the end of `/tmp/blue`.

  * **Verification:**

    ```bash
    hashim@Hashim:~/find_sandbox$ cat /tmp/blue
    ./project/main.sh
    ./project/src/utils.sh
    ./project/README.md
    ```

    *As you can see, the new file was added to the end, and the original `.sh` files were not overwritten.*

---