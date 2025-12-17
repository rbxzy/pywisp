# PythonWisp Syntax Reference

Complete syntax guide for the PythonWisp DSL (Python-style syntax that transpiles to JavaScript/TypeScript).

---

## Table of Contents

- [Variables](#variables)
- [Functions](#functions)
- [Classes and Inheritance](#classes-and-inheritance)
- [Objects](#objects)
- [Lists](#lists)
- [Property Access](#property-access)
- [Data Types](#data-types)
- [Expressions](#expressions)
- [Comments](#comments)
- [Strings](#strings)
- [Control Flow](#control-flow)

---

## Variables

### Local Variables

All variables are local by default (scoped to their block).

```python
x = 10
name = "Alice"
isActive = True
```

**Transpiles to:**
```javascript
var x:any = 10;
var name:any = "Alice";
var isActive:any = true;
```

### Global Variables

To expose variables globally, use the `global` keyword.

```python
global x = 10
global name = "Alice"
```

**Transpiles to:**
```javascript
globals.x = 10;
globals.name = "Alice";
```

**Note:** Use `global` sparingly to avoid polluting the global scope.

---

## Functions

### Local Functions

Functions are local by default and use `def` keyword.

```python
def greet(name):
    print("Hello, " + name)

greet("World")
```

**Transpiles to:**
```javascript
function greet(name:any) {
    console.log(("Hello, " + name));
}
greet("World");
```

### Global Functions

Use `global` keyword to expose functions globally.

```python
global def greet(name):
    print("Hello, " + name)

greet("World")
```

**Transpiles to:**
```javascript
globals.greet = function(name:any) {
    console.log(("Hello, " + name));
}
globals.greet("World");
```

### Lambda Functions

Functions can be assigned to variables using lambda syntax.

```python
sayHello = lambda name: "Hello, " + name

print(sayHello("Alice"))
```

---

## Classes and Inheritance

### Basic Class

Classes must have an `init` method (constructor). Use `pass` for empty class bodies.

```python
class Person:
    def init(name, age):
        self.name = name
        self.age = age
    
    def greet():
        print("Hi, I'm " + self.name)

person = Person("Alice", 30)
person.greet()
```

**Transpiles to:**
```javascript
function Person(name:any, age:any) {
    this.name = name;
    this.age = age;
}
Person.prototype.greet = function() {
    console.log(("Hi, I'm " + this.name));
};
var person:any = new Person("Alice", 30);
person.greet();
```

### Empty Classes

Use `pass` to create empty class bodies.

```python
class Animal:
    pass

class EmptyClass:
    def init():
        pass
```

### Class Inheritance

Use `implements` for inheritance (single inheritance only).

```python
class Animal:
    def init(name):
        self.name = name
    
    def speak():
        print(self.name + " makes a sound")

class Dog implements Animal:
    def init(name, breed):
        self.name = name
        self.breed = breed
    
    def speak():
        print(self.name + " barks")

dog = Dog("Buddy", "Golden Retriever")
dog.speak()
```

**Note:** 
- All class definitions are **local by default**
- To expose a class globally, wrap it in a global variable:
```python
global MyClass = class MyClass:
    def init():
        pass
```

---

## Objects

### Object Literals

Objects use `=` for key-value pairs inside curly braces `{}`.

```python
person = {
    name = "Alice",
    age = 30,
    isStudent = False
}

print(person.name)
```

**Transpiles to:**
```javascript
var person:any = { name: "Alice", age: 30, isStudent: false };
console.log((person.name));
```

### Nested Objects

Objects can be nested arbitrarily.

```python
person = {
    name = "Alice",
    address = {
        street = "123 Main St",
        city = "Springfield",
        coordinates = {
            lat = 40.7128,
            lng = -74.0060
        }
    }
}

print(person.address.city)
print(person.address.coordinates.lat)
```

### Objects with Methods

Methods can reference the object by name (not `self` - that's only for classes).

```python
person = {
    name = "Jim",
    sayName = lambda: print(person.name)
}

person.sayName()
```

**Transpiles to:**
```javascript
var person:any = { 
    name: "Jim", 
    sayName: function() {
        console.log((person.name));
    } 
};
person.sayName();
```

---

## Lists

### List Literals

Lists use curly braces `{}` with comma-separated values (no keys).

```python
numbers = {1, 2, 3, 4, 5}
names = {"Alice", "Bob", "Charlie"}
mixed = {1, "two", 3, True}
```

**Transpiles to:**
```javascript
var numbers:any = [1, 2, 3, 4, 5];
var names:any = ["Alice", "Bob", "Charlie"];
var mixed:any = [1, "two", 3, true];
```

### Nested Lists

Lists can be nested.

```python
matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
}

print(matrix[0][0])  # Prints 1
```

### List Indexing

Lists are zero-indexed (like JavaScript arrays).

```python
fruits = {"apple", "banana", "orange"}

print(fruits[0])  # apple
print(fruits[1])  # banana
print(fruits[2])  # orange
```

### Lists of Objects

```python
people = {
    {name = "Alice", age = 30},
    {name = "Bob", age = 25},
    {name = "Charlie", age = 35}
}

print(people[0].name)  # Alice
print(people[1].age)   # 25
```

**Important:** You cannot mix list syntax and object syntax in the same literal:
```python
# ❌ ERROR: Cannot mix types
invalid = {
    a = 1,
    {1, 2, 3}  # Error!
}

# ✅ Correct: Choose one
list = {1, 2, 3}
obj = {a = 1, b = 2}
```

---

## Property Access

### Member Access (Dot Notation)

```python
person = {name = "Alice", age = 30}
print(person.name)
print(person.age)
```

### Index Access (Bracket Notation)

```python
list = {10, 20, 30}
print(list[0])
print(list[1])

x = 1
print(list[x])
```

### Chained Access

```python
data = {
    user = {
        profile = {
            name = "Alice"
        }
    }
}

print(data.user.profile.name)
```

### Mixed Access

```python
data = {
    users = {
        {name = "Alice"},
        {name = "Bob"}
    }
}

print(data.users[0].name)  # Alice
```

---

## Data Types

### Numbers

```python
integer = 42
float = 3.14
negative = -10
```

### Strings

See [Strings](#strings) section for details.

### Booleans

```python
isTrue = True
isFalse = False
```

**Note:** Python-style capitalized booleans (`True`/`False`)

### None

```python
empty = None
```

**Transpiles to:** `null`

---

## Expressions

### Arithmetic Operators

```python
x = 10 + 5   # Addition
y = 10 - 5   # Subtraction
z = 10 * 5   # Multiplication
w = 10 / 5   # Division
m = 10 % 3   # Modulo
p = 2 ** 8   # Exponentiation
```

### Augmented Assignment

```python
x = 10
x += 5   # Add and assign
x -= 3   # Subtract and assign
x *= 2   # Multiply and assign
x /= 2   # Divide and assign
```

### String Concatenation

```python
greeting = "Hello, " + "World!"
message = "Count: " + str(42)
```

**Note:** Use `+` operator for string concatenation

### Comparison Operators

```python
a = 10 == 10   # Equal
b = 10 != 5    # Not equal
c = 10 > 5     # Greater than
d = 10 < 5     # Less than
e = 10 >= 10   # Greater than or equal
f = 10 <= 5    # Less than or equal
```

### Logical Operators

```python
a = True and False   # Logical AND
b = True or False    # Logical OR
c = not True         # Logical NOT
```

### Grouping

```python
result = (10 + 5) * 2
check = (x > 5) and (y < 10)
```

---

## Comments

### Single-Line Comments

```python
# This is a single-line comment
x = 10  # Comment after code
```

### Multi-Line Comments

Use triple quotes `"""` for multi-line comments.

```python
"""
This is a multi-line comment.
It can span multiple lines.
Useful for documentation.
"""
x = 10
```

---

## Strings

### Double-Quoted Strings

```python
greeting = "Hello, World!"
message = "She said, \"Hello!\""
```

### Single-Quoted Strings

```python
greeting = 'Hello, World!'
message = 'It\'s a nice day'
```

### Escape Sequences

Both single and double-quoted strings support escape sequences:

```python
newline = "Line 1\nLine 2"
tab = "Column1\tColumn2"
quote = "He said \"Hi\""
backslash = "Path: C:\\Users\\Name"
```

Supported escape sequences:
- `\n` - Newline
- `\t` - Tab
- `\r` - Carriage return
- `\\` - Backslash
- `\"` - Double quote
- `\'` - Single quote
- `\0` - Null character
- `\a` - Bell
- `\b` - Backspace
- `\f` - Form feed
- `\v` - Vertical tab

### Multi-Line Strings

Use triple quotes `"""` for multi-line strings.

```python
text = """
This is a multi-line string.
It preserves line breaks and spacing.
No escape sequences needed for quotes: "Hello" 'World'
"""

a = """
Another multi-line string
with multiple lines
"""
```

**Note:** Triple-quoted strings can also be used for multi-line comments.

---

## Control Flow

### If Statements

```python
if x > 10:
    print("Greater than 10")
elif x > 5:
    print("Greater than 5")
else:
    print("5 or less")
```

### Pass Statement

Use `pass` for empty code blocks.

```python
if condition:
    pass  # Do nothing

def emptyFunction():
    pass
```

### While Loops

```python
i = 0
while i < 5:
    print(i)
    i += 1

# Infinite loop
while True:
    print("Forever")
    break
```

### For Loops

PythonWisp uses a unique for loop syntax with three parts separated by commas:

```python
# Standard for loop: initialization, condition, increment
for i = 0, i < 10, i += 1:
    print(i)

# Loop with global variable
for global i = 0, i < 10, i += 1:
    print(i)

# Loop over list
for i = 0, i < len(obj), i += 1:
    print(obj[i])
```

**Transpiles to:**
```javascript
for (var i:any = 0; i < 10; i += 1) {
    console.log((i));
}
```

**Format:** `for INIT, CONDITION, INCREMENT:`
- **INIT**: Variable initialization (can use `global`)
- **CONDITION**: Loop continuation condition
- **INCREMENT**: Update expression after each iteration

### Break Statement

```python
i = 0
while True:
    print(i)
    i += 1
    if i >= 5:
        break
```

---

## Special Features

### Print Function

Built-in `print` function.

```python
print("Hello, World!")
print(x + y)
print("Value:", x)
```

**Transpiles to:** `console.log(...)`

### Return Statement

```python
def add(a, b):
    return a + b
```

### Self Keyword

`self` is only available inside class methods.

```python
class Dog:
    def init(name):
        self.name = name  # ✅ Valid in class

obj = {
    name = "test",
    greet = lambda: print(obj.name)  # ✅ Use object name
}
```

---

## Reserved Keywords

The following keywords are reserved and cannot be used as variable names:

- `global`
- `def`, `lambda`
- `class`, `implements`
- `self`
- `if`, `elif`, `else`
- `while`, `for`
- `break`, `return`, `pass`
- `and`, `or`, `not`
- `True`, `False`, `None`

---

## Examples

### Complete Example: Todo List

```python
class Todo:
    def init(title):
        self.title = title
        self.completed = False
    
    def toggle():
        self.completed = not self.completed

todos = {
    Todo("Learn PythonWisp"),
    Todo("Build a project"),
    Todo("Deploy to production")
}

# Mark first todo as complete
todos[0].toggle()

# Print all todos
for i = 0, i < 3, i += 1:
    status = "[ ]"
    if todos[i].completed:
        status = "[x]"
    print(status + " " + todos[i].title)
```

---

## Best Practices

1. **Use local variables by default** - Only use `global` when necessary
2. **Prefer classes for complex objects** - Use `self` for better method context
3. **Use object names in object methods** - Since `self` isn't available in object literals
4. **Be consistent with quotes** - Pick single or double quotes and stick with it
5. **Use triple quotes for multi-line text** - Cleaner than escape sequences
6. **Comment your code** - Use `#` for single lines, `"""` for blocks
7. **Zero-index your lists** - Remember lists start at 0
8. **Use `pass` for empty blocks** - Required for empty classes and functions
9. **Use augmented assignment** - `x += 1` instead of `x = x + 1`

---

## Differences from Standard Python

PythonWisp is inspired by Python but has some differences:

1. **Unified `{}` syntax** for both objects and lists (determined by content)
2. **Objects use `=` not `:`** - `{a = 1}` not `{a: 1}`
3. **`self` keyword for classes** instead of explicit self parameter
4. **Classes use `implements`** for inheritance, not Python's class syntax
5. **All variables are local by default** - Use `global` keyword for globals
6. **Unique for loop syntax** - Three-part comma-separated format
7. **No `len()` for lists** - Must implement as registered function
8. **Built-in `print` function** transpiles to `console.log`
9. **`True`/`False`/`None`** transpile to JavaScript equivalents
10. **`pass` statement** for empty blocks (like Python)

---

## Migration from LuaWisp

If you're coming from LuaWisp, here are the key differences:

| Feature | LuaWisp | PythonWisp |
|---------|---------|------------|
| Comments | `--` and `--[[]]` | `#` and `"""` |
| Booleans | `true`/`false`/`nil` | `True`/`False`/`None` |
| Variables | `local x = 10` | `x = 10` (local by default) |
| Global | `x = 10` | `global x = 10` |
| Functions | `function name()` | `def name():` |
| Classes | `this` keyword | `self` keyword |
| Conditionals | `if...then...end` | `if...:` (indentation) |
| Loops | `while...do...end` | `while...:` (indentation) |
| String concat | `..` operator | `+` operator |
| Not equal | `~=` | `!=` |
| Exponent | `^` | `**` |
| Empty blocks | (no keyword) | `pass` |
| Augmented assign | Not available | `+=`, `-=`, `*=`, `/=` |

---
