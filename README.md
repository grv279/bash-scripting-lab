# 🐚 Bash Scripting Practical Guide

This guide serves as a quick reference and learning resource for Bash scripting. Whether you're new to shell scripts or want a refresher on syntax and concepts, this document covers essential topics with examples.

---

## 📘 Table of Contents

- [🐚 Bash Scripting Practical Guide](#-bash-scripting-practical-guide)
  - [📘 Table of Contents](#-table-of-contents)
  - [🧱 Introduction](#-introduction)
  - [🖇️ Bash shell inbuilt Variables](#️-bash-shell-inbuilt-variables)
    - [Exit Codes in Bash](#exit-codes-in-bash)
  - [📦 Bash Variables](#-bash-variables)
    - [Creating and Using Variables](#creating-and-using-variables)
      - [Escaping characters](#escaping-characters)
  - [💡 Bash Substitutions and Expansions](#-bash-substitutions-and-expansions)
    - [Command Substitution](#command-substitution)
    - [Arithmetic Substitution](#arithmetic-substitution)
    - [Parameter Expansion](#parameter-expansion)
    - [Brace Expansion](#brace-expansion)
    - [Tilde Expansion](#tilde-expansion)
    - [Variable Expansion](#variable-expansion)
    - [Process Substitution](#process-substitution)
    - [Word Splitting](#word-splitting)
    - [ANSI-C Quoting](#ansi-c-quoting)
    - [Pathname Expansion (Globbing)](#pathname-expansion-globbing)
  - [📤 Passing Arguments](#-passing-arguments)
  - [🧵 Input Parameter Parsing](#-input-parameter-parsing)
  - [🧮 Arrays](#-arrays)
  - [➕ Operators](#-operators)
    - [Arithmetic](#arithmetic)
  - [✂️ String Operations](#️-string-operations)
  - [🔍 Comparisons](#-comparisons)
    - [Numeric Comparisons](#numeric-comparisons)
    - [String Comparisons](#string-comparisons)
  - [🔀 Conditionals](#-conditionals)
  - [🎭 Case Statements](#-case-statements)
  - [🔁 Loops](#-loops)
    - [For Loop](#for-loop)
    - [While Loop](#while-loop)
  - [⚙️ Functions](#️-functions)
  - [🚨 Signal Trapping](#-signal-trapping)
  - [🧩 Including Scripts](#-including-scripts)
  - [📂 File Tests](#-file-tests)
  - [🛠️ Useful Commands](#️-useful-commands)
  - [🔗 Pipelines](#-pipelines)
  - [🌀 Subshells](#-subshells)
  - [🧠⚙️ Job Control](#️-job-control)
  - [📎 License](#-license)
  - [🙌 Contributions Welcome!](#-contributions-welcome)

---

## 🧱 Introduction

Bash (short for Bourne Again Shell) is a powerful command-line interpreter and scripting language developed for Unix-like operating systems. Created in 1989 by Brian Fox as part of the GNU Project, Bash was designed as a free and open-source replacement for the original Bourne shell (sh). Today, it is the default shell in most Linux distributions and widely available across modern operating systems. Bash enables users to interact with the system through text-based commands, automate tasks with scripts, and perform complex operations with ease. Whether used in interactive terminals or non-interactive scripts, Bash remains an essential tool for developers, system administrators, and power users alike.

---

## 🖇️ Bash shell inbuilt Variables

Bash shell includes a variety of inbuilt variables, also known as special variables or environment variables, that provide information about the shell's environment, the current user, or the execution of commands and scripts. These variables are automatically set and maintained by the shell.

| Variable | Description |
|----------|-------------|
| `$0`     | Name of the script |
| `$1`...`$n` | Positional arguments |
| `$#`     | Total number of arguments |
| `$@`     | All arguments as separate strings |
| `$*`     | All arguments as a single string |
| `$?`     | Exit status of the last command |
| `$$`     | Process ID of the current shell |
| `$!`     | PID of the last background command |

### Exit Codes in Bash

When a command runs in Bash, it returns an **exit code** (or **return code**) indicating success or failure. This code is stored in the special variable `$?`.

- `0` usually means **success**
- Non-zero values indicate **errors** or **partial failure**

In **arithmetic contexts** like `((...))` or `$((...))`, the truth logic is reversed:
- `1` means **true**
- `0` means **false**

> Note: Not all commands use standardized exit codes. For details, refer to the command's man page.

```bash
$ true; echo "$?"
0

$ false; echo "$?"
1

$ (( 1 + 1 )); printf 'exit-code: %d\n' "$?"
exit-code: 0

$ (( 1 - 1 )); printf 'exit-code: %d\n' "$?"
exit-code: 1

$ bash -c 'exit 99'; printf 'exit-code: %d\n' "$?"
exit-code: 99
```

---

## 📦 Bash Variables

Bash variables are used to store and manipulate data such as strings, numbers, and arrays. They're essential for scripting and managing shell sessions.

### Creating and Using Variables

- **Assignment**:  
  No spaces around `=`  
  ```bash
  my_variable="Hello"
  ```

- **Referencing**:  
  Use `$` to access the value  
  ```bash
  echo $my_variable
  ```

> Notes:
  - No space around `=` when assigning.
  - Case-sensitive.
  - Underscores `_` allowed in names.

| Syntax       | Effect                      | Notes                    |
|--------------|-----------------------------|--------------------------|
| `TMP=abc`    | Assign string `abc`         | Basic string             |
| `TMP="abc"`  | Assign with expansion       | Double quotes support expansion |
| `TMP='abc'`  | Literal string              | No expansion             |
| `TMP=`cmd` | Output of command `cmd`     | Backticks for commands   |

#### Escaping characters

```bash
PRICE=5
echo "Price is: \$${PRICE}"
```

---
## 💡 Bash Substitutions and Expansions

### Command Substitution  
Capture command output into a variable:
```bash
current_date=$(date)
```

### Arithmetic Substitution
Evaluates an arithmetic expression:
```bash
sum=$((3 + 5))
```
> Note: This includes almost all "C" language operators for arithmetic and numeric comparison;

### Parameter Expansion
Accesses and manipulates variables:
```bash
echo ${var}         # Basic usage
echo ${var:-default}  # Use default if var is unset or null
echo ${#var}        # Length of variable
echo ${var/pattern/replacement}  # Pattern substitution
```

### Brace Expansion
Generates sequences or sets of strings:
```bash
echo file{1..3}.txt     # file1.txt file2.txt file3.txt
echo {a,b,c}            # a b c
```

### Tilde Expansion
Expands `~` to the home directory:
```bash
cd ~      # Goes to the current user's home
cd ~user  # Goes to 'user's home directory
```

### Variable Expansion
Expands variables inline:
```bash
name="world"
echo "Hello, $name!"
```

### Process Substitution
Creates file descriptors for input/output of commands:
```bash
diff <(ls dir1) <(ls dir2)
```

### Word Splitting
Splits results of expansions into separate words/tokens:
```bash
list="a b c"
for i in $list; do echo $i; done
```

### ANSI-C Quoting
Supports special characters and escape sequences:
```bash
echo $'Line1\nLine2'
```

### Pathname Expansion (Globbing)
Expands wildcards into matching filenames:
```bash
echo *.txt
```

---

## 📤 Passing Arguments

```bash
bash myscript.sh arg1 arg2 "arg with spaces"
```

- `$0`: script name
- `$1`, `$2`, ..., `$@`, `$*`
- `"$@"` is recommended in loops for correct argument handling

---

## 🧵 Input Parameter Parsing

You can use `getopts` for flags:

```bash
while getopts ":a:b:" opt; do
  case $opt in
    a) echo "Option A: $OPTARG" ;;
    b) echo "Option B: $OPTARG" ;;
    \?) echo "Invalid option: -$OPTARG" ;;
  esac
done
```

---


## 🧮 Arrays

```bash
my_array=(apple banana "Fruit Basket" orange)
new_array[2]=apricot

echo ${#my_array[@]}     # Length
echo ${my_array[3]}      # 4th element
```

---

## ➕ Operators

### Arithmetic

```bash
a + b   # Addition
a - b   # Subtraction
a * b   # Multiplication
a / b   # Division (integer)
a % b   # Modulo
a ** b  # Exponentiation
```

---

## ✂️ String Operations

```bash
string="Hello World"
echo ${#string}            # Length
echo ${string:0:5}         # Substring
echo ${string:6}           # "World"

# Replace substrings
text="to be or not to be"
echo ${text[@]/be/eat}     # Replace first
echo ${text[@]//be/eat}    # Replace all
```

---

## 🔍 Comparisons

### Numeric Comparisons

| Expression    | Meaning           |
|---------------|-------------------|
| `$a -lt $b`   | a < b             |
| `$a -gt $b`   | a > b             |
| `$a -le $b`   | a ≤ b             |
| `$a -ge $b`   | a ≥ b             |
| `$a -eq $b`   | a == b            |
| `$a -ne $b`   | a != b            |

### String Comparisons

| Expression      | Meaning                |
|------------------|------------------------|
| `"$a" = "$b"`    | Equal                  |
| `"$a" != "$b"`   | Not equal              |
| `-z "$a"`        | Empty string           |

---

## 🔀 Conditionals

```bash
NAME="George"
if [ "$NAME" = "John" ]; then
  echo "John Lennon"
elif [ "$NAME" = "George" ]; then
  echo "George Harrison"
else
  echo "This leaves us with Paul and Ringo"
fi
```

Logical conditions with `[[ ... ]]`:

```bash
if [[ ${A[0]} -eq 1 && ($B == "bee" || $T == "tee") ]]; then
  echo "Match!"
fi
```

---

## 🎭 Case Statements

```bash
mycase=2
case $mycase in
  1) echo "bash";;
  2) echo "perl";;
  3) echo "python";;
  4) echo "c++";;
  5) exit;;
esac
```

---

## 🔁 Loops

### For Loop

```bash
for item in "${array[@]}"; do
  echo "Item: $item"
done
```

Loop over output:

```bash
for file in $(ls *.sh); do
  echo "File: $file"
done
```

### While Loop

```bash
COUNT=3
while [ $COUNT -gt 0 ]; do
  echo "Count: $COUNT"
  COUNT=$((COUNT - 1))
done
```

---

## ⚙️ Functions

```bash
function greet {
  echo "Hello $1"
}

result=$(greet "world")
echo "$result"
```

Use `echo` to return values (captured via `$(...)`).

Quoting variables prevents unintended expansions.

---

## 🚨 Signal Trapping

```bash
trap "echo 'SIGINT received'" SIGINT
trap "echo 'SIGTERM received'" SIGTERM
trap "echo 'Cleaning up...'" EXIT

echo "Running..."
sleep 30
```

| Signal  | Trigger         | Description           | Trap? |
|---------|------------------|------------------------|--------|
| SIGINT  | Ctrl+C           | Interrupt              | ✅     |
| SIGTERM | `kill`           | Terminate              | ✅     |
| EXIT    | On exit          | Script ends            | ✅     |

---

## 🧩 Including Scripts

```bash
source ./lib.sh
# or
. ./lib.sh
```

Allows you to reuse code or functions from another script.

---

## 📂 File Tests

| Test     | Checks                            |
|----------|-----------------------------------|
| `-e`     | File exists                       |
| `-f`     | Regular file                      |
| `-d`     | Directory                         |
| `-r`     | Readable                          |
| `-w`     | Writable                          |
| `-x`     | Executable                        |
| `-nt`    | Newer than another file           |
| `-ot`    | Older than another file           |
| `! -e`   | File does **not** exist           |

---

## 🛠️ Useful Commands

| Command | Use                             | Example                     |
|---------|----------------------------------|-----------------------------|
| `grep`  | Search text                     | `grep "pattern" file.txt`   |
| `sed`   | Replace/edit text               | `sed 's/old/new/g'`         |
| `awk`   | Filter or print by columns      | `awk '{print $2}' file.txt` |
| `sort`  | Sort lines in files             | `sort -n file.txt`          |

---

## 🔗 Pipelines

---

## 🌀 Subshells

---

## 🧠⚙️ Job Control

---


## 📎 License

MIT License

---

## 🙌 Contributions Welcome!

Feel free to fork and improve this guide. Pull requests are welcome!