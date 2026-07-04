### Python notes
* A file ending in **`.py`** is a Python program.

* When you run a Python file, the **Python interpreter** reads the code line by line and understands what each statement means.

* In the example:

  ```python
  print("Hello Python world!")
  ```

print() is a built-in function that displays the text inside the parentheses on the screen.

Output:
Hello Python world!

Code editors use syntax highlighting, which colors different parts of the code (such as functions, strings, keywords, etc.) to make programs easier to read and understand.

In one sentence:
When you run a .py file, the Python interpreter executes the code, and syntax highlighting in the editor helps you identify different parts of the program visually.

### Summary: Variables in Python

* A **variable** is a name used to store a value.

* You assign a value to a variable using the **assignment operator (`=`)**.

* Example:

  ```python
  message = "Hello Python world!"
  print(message)
  ```

* Here, `message` stores the string `"Hello Python world!"`.

* When `print(message)` is executed, Python prints the value stored in the variable.

* A variable's value can be **changed (reassigned)** at any time.

Example:

```python
message = "Hello Python world!"
print(message)

message = "Hello Python Crash Course world!"
print(message)
```

**Output:**

```text
Hello Python world!
Hello Python Crash Course world!
```

### Key Points for Interviews

* A **variable** is a named reference to a value.
* Use `=` to assign a value to a variable.
* Variables can be **reassigned**, and Python always uses the **latest assigned value**.
* `print(variable_name)` displays the value stored in the variable, not the variable name itself.
### Summary: Naming and Using Variables in Python

Follow these rules when naming variables:

1. **Use only letters, numbers, and underscores (`_`).**

   * ✅ `message`, `student_name`, `message_1`
   * ❌ `1message` (cannot start with a number)

2. **Variable names cannot contain spaces.**

   * ✅ `greeting_message`
   * ❌ `greeting message`

3. **Do not use Python keywords or built-in function names.**

   * ❌ `print`, `if`, `for`, `class`

4. **Use meaningful and descriptive names.**

   * ✅ `student_name`, `total_marks`
   * ❌ `x`, `a`, `s_n` (unless in small examples)

5. **Avoid confusing names.**

   * Don't use lowercase **`l`** or uppercase **`O`**, as they can look like **`1`** and **`0`**.

6. **Prefer lowercase variable names.**

   * Example: `user_name`, `total_price`

### Good Examples

```python
student_name = "Khushi"
age = 21
total_marks = 450
```

### Bad Examples

```python
1name = "Khushi"        # Starts with a number ❌
student name = "Khushi" # Contains space ❌
print = "Hello"         # Uses built-in function name ❌
```

### Interview Points

* Variable names must start with a **letter or underscore (`_`)**.
* They can contain **letters, digits, and underscores**.
* **Spaces are not allowed**.
* **Keywords and built-in function names** should not be used as variable names.
* Use **short, meaningful, lowercase** names following the `snake_case` convention (e.g., `student_name`).

  ### Summary: Avoiding Name Errors When Using Variables

* A **`NameError`** occurs when you use a variable that has **not been defined** or is **misspelled**.

### Example of a NameError

```python
message = "Hello Python Crash Course reader!"
print(mesage)   # Misspelled variable name
```

**Output:**

```text
NameError: name 'mesage' is not defined. Did you mean: 'message'?
```

### What is a Traceback?

A **traceback** is an error report that helps you debug your program. It shows:

* The file where the error occurred.
* The line number of the error.
* The type of error (e.g., `NameError`).
* Sometimes a suggestion for the correct variable name.

### Correct Code

```python
message = "Hello Python Crash Course reader!"
print(message)
```

### Consistent Names Work

Even if the spelling is unusual, Python accepts it as long as it's **consistent**.

```python
mesage = "Hello Python Crash Course reader!"
print(mesage)
```

**Output:**

```text
Hello Python Crash Course reader!
```

### Key Interview Points

* **`NameError`** happens when a variable is undefined or misspelled.
* A **traceback** tells you where and why the error occurred.
* Python is **case-sensitive** and requires variable names to match exactly.
* Python does **not** check English spelling—it only checks whether the variable name is used consistently.

  ### Summary: Variables Are Labels

* In Python, a variable is **not a box** that stores data.
* A variable is better thought of as a **label (or reference)** that points to a value (object) in memory.
* Multiple variables can even refer to the **same object**.

### Example

```python
message = "Hello"
```

Here:

* `message` is the **label (reference)**.
* `"Hello"` is the **value (object)**.

If you change the variable:

```python
message = "Hi"
```

The label `message` now points to a new value (`"Hi"`).

### Why is this important?

Understanding variables as **references** helps explain Python's behavior, especially with **mutable objects** (like lists) and **immutable objects** (like strings and integers).

### Interview Points

* A variable is a **reference (label)** to an object, **not the object itself**.
* Variables **point to values** stored in memory.
* Reassigning a variable changes **what it references**, not necessarily the original object.
* This concept is important for understanding **assignment, memory management, mutability, and object references** in Python.

  ### Summary: Strings in Python

#### What is a String?

* A **string (`str`)** is a sequence of characters.
* Anything enclosed in **single quotes (`' '`)** or **double quotes (`" "`)** is a string.

Examples:

```python
"This is a string."
'This is also a string.'
```

You can mix quotes to include apostrophes or quotation marks:

```python
'I told my friend, "Python is my favorite language!"'
"Python's community is supportive."
```

---

## String Methods

A **method** is a function that belongs to an object and performs an action on it.

**Syntax:**

```python
object.method()
```

Example:

```python
name = "ada lovelace"
print(name.title())
```

**Output:**

```text
Ada Lovelace
```

### Common String Methods

#### 1. `title()`

* Converts the first letter of each word to uppercase.

```python
name = "ada lovelace"
print(name.title())
```

**Output:**

```text
Ada Lovelace
```

---

#### 2. `upper()`

* Converts all characters to uppercase.

```python
name = "Ada Lovelace"
print(name.upper())
```

**Output:**

```text
ADA LOVELACE
```

---

#### 3. `lower()`

* Converts all characters to lowercase.

```python
name = "Ada Lovelace"
print(name.lower())
```

**Output:**

```text
ada lovelace
```

---

### Why use `lower()`?

* It helps store data consistently.
* Useful when comparing user input regardless of capitalization.

Example:

```python
name = input("Enter your name: ")
print(name.lower())
```

If the user enters:

```text
ADA
Ada
aDa
```

All become:

```text
ada
```

---

## Interview Points

* A **string (`str`)** is a sequence of characters enclosed in single or double quotes.
* A **method** is a function associated with an object, called using dot notation (`object.method()`).
* Common string methods:

  * `title()` → Capitalizes the first letter of each word.
  * `upper()` → Converts the entire string to uppercase.
  * `lower()` → Converts the entire string to lowercase.
* `lower()` is commonly used to normalize user input for comparisons and storage.

## Summary: Using Variables in Strings & String Formatting

### 1. f-Strings (Formatted Strings)

* **f-strings** are used to insert variables directly into a string.
* Prefix the string with **`f`** and place variables inside **`{}`**.

**Example:**

```python
first_name = "ada"
last_name = "lovelace"

full_name = f"{first_name} {last_name}"
print(full_name)
```

**Output:**

```text
ada lovelace
```

---

### 2. Using Expressions and Methods in f-Strings

You can also call methods inside `{}`.

```python
first_name = "ada"
last_name = "lovelace"

full_name = f"{first_name} {last_name}"
print(f"Hello, {full_name.title()}!")
```

**Output:**

```text
Hello, Ada Lovelace!
```

---

### 3. Storing an f-String in a Variable

```python
message = f"Hello, {full_name.title()}!"
print(message)
```

This makes your code cleaner and more readable.

---

# Whitespace in Strings

**Whitespace** means:

* Space (` `)
* Tab (`\t`)
* Newline (`\n`)

---

## 4. Tab (`\t`)

Adds horizontal space.

```python
print("\tPython")
```

**Output:**

```text
    Python
```

---

## 5. Newline (`\n`)

Moves text to the next line.

```python
print("Languages:\nPython\nC\nJava")
```

**Output:**

```text
Languages:
Python
C
Java
```

---

## 6. Combining `\n` and `\t`

```python
print("Languages:\n\tPython\n\tC\n\tJava")
```

**Output:**

```text
Languages:
    Python
    C
    Java
```

---

# Removing Extra Whitespace (Stripping)

Extra spaces can cause comparison problems.

Example:

```python
name = "python "
```

Although it looks similar to `"python"`, Python treats them as **different strings**.

---

## 7. `rstrip()`

Removes whitespace from the **right** side.

```python
language = "python "
print(language.rstrip())
```

**Output**

```text
python
```

---

## 8. `lstrip()`

Removes whitespace from the **left** side.

```python
language = " python"
print(language.lstrip())
```

**Output**

```text
python
```

---

## 9. `strip()`

Removes whitespace from **both** sides.

```python
language = " python "
print(language.strip())
```

**Output**

```text
python
```

---

## 10. Permanent vs Temporary Changes

This **does NOT** change the original string:

```python
language = "python "
language.rstrip()
```

To save the change permanently:

```python
language = language.rstrip()
```

---

# Interview Points

* **f-string**: A formatted string prefixed with `f` that allows variables and expressions inside `{}`.
* `\t` → Inserts a **tab**.
* `\n` → Inserts a **new line**.
* **Whitespace** includes spaces, tabs, and newlines.
* String methods:

  * `rstrip()` → Removes trailing (right-side) whitespace.
  * `lstrip()` → Removes leading (left-side) whitespace.
  * `strip()` → Removes whitespace from both ends.
* String methods return a **new string** because **strings are immutable**. To keep the change, assign the result back to the variable:

```python
language = language.strip()
```
## Summary: Removing Prefixes & Avoiding Syntax Errors

# 1. `removeprefix()`

* `removeprefix()` removes a specified prefix from the beginning of a string.
* It **returns a new string** and **does not modify the original string**.

### Example

```python
url = "https://nostarch.com"
print(url.removeprefix("https://"))
```

**Output**

```text
nostarch.com
```

To save the change permanently:

```python
url = url.removeprefix("https://")
```

or

```python
simple_url = url.removeprefix("https://")
```

---

# 2. Syntax Error

A **SyntaxError** occurs when Python cannot understand your code because it breaks Python's grammar (syntax).

---

## Correct Example

Use **double quotes** if the string contains an apostrophe.

```python
message = "One of Python's strengths is its diverse community."
print(message)
```

**Output**

```text
One of Python's strengths is its diverse community.
```

---

## Incorrect Example

```python
message = 'One of Python's strengths is its diverse community.'
```

**Output**

```text
SyntaxError: unterminated string literal
```

**Reason:**

* Python thinks the string ends at:

```python
'One of Python'
```

The remaining text is treated as invalid Python code.

---

## How to Avoid This Error

### Option 1 (Recommended)

Use double quotes:

```python
message = "Python's syntax is simple."
```

### Option 2

Escape the apostrophe using a backslash (`\`):

```python
message = 'Python\'s syntax is simple.'
```

---

# Interview Points

* `removeprefix(prefix)` removes the specified prefix from a string.
* Strings are **immutable**, so `removeprefix()` returns a **new string**.
* A **SyntaxError** occurs when Python cannot parse the code because it violates Python's syntax rules.
* If a string contains an apostrophe (`'`), either:

  * Use **double quotes** around the string, or
  * Escape the apostrophe with `\`.
* Editors with **syntax highlighting** can help you spot mismatched quotes and other syntax errors quickly.

  ## Summary: Numbers in Python

Python supports two main numeric types:

1. **Integers (`int`)** → Whole numbers
2. **Floats (`float`)** → Decimal numbers

---

# 1. Integers (`int`)

Integers are whole numbers without a decimal point.

Examples:

```python
10
-5
0
100
```

### Arithmetic Operators

| Operator | Meaning | Example | Result |
| -------- | ------- | ------- | ------ |
| `+` | Addition | `2 + 3` | `5` |
| `-` | Subtraction | `3 - 2` | `1` |
| `*` | Multiplication | `2 * 3` | `6` |
| `/` | Division | `3 / 2` | `1.5` |
| `**` | Exponent (Power) | `3 ** 2` | `9` |

Example:

```python
print(2 + 3)
print(3 - 2)
print(2 * 3)
print(3 / 2)
print(3 ** 2)
```

---

# 2. Order of Operations (PEMDAS/BODMAS)

Python follows the normal mathematical order.

```python
2 + 3 * 4
```

Result:

```text
14
```

Using parentheses changes the order:

```python
(2 + 3) * 4
```

Result:

```text
20
```

---

# 3. Floats (`float`)

A **float** is any number containing a decimal point.

Examples:

```python
3.14
0.5
2.0
-7.8
```

Operations:

```python
0.1 + 0.1
0.2 + 0.2
2 * 0.1
```

Results:

```text
0.2
0.4
0.2
```

---

# 4. Floating-Point Precision

Sometimes decimal calculations produce small rounding errors.

Example:

```python
0.1 + 0.2
```

Output:

```text
0.30000000000000004
```

This is normal because computers cannot represent some decimal numbers exactly in binary.

---

# 5. Integer and Float Together

If **any operand is a float**, the result is a **float**.

Examples:

```python
4 / 2
```

Output:

```text
2.0
```

```python
1 + 2.0
```

Output:

```text
3.0
```

```python
2 * 3.0
```

Output:

```text
6.0
```

---

# 6. Underscores in Large Numbers

Underscores improve readability.

```python
population = 1_400_000_000
```

Python ignores the underscores.

```python
print(population)
```

Output:

```text
1400000000
```

Both are equivalent:

```python
1000000
1_000_000
```

---

# Interview Points

* **`int`** represents whole numbers.
* **`float`** represents decimal numbers.
* Arithmetic operators:

  * `+` → Addition
  * `-` → Subtraction
  * `*` → Multiplication
  * `/` → Division (always returns a `float` in Python 3)
  * `**` → Exponentiation (power)
* Python follows the standard order of operations (PEMDAS/BODMAS); parentheses can change evaluation order.
* Decimal calculations may show small precision errors due to floating-point representation.
* If an expression contains a `float`, the result is usually a `float`.
* Underscores (`_`) in numeric literals improve readability and are ignored by Python.


## Summary: Multiple Assignment & Constants

# 1. Multiple Assignment

* Python allows you to assign values to **multiple variables in a single line**.
* The number of variables must match the number of values.

### Syntax

```python
variable1, variable2 = value1, value2
```

### Example

```python
x, y, z = 0, 0, 0

print(x)
print(y)
print(z)
```

**Output**

```text
0
0
0
```

Another example:

```python
name, age, city = "Khushi", 21, "Panipat"
```

Python assigns:

* `name` → `"Khushi"`
* `age` → `21`
* `city` → `"Panipat"`

---

# 2. Constants

A **constant** is a variable whose value is **intended to remain unchanged** throughout the program.

Python **does not have true constants**, but by convention, programmers write constant names in **UPPERCASE**.

### Example

```python
MAX_CONNECTIONS = 5000
PI = 3.14159
```

Although you *can* change these values:

```python
PI = 3.14
```

you **should not**, because uppercase names indicate they should be treated as constants.

---

## Interview Points

* **Multiple assignment** lets you assign values to multiple variables in one statement.
* The **number of variables must equal the number of values**, otherwise Python raises a `ValueError`.
* Python has **no built-in constant type**.
* By convention, constants are written in **uppercase** (e.g., `PI`, `MAX_SIZE`) to indicate they should not be modified.

## Summary: Comments in Python

### What are Comments?

* **Comments** are notes written inside the code to explain what the code does.
* They are **ignored by the Python interpreter** and are **not executed**.

---

## How to Write Comments

In Python, comments start with the **hash (`#`)** symbol.

### Example

```python
# Say hello to everyone
print("Hello Python people!")
```

**Output**

```text
Hello Python people!
```

The first line is a comment, so Python ignores it.

---

## Why Use Comments?

Comments help to:

* Explain what the code does.
* Make code easier to read and maintain.
* Help you understand your own code later.
* Help other programmers understand your code.
* Document the logic or approach used to solve a problem.

---

## Good Comment Example

```python
# Calculate the total price including tax
total = price + tax
```

---

## Bad Comment Example

```python
# Add two numbers
x = 5 + 10
```

This comment is unnecessary because the code is already obvious.

---

## Best Practices

* Write **clear and meaningful** comments.
* Explain **why** the code is written, not just **what** it does.
* Keep comments **short, accurate, and up to date**.
* Avoid commenting every obvious line of code.

---

## Interview Points

* A **comment** is text that explains code and is ignored by the Python interpreter.
* Single-line comments begin with the **`#`** symbol.
* Comments improve **readability**, **maintainability**, and **collaboration**.
* Good comments explain the **purpose or logic** behind the code, especially for complex sections.

## Summary: The Zen of Python

The **Zen of Python** is a collection of guiding principles for writing clean, readable, and Pythonic code. You can view it by running:

```python
import this
```

### Key Principles

1. **Beautiful is better than ugly.**

   * Write clean, elegant, and well-organized code.

2. **Simple is better than complex.**

   * Choose the simplest solution that solves the problem.

3. **Complex is better than complicated.**

   * If complexity is unavoidable, keep the solution as clear and manageable as possible.

4. **Readability counts.**

   * Write code that is easy for you and others to understand.
   * Use meaningful variable names and helpful comments.

5. **There should be one—and preferably only one—obvious way to do it.**

   * Follow common Python conventions instead of creating unnecessarily different solutions.

6. **Now is better than never.**

   * Write working code first, then improve or optimize it later.

---

## Interview Points

* The **Zen of Python** is a set of design principles written by Tim Peters.
* It can be displayed using:

```python
import this
```

* Core philosophy:

  * Write **simple** and **readable** code.
  * Prefer **clarity** over cleverness.
  * Follow **Python conventions**.
  * Build working solutions first, then refine them.

### Most Frequently Asked Zen of Python Principles

* **Beautiful is better than ugly.**
* **Simple is better than complex.**
* **Readability counts.**
* **Explicit is better than implicit.**
* **There should be one—and preferably only one—obvious way to do it.**
* **Now is better than never.**

> **Easy interview definition:**
> *The Zen of Python is a collection of guiding principles by Tim Peters that encourage writing code that is simple, readable, maintainable, and Pythonic.*


# Summary: Lists in Python

## What is a List?

* A **list** is an **ordered collection of items**.
* A list can store **strings, numbers, objects, or even other lists**.
* Lists are **mutable**, meaning their elements can be changed after creation.
* Lists are created using **square brackets `[]`**, with elements separated by commas.

### Example

```python
bicycles = ["trek", "cannondale", "redline", "specialized"]
print(bicycles)
```

**Output**

```text
['trek', 'cannondale', 'redline', 'specialized']
```

---

# Accessing Elements

Access elements using their **index**.

### Syntax

```python
list_name[index]
```

Example:

```python
bicycles = ["trek", "cannondale", "redline", "specialized"]

print(bicycles[0])
```

**Output**

```text
trek
```

---

# Using String Methods on List Elements

Since list elements can be strings, you can call string methods on them.

```python
print(bicycles[0].title())
```

**Output**

```text
Trek
```

---

# List Indexing

Python uses **zero-based indexing**.

| Index | Value |
| ----: | ----- |
| 0 | trek |
| 1 | cannondale |
| 2 | redline |
| 3 | specialized |

Example:

```python
print(bicycles[1])
print(bicycles[3])
```

**Output**

```text
cannondale
specialized
```

---

# Negative Indexing

Negative indexes count from the **end** of the list.

| Index | Value |
| ----: | ----- |
| -1 | specialized |
| -2 | redline |
| -3 | cannondale |
| -4 | trek |

Example:

```python
print(bicycles[-1])
```

**Output**

```text
specialized
```

---

# Why Use Negative Indexing?

It lets you access the last elements **without knowing the list length**.

Example:

```python
numbers = [10, 20, 30, 40]

print(numbers[-1])
```

**Output**

```text
40
```

---

# Interview Points

* A **list** is an **ordered, mutable collection** of elements.
* Lists are created using **square brackets (`[]`)**.
* Elements are accessed using **indexes**.
* Python uses **zero-based indexing**, so the first element is at index `0`.
* Negative indexing:

  * `-1` → Last element
  * `-2` → Second last element
  * `-3` → Third last element
* You can call methods (e.g., `.title()`, `.upper()`) on string elements in a list.
* Lists can contain **mixed data types**, although using related types together is generally recommended for clarity.

  ## Summary: Using Individual Values from a List

* Individual elements of a list can be used **just like normal variables**.
* You can access a list element using its **index** and use it in expressions, functions, or **f-strings**.

### Example

```python
bicycles = ["trek", "cannondale", "redline", "specialized"]

message = f"My first bicycle was a {bicycles[0].title()}."

print(message)
```

**Output**

```text
My first bicycle was a Trek.
```

### Explanation

* `bicycles[0]` → Retrieves the first element (`"trek"`).
* `.title()` → Converts `"trek"` to `"Trek"`.
* `f"..."` → Inserts the value into the string.

---

## Another Example

```python
fruits = ["apple", "banana", "mango"]

print(f"My favorite fruit is {fruits[2].title()}.")
```

**Output**

```text
My favorite fruit is Mango.
```

---

## Interview Points

* List elements can be used like any other variable.
* Access elements using **indexing** (`list[index]`).
* List elements can be used inside **f-strings**.
* You can call methods (e.g., `.title()`, `.upper()`, `.lower()`) on string elements directly:

```python
bicycles[0].title()
```

* Combining **list indexing**, **string methods**, and **f-strings** is a common and Pythonic way to build dynamic messages.

# Summary: Modifying, Adding, and Removing Elements in a List

Lists are **dynamic**, meaning you can modify, add, and remove elements after creating them.

---

# 1. Modifying Elements

Use the **index** to replace an existing element.

### Syntax

```python
list_name[index] = new_value
```

### Example

```python
motorcycles = ["honda", "yamaha", "suzuki"]

motorcycles[0] = "ducati"

print(motorcycles)
```

**Output**

```text
['ducati', 'yamaha', 'suzuki']
```

---

# 2. Adding Elements

## a) `append()`

Adds an element to the **end** of the list.

### Syntax

```python
list_name.append(value)
```

### Example

```python
motorcycles = ["honda", "yamaha", "suzuki"]

motorcycles.append("ducati")

print(motorcycles)
```

**Output**

```text
['honda', 'yamaha', 'suzuki', 'ducati']
```

---

### Building a List Dynamically

Start with an empty list.

```python
motorcycles = []

motorcycles.append("honda")
motorcycles.append("yamaha")
motorcycles.append("suzuki")

print(motorcycles)
```

**Output**

```text
['honda', 'yamaha', 'suzuki']
```

This is useful when data comes from **user input**, a **file**, or a **database**.

---

## b) `insert()`

Adds an element at a **specific position**.

### Syntax

```python
list_name.insert(index, value)
```

### Example

```python
motorcycles = ["honda", "yamaha", "suzuki"]

motorcycles.insert(0, "ducati")

print(motorcycles)
```

**Output**

```text
['ducati', 'honda', 'yamaha', 'suzuki']
```

**Note:** Existing elements shift one position to the right.

---

# 3. Removing Elements

## `del` Statement

Removes an element using its **index**.

### Syntax

```python
del list_name[index]
```

### Example 1

```python
motorcycles = ["honda", "yamaha", "suzuki"]

del motorcycles[0]

print(motorcycles)
```

**Output**

```text
['yamaha', 'suzuki']
```

---

### Example 2

```python
motorcycles = ["honda", "yamaha", "suzuki"]

del motorcycles[1]

print(motorcycles)
```

**Output**

```text
['honda', 'suzuki']
```

---

### Important

After using `del`, the removed element is **permanently deleted** and **cannot be retrieved**.

---

# Interview Points

### Modifying

* Replace an element using:

  ```python
  list[index] = new_value
  ```

### Adding

* `append(value)` → Adds to the **end** of the list.
* `insert(index, value)` → Inserts at a **specific index**.

### Removing

* `del list[index]` → Removes an element by **index** permanently.
* Use `del` when you **don't need the removed value later**.

### Common Interview Difference

| Method | Purpose |
| ------ | ------- |
| `append(x)` | Adds an element to the end of the list |
| `insert(i, x)` | Inserts an element at a specified index |
| `del list[i]` | Permanently removes an element by index (does not return it) |

# Summary: Removing Elements from a List

Python provides **three main ways** to remove elements from a list:

* `del` → Remove by **index** (does not return the value).
* `pop()` → Remove by **index** and **return the removed value**.
* `remove()` → Remove by **value**.

---

# 1. `pop()`

The `pop()` method removes and **returns** an element from the list.

### Syntax

```python
list_name.pop()
```

By default, it removes the **last element**.

### Example

```python
motorcycles = ["honda", "yamaha", "suzuki"]

popped_motorcycle = motorcycles.pop()

print(motorcycles)
print(popped_motorcycle)
```

**Output**

```text
['honda', 'yamaha']
suzuki
```

---

## Using the Popped Value

```python
motorcycles = ["honda", "yamaha", "suzuki"]

last_owned = motorcycles.pop()

print(f"The last motorcycle I owned was a {last_owned.title()}.")
```

**Output**

```text
The last motorcycle I owned was a Suzuki.
```

---

# 2. `pop(index)`

You can remove an element from a **specific position**.

### Syntax

```python
list_name.pop(index)
```

### Example

```python
motorcycles = ["honda", "yamaha", "suzuki"]

first_owned = motorcycles.pop(0)

print(first_owned)
```

**Output**

```text
Honda
```

**Note:** After `pop()`, the removed element is **no longer in the list**.

---

# 3. `remove()`

Removes an element by **value**, not by index.

### Syntax

```python
list_name.remove(value)
```

### Example

```python
motorcycles = ["honda", "yamaha", "suzuki", "ducati"]

motorcycles.remove("ducati")

print(motorcycles)
```

**Output**

```text
['honda', 'yamaha', 'suzuki']
```

---

## Using a Variable with `remove()`

```python
motorcycles = ["honda", "yamaha", "suzuki", "ducati"]

too_expensive = "ducati"

motorcycles.remove(too_expensive)

print(f"A {too_expensive.title()} is too expensive for me.")
```

**Output**

```text
A Ducati is too expensive for me.
```

---

## Important Note

`remove()` deletes **only the first occurrence** of the specified value.

Example:

```python
numbers = [1, 2, 2, 3]

numbers.remove(2)

print(numbers)
```

**Output**

```text
[1, 2, 3]
```

The second `2` remains.

---

# Interview Points

### `pop()`

* Removes and **returns** an element.
* Default: removes the **last element**.
* Can also remove an element at a specified index (`pop(index)`).

### `remove()`

* Removes an element by **value**.
* Removes **only the first matching occurrence**.
* Raises a `ValueError` if the value is not found.

### `del`

* Removes an element by **index**.
* Does **not** return the removed value.

---

# Frequently Asked Interview Question

| Method | Removes By | Returns Value? | Error Condition |
| ------- | ---------- | -------------- | --------------- |
| `del list[i]` | Index | ❌ No | `IndexError` if index is invalid |
| `pop()` | Index (default: last) | ✅ Yes | `IndexError` if the list is empty or index is invalid |
| `remove(x)` | Value | ❌ No | `ValueError` if the value is not found |

# Summary: Organizing a List in Python

Python provides several ways to organize lists depending on whether you want to **change the original list** or **keep it unchanged**.

---

# 1. `sort()` – Permanent Sorting

The `sort()` method sorts the **original list permanently**.

### Syntax

```python
list_name.sort()
```

### Example

```python
cars = ["bmw", "audi", "toyota", "subaru"]

cars.sort()

print(cars)
```

**Output**

```text
['audi', 'bmw', 'subaru', 'toyota']
```

---

## Reverse Sorting

Use `reverse=True` to sort in descending (reverse alphabetical) order.

```python
cars = ["bmw", "audi", "toyota", "subaru"]

cars.sort(reverse=True)

print(cars)
```

**Output**

```text
['toyota', 'subaru', 'bmw', 'audi']
```

---

# 2. `sorted()` – Temporary Sorting

The `sorted()` function returns a **new sorted list** without changing the original list.

### Example

```python
cars = ["bmw", "audi", "toyota", "subaru"]

print(sorted(cars))
print(cars)
```

**Output**

```text
['audi', 'bmw', 'subaru', 'toyota']
['bmw', 'audi', 'toyota', 'subaru']
```

The original list remains unchanged.

### Reverse Temporary Sort

```python
print(sorted(cars, reverse=True))
```

---

# 3. `reverse()`

The `reverse()` method reverses the **current order** of the list.

It **does not sort** the list.

### Example

```python
cars = ["bmw", "audi", "toyota", "subaru"]

cars.reverse()

print(cars)
```

**Output**

```text
['subaru', 'toyota', 'audi', 'bmw']
```

Calling `reverse()` again restores the previous order.

---

# 4. `len()`

The `len()` function returns the number of elements in a list.

### Syntax

```python
len(list_name)
```

### Example

```python
cars = ["bmw", "audi", "toyota", "subaru"]

print(len(cars))
```

**Output**

```text
4
```

---

# Interview Points

### `sort()`

* Sorts the **original list permanently**.
* Default order: ascending.
* `reverse=True` sorts in descending order.

### `sorted()`

* Returns a **new sorted list**.
* Does **not** modify the original list.
* Supports `reverse=True`.

### `reverse()`

* Reverses the **existing order** of elements.
* Does **not** sort the list.

### `len()`

* Returns the **number of elements** in a list.
* Counting starts from **1** for length, even though indexing starts from **0**.

---

# Frequently Asked Interview Question

| Function/Method | Changes Original List? | Purpose |
| --------------- | ---------------------- | ------- |
| `sort()` | ✅ Yes | Permanently sorts the list |
| `sorted()` | ❌ No | Returns a new sorted list |
| `reverse()` | ✅ Yes | Reverses the current order of elements |
| `len()` | ❌ No | Returns the total number of elements |


## Summary: Looping Through a List (`for` Loop) in Python

### 1. What is a `for` Loop?

* A **`for` loop** is used to iterate over each element in a sequence (such as a list).
* It executes the same block of code for every element.

### Syntax

```python
for variable in list_name:
    # code to execute
```

### Example

```python
magicians = ["alice", "david", "carolina"]

for magician in magicians:
    print(magician)
```

**Output**

```text
alice
david
carolina
```

---

# 2. How a `for` Loop Works

For the list:

```python
magicians = ["alice", "david", "carolina"]
```

Python performs these steps:

1. `magician = "alice"` → Execute loop body
2. `magician = "david"` → Execute loop body
3. `magician = "carolina"` → Execute loop body
4. End loop when no items remain.

---

# 3. Choosing Good Loop Variable Names

Use meaningful names.

✅ Good Examples

```python
for cat in cats:
```

```python
for dog in dogs:
```

```python
for item in list_of_items:
```

**Best Practice:**
Use a **singular** loop variable (`cat`) with a **plural** list name (`cats`).

---

# 4. Doing More Work Inside a Loop

A loop can execute multiple statements for each item.

### Example

```python
magicians = ["alice", "david", "carolina"]

for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
```

**Output**

```text
Alice, that was a great trick!
David, that was a great trick!
Carolina, that was a great trick!
```

---

# 5. Multiple Statements Inside a Loop

All **indented** statements run once per iteration.

```python
magicians = ["alice", "david", "carolina"]

for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")
    print(f"I can't wait to see your next trick, {magician.title()}.\n")
```

**Output**

```text
Alice, that was a great trick!
I can't wait to see your next trick, Alice.

David, that was a great trick!
I can't wait to see your next trick, David.

Carolina, that was a great trick!
I can't wait to see your next trick, Carolina.
```

---

# 6. Code After a `for` Loop

Code **without indentation** executes **only once**, after the loop finishes.

```python
magicians = ["alice", "david", "carolina"]

for magician in magicians:
    print(f"{magician.title()}, that was a great trick!")

print("Thank you, everyone. That was a great magic show!")
```

**Output**

```text
Alice, that was a great trick!
David, that was a great trick!
Carolina, that was a great trick!
Thank you, everyone. That was a great magic show!
```

---

# 7. Indentation in Python

Python uses **indentation** (spaces or tabs) to define code blocks.

Everything indented under the `for` statement belongs to the loop.

Example:

```python
for item in items:
    print(item)   # Inside the loop

print("Done")     # Outside the loop
```

---

# 8. Common Indentation Errors

### a) Forgetting to Indent

```python
for magician in magicians:
print(magician)
```

**Error**

```text
IndentationError: expected an indented block
```

---

### b) Forgetting to Indent Additional Lines

```python
for magician in magicians:
    print(magician)

print("Next trick!")
```

The second `print()` runs **only once**, not for every magician.

This is a **logical error**.

---

### c) Unnecessary Indentation

```python
message = "Hello"

    print(message)
```

**Error**

```text
IndentationError: unexpected indent
```

---

### d) Indenting Code That Should Be Outside the Loop

```python
for magician in magicians:
    print(magician)
    print("Thank you!")
```

Here, `"Thank you!"` prints **for every iteration** instead of once after the loop.

---

### e) Forgetting the Colon (`:`)

```python
for magician in magicians
    print(magician)
```

**Error**

```text
SyntaxError: expected ':'
```

---

# Interview Points

* A **`for` loop** iterates over each element in a sequence.
* Syntax:

```python
for variable in iterable:
    # loop body
```

* The **loop variable** is temporary and receives one item at a time.
* All **indented statements** belong to the loop.
* Code **outside the indentation** runs only once after the loop.
* Python uses **indentation** to define code blocks.
* Common errors:

  * `IndentationError` → Missing or incorrect indentation.
  * `SyntaxError` → Missing colon (`:`) after the `for` statement.
  * **Logical errors** occur when indentation is syntactically valid but produces the wrong behavior.
