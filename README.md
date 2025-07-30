# 🐚 Bash Scripting Practical Guide

This guide serves as a quick reference and learning resource for Bash scripting. Whether you're new to shell scripts or want a refresher on syntax and concepts, this document covers essential topics with examples.

---

## 📘 Table of Contents

- [🐚 Bash Scripting Practical Guide](#-bash-scripting-practical-guide)
  - [📘 Table of Contents](#-table-of-contents)
  - [🧱 Basics](#-basics)
  - [🖇️ Shell Variables](#️-shell-variables)
  - [📦 Bash Variables](#-bash-variables)
    - [Escaping characters](#escaping-characters)
  - [📤 Passing Arguments](#-passing-arguments)
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
  - [🧵 Input Parameter Parsing](#-input-parameter-parsing)
  - [📎 License](#-license)
  - [🙌 Contributions Welcome!](#-contributions-welcome)

---

## 🧱 Basics

- `#` is used to write comments.
- To check the currently active shell:
  ```bash
  ps | grep $$
  ```

---

## 🖇️ Shell Variables

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

---

## 📦 Bash Variables

- No space around `=` when assigning.
- Case-sensitive.
- Underscores `_` allowed in names.

| Syntax       | Effect                      | Notes                    |
|--------------|-----------------------------|--------------------------|
| `TMP=abc`    | Assign string `abc`         | Basic string             |
| `TMP="abc"`  | Assign with expansion       | Double quotes support expansion |
| `TMP='abc'`  | Literal string              | No expansion             |
| `TMP=`cmd` | Output of command `cmd`     | Backticks for commands   |

### Escaping characters

```bash
PRICE=5
echo "Price is: \$${PRICE}"
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

## 📎 License

MIT License

---

## 🙌 Contributions Welcome!

Feel free to fork and improve this guide. Pull requests are welcome!