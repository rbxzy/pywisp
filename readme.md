# PyWisp Syntax Reference

Complete syntax guide for the PyWisp DSL (Python-style syntax that transpiles to JavaScript/TypeScript).

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

Variables are scoped to their block by default and transpile to `var` declarations.

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

Use the `global` keyword to add variables to the `globals` object.

```python
global x = 10
global name = "Alice"
```

**Transpiles to:**
```javascript
globals.x = 10;
globals.name = "Alice";
```

**Note:** It's recommended to use local variables for most cases to avoid polluting the global scope.

---

## Functions

### Local Functions

Functions are scoped to their block by default.

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

Use the `global` keyword to add functions to the `globals` object.

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

### Function Expressions

Functions can be assigned to variables as expressions.

```python
sayHello = def(name):
    return "Hello, " + name

print(sayHello("Alice"))
```

**Note:** Lambda functions are not supported. Use `def(): pass` for inline function expressions.

---

## Classes and Inheritance

### Basic Class

Classes must have an `init` method (constructor). All class definitions are local by default.

```python
class Person:
    def init(name, age):
        this.name = name
        this.age = age
    
    def greet():
        print("Hi, I'm " + this.name)

person = Person("Alice", 30)
person.greet()
```

**Transpiles to:**
```javascript
function Person(name:any,age:any) {
this.name = name;
this.age = age;
}
Person.prototype.greet = function() {
console.log(("Hi, I'm " + this.name));
};
var person:any = new Person("Alice",30);
person.greet();
```

### Empty Classes

Classes can be declared without methods using `pass`.

```python
class Animal:
    pass

class EmptyClass:
    pass
```

### Class Inheritance

Use `implements` for inheritance (single inheritance only).

```python
class Animal:
    def init(name):
        this.name = name
    
    def speak():
        print(this.name + " makes a sound")

class Dog implements Animal:
    def init(name, breed):
        this.name = name
        this.breed = breed
    
    def speak():
        print(this.name + " barks")

dog = Dog("Buddy", "Golden Retriever")
dog.speak()
```

**Note:** The parent class constructor is automatically called with the same arguments.

### Global Class Instances

Classes and their methods are always local. To expose class instances to other sprite scripts, create a global instance.

```python
# Local class definition
class MyClass:
    def init():
        this.value = 0
    
    def increment():
        this.value += 1

# Create global instance to expose to other scripts
global instance = MyClass()

# Now 'instance' is accessible from other sprite scripts
instance.increment()
print(instance.value)
```

---

## Objects

### Object Literals

Objects use `=` for key-value pairs (not `:` like JavaScript).

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

Methods can reference the object by name (not `this` - that's only for classes).

```python
person = {
    name = "Jim",
    sayName = def():
        print(person.name)
}

person.sayName()
```

**Transpiles to:**
```javascript
var person:any = { name: "Jim", sayName: function() {
console.log((person.name));

} };
person.sayName();
```

---

## Lists

### List Literals

Lists use `{}` braces (not `[]` like standard Python).

```python
numbers = {1, 2, 3, 4, 5}
names = {"Alice", "Bob", "Charlie"}
mixed = {1, "two", 3, True}
```

**Transpiles to:**
```javascript
var numbers:any = [1,2,3,4,5];
var names:any = ["Alice","Bob","Charlie"];
var mixed:any = [1,"two",3,true];
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
# ❌ ERROR: Cannot mix
invalid = {1, 2, name = "test"}

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

### Compound Assignment Operators

```python
x += 5   # x = x + 5
x -= 3   # x = x - 3
x *= 2   # x = x * 2
x /= 4   # x = x / 4
x %= 3   # x = x % 3
```

### String Concatenation

```python
greeting = "Hello, " + "World!"
message = "Count: " + str(42)
```

**Transpiles to:** JavaScript `+` operator

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
```

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

### While Loops

```python
i = 0
while i < 5:
    print(i)
    i = i + 1
```

### While True Loop

```python
i = 0
while True:
    print(i)
    i = i + 1
    if i >= 5:
        break
```

### For Loops

PyWisp uses a unique for loop syntax with three expressions.

**Standard For Loop:**
```python
for i = 0, i < 10, i = i + 1:
    print(i)
```

**With Compound Assignment:**
```python
for i = 0, i < 10, i += 1:
    print(i)
```

**With Global Variable:**
```python
for global i = 0, i < 10, i += 1:
    print(i)
```

**Transpiles to:**
```javascript
for (var i:any = 0; i < 10; i = i + 1) {
    console.log((i));
}
```

### Break Statement

```python
i = 0
while True:
    print(i)
    i = i + 1
    if i >= 5:
        break
```

### Pass Statement

Use `pass` as a placeholder in empty blocks.

```python
if x > 10:
    pass  # Do nothing

class EmptyClass:
    pass  # Empty class definition

def emptyFunction():
    pass  # Empty function
```

---

## Special Features

### Print Statement

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

### This Keyword

`this` is only available inside class methods.

```python
class Dog:
    def init(name):
        this.name = name  # ✅ Valid in class

obj = {
    name = "test",
    greet = def():
        # ❌ Cannot use 'this' here
        print(obj.name)  # ✅ Use object name instead
}
```

---

## Reserved Keywords

The following keywords are reserved and cannot be used as variable names:

- `global`
- `def`
- `class`
- `implements`
- `this`
- `if`, `elif`, `else`
- `while`
- `for`
- `break`, `return`, `pass`
- `and`, `or`, `not`
- `True`, `False`, `None`

---

## Examples

### Complete Example: Todo List

```python
class Todo:
    def init(title):
        this.title = title
        this.completed = False
    
    def toggle():
        this.completed = not this.completed

todos = {
    Todo("Learn PyWisp"),
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

### Complete Example: Counter Class

```python
class Counter:
    def init(start):
        this.value = start
    
    def increment():
        this.value += 1
    
    def decrement():
        this.value -= 1
    
    def reset():
        this.value = 0

counter = Counter(10)
counter.increment()
counter.increment()
print(counter.value)  # 12
counter.reset()
print(counter.value)  # 0
```

---

## Best Practices

1. **Use local variables by default** - Only use `global` when necessary
2. **Use `pass` for empty blocks** - Required in class/function definitions
3. **Prefer classes for complex objects** - Use `this` for better method context
4. **Use object names in object methods** - Since `this` isn't available in object literals
5. **Be consistent with quotes** - Pick single or double quotes and stick with it
6. **Use triple quotes for multi-line text** - Cleaner than escape sequences
7. **Comment your code** - Use `#` for single lines, `"""` for blocks
8. **Zero-index your lists** - Remember lists start at 0, not 1
9. **Use compound assignment operators** - `x += 1` instead of `x = x + 1`
10. **Create global instances for shared classes** - Use `global instance = MyClass()` to expose class instances

---

## Differences from Standard Python

PyWisp is inspired by Python but has some key differences:

1. **Lists use `{}` braces**, not `[]` brackets
2. **Objects use `{key = value}` syntax**, not `{key: value}`
3. **`this` keyword for classes** instead of self parameter
4. **No lambda functions** - Use `def(): pass` for inline functions
5. **`pass` is required** for empty blocks (classes, functions, if statements)
6. **Unified `{}` syntax** for both objects and lists (determined by content)
7. **No list comprehensions** - Use standard for loops
8. **`implements` keyword** for inheritance, not standard Python inheritance
9. **Built-in `print` function** transpiles to `console.log`
10. **`global` keyword** for global scope, not Python's global statement
11. **For loop syntax** uses three expressions like C-style for loops
12. **Booleans are `True`/`False`** (capitalized), `None` instead of `null`
13. **Exponentiation uses `**`** like Python, not `^`
14. **Classes are always local** - must create global instances to share

---

## Syntax Comparison Table

| Feature | PyWisp | Standard Python |
|---------|--------|-----------------|
| Lists | `{1, 2, 3}` | `[1, 2, 3]` |
| Objects | `{a = 1, b = 2}` | `{"a": 1, "b": 2}` |
| Functions | `def name(): pass` | `def name(): pass` |
| Lambda | `def(): pass` | `lambda: None` |
| Classes | `class Name: pass` | `class Name: pass` |
| Inheritance | `implements` | `(ParentClass)` |
| Self | `this` | `self` |
| Global | `global x = 1` | `global x; x = 1` |
| For Loop | `for i = 0, i < 10, i += 1:` | `for i in range(10):` |
| Boolean | `True`, `False` | `True`, `False` |
| Null | `None` | `None` |
| Comments | `#` and `"""` | `#` and `"""` |
| Exponent | `2 ** 8` | `2 ** 8` |

---
