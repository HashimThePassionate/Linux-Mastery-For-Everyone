# 📚 **LINUX SHELL PROGRAMMING**

This folder contains a comprehensive curriculum designed to guide you from basic command-line navigation to advanced data processing and shell scripting mastery using powerful Unix tools like **`grep`**, **`sed`**, and **`awk`**.

---

## 🎯 What You Will Learn (Learning Path)

This curriculum is structured across ten Sections, covering fundamental concepts, powerful commands, and advanced scripting techniques.

### Section 1: [Introduction to the Shell](./01_introduction_to_shell/) 🐚

You will establish a solid foundation by understanding the core differences between **Unix versus Linux** and exploring **Available Shell Types**. This Section introduces **bash**, how to get **help**, **navigate directories**, and use essential commands like **`ls`**, **`cat`**, **`head`**, and **`tail`**. You'll also learn about **File Ownership**, **Environment Variables** (especially **`PATH`**), **Aliases**, and data handling tools like **`printf`** and **`cut`**.

---

### Section 2: [Files and Directories Management](./02_files_and_directories/) 📂

This section focuses on essential file operations: **Create, Copy, Remove, and Move Files**. You will master **hard and soft links** (`ln`), file information commands (`basename`, `dirname`, `file`), and file content analysis (`wc`, `more`, `less`). A critical focus will be placed on **Working with File Permissions** using **`chmod`**, **`umask`**, and managing both **Absolute and Relative Pathnames** and **Redirection Commands**.

---

### Section 3: [Mastering Useful Commands for Data Flow](./03_useful_commands/) 🔄

You will learn specialized commands that are vital for data processing: **`join`**, **`fold`**, **`split`**, **`sort`**, and **`uniq`**. This Section also covers comparing files, inspecting binary content (`od`), character translation (`tr`), finding files (`find`), and file compression using **`tar`**, **`gzip`**, **`zip`**, and their counterparts. Understanding the **Internal Field Separator (`IFS`)** for handling datasets is also a key topic.

---

### Section 4: [Conditional Logic and Loops in Bash](./04_conditional_logic_and_loops/) 🚦

This is where scripting begins! You will cover an **Overview of Operators** (Arithmetic, Boolean, String, File Test) and **Working with Variables** and user input (`read`). The core concepts of flow control will be introduced: **Conditional Logic** with **`if/else/fi`** and **`case/esac`** statements. You will master **Loops** using **`for`**, **`while`**, and **`until`** constructs, and learn to create reusable **User-defined Functions** and work with **Arrays**.

---

### Section 5: [Filtering Data with `grep`](./05_filtering_data_with_grep/) 🔎

The essential tool for pattern matching. You will learn **What is the `grep` Command?** and how to use its various **Metacharacters** and **Character Classes**. Topics include useful **`grep` Options** (like `-c`), matching ranges of lines, using **Back References**, and advanced searching techniques including piping output to **`xargs`** and searching within zipped files. The differences between **`grep`**, **`egrep`**, and **`fgrep`** will be clarified.

---

### Section 6: [Transforming Data with `sed`](./06_transforming_data_with_sed/) 📝

You will transition from filtering to **transforming** data. This Section explains the **`sed` Execution Cycle**, focusing on **Matching** and **Substituting** string patterns. You will learn to delete lines, replace vowels, handle datasets with **Multiple Delimiters**, and use **Character Classes** within `sed`. Advanced topics include **Back References** and essential **One-Line `sed` Commands**.

---

### Section 7: [Doing Everything Else with `awk`](./07_working_with_awk/) 💡

The most powerful text processing tool. You will learn **How Does the `awk` Command Work?**, utilizing its **Built-in Variables** and **Control Statements** (loops and conditionals). Key skills include **Aligning Text** with `printf()`, **Deleting/Merging/Joining Lines**, working with **Metacharacters**, and performing **Numeric Functions**. Advanced exercises cover **Count Occurrences**, **Aligning Columns**, **Removing Columns**, and **Counting Word Frequency**.

---

### Section 8: [Introduction to Shell Scripts and Functions](./08_shell_scripts_and_functions/) ⚙️

This Section solidifies scripting skills by defining **What are Shell Scripts?** and teaching you to **Source or “Dot” a Shell Script**. The focus shifts to advanced **Working with Functions**, including **Passing Values** and understanding **Positional Parameters**. You will apply recursion to calculate complex values like **Factorial Values**, **Fibonacci Numbers**, and **Prime Divisors**.

---

### Section 9: [Shell Scripts with `grep` and `awk` Commands](./09_shell_scripts_with_sed_and_awk/) 🚀

You will combine your knowledge to write powerful scripts. This section focuses on using **`grep` to Simulate Relational Data**, processing **Multi-line Records**, and advanced **`awk`** usage for **Scanning Diagonal Elements** and **Adding Values From Multiple Datasets** in complex ways.

---

### Section 10: [Miscellaneous Shell Scripts and System Tasks](./10_misscellaneous_shell_scripts/) 🔧

The final Section covers system administration and utility scripting. Topics include using **`rm`** and **`mv` with Directories**, advanced **`find`** command usage, and controlling file actions like **Compressing and Uncompressing Files**. Crucially, you will learn **Scheduled Commands** using **`crontab`**, executing commands in the **Background** (`nohup`), and essential process management commands like **Terminating Processes** and **Monitoring Processes**. Final utilities include **Disk Usage Commands** and **Arithmetic with `bc` and `dc`**.