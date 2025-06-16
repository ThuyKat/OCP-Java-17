# Java Building Blocks - Study Notes

## 🏗️ Java Building Blocks

**Java building blocks are classes/interfaces** (interfaces covered in chapter 5)

- **Object**: A runtime instance of a class
- All objects of all different classes represent the state of a program

### Components of a Class

1. **Methods** → functions
2. **Fields** → variables

**Keywords**: `class`, `public`, `void`, `parameters`

**Method signature**: Full declaration includes:
- Return type
- Parameters  
- Method's name
- Access modifier

---

## 🚀 Main Method

**main() method**: Gateway between startup of Java process and programmer's code.

### Compilation and Execution
```bash
javac file.java  # Compile
java file        # Run
```

---

## 📦 Packages and Imports

- **Import statement**: Tells Java which packages to look for a class
- **java.lang**: Special package that's automatically imported
- **Packages**: Allow us to use the same name for different classes

### Naming Conflicts
Example: `java.util.Date` and `java.sql.Date`

When both are required in code, use **fully qualified class name**:
```java
java.util.Date myDate = new java.util.Date();
```

---

## 🏗️ Constructors

- **Purpose**: Initialize fields
- **No return type**
- **Execution order**: Fields and instance initializer blocks run first, then constructor

### Execution Order Example

```java
public class Chick {
    // Step 1: Field initialization
    private String name = "Fluffy";

    // Step 2: Instance initializer block  
    { System.out.println("setting field"); } 

    // Step 3: Constructor execution
    public Chick() {
        name = "Tiny";
        System.out.println("setting constructor"); 
    }
    
    public static void main(String[] args) { 
        Chick chick = new Chick();
        System.out.println(chick.name); 
    } 
}
```

**OUTPUT**:
```
setting field
setting constructor
Tiny
```

**Rule**: 
1. Fields and instance initializer blocks run in the order they appear in the file
2. Constructor runs after all fields and instance initializer blocks

---

## 📊 Data Types

### Primitive Types (8 types)
- Recognize valid values that can be assigned to numbers
- Stored on the stack

### Reference Types
- Can be assigned `null` (variable not referring to any object)
- When not null, can be used to call methods
- Stored on the heap

### Variable Declaration
- Can declare many variables in same declaration if they're all the same type
```java
int a, b, c;  // Valid
int x, String y;  // Invalid - different types
```

---

## 🏷️ Naming Conventions

### Identifier Rules
- **Method and variable names**: Begin with lowercase letter followed by CamelCase
- **Class names**: Begin with uppercase letter followed by CamelCase
- **Don't start with $**: Compiler uses this symbol for some files
- **Allowed characters**: A-Za-z0-9_$
- **0-9 cannot be first character**

---

## 📁 File Structure

**Multiple classes can be defined in the same file**, but:
- Only **one** can be `public`
- Public class name **must match** the filename

---

## 🗑️ Garbage Collection

**Process**: Automatically freeing memory on the **HEAP** by deleting objects no longer reachable

### Triggering Garbage Collection
- `System.gc()` suggests Java to run GC, but **Java can ignore it**
- GC typically runs when:
  1. Heap memory is full or nearly full
  2. JVM decides it's optimal time based on algorithms

### Objects Become Candidates for GC
1. **No references pointing to it**: Object sits on heap without accessible name
2. **All references gone out of scope**:
   - **Local variable**: When method ends
   - **Class variable**: Till end of program or class unloaded
   - **Instance variable**: When containing object becomes unreachable

### Important Notes
- GC is concerned with **objects on heap**, not **primitive values on stack**
- `System.gc()` does not directly collect primitive values
- When primitive variable goes out of scope, memory auto-reclaimed via stack management

---

## 🎯 Variable Scope

### 1. Class Variable
- **Static variable** outside method, inside a class

### 2. Local Variable  
- Variable inside a method or passed as argument
- **No default values** - must be explicitly initialized

### 3. Instance Variable
- **Non-static variable** outside method, inside a class

---

## 🔄 var Identifier

**Local variable type inference** - compiler infers the type

### Rules
- **Only for local variables**
- **Cannot reassign to different type** (unlike JavaScript)
- **Not a keyword** - can be used as identifier
- **Reserved type name** - cannot define class, interface, or enum named `var`

### Restrictions
```java
// ❌ Cannot be used in multiple variable declaration
var a = 5, b = 10;  // DOES NOT COMPILE

// ❌ Must assign value on declaration line  
var x;  // DOES NOT COMPILE

// ❌ Cannot assign null without type context
var y = null;  // DOES NOT COMPILE
```

---

## 📝 Text Blocks

**Multiple line string** - no need for `\"` or `\n`

### Syntax
```java
String textBlock = """
    This is a 
    text block
    """;
```

### Whitespace Types
- **Incidental whitespace**: Extra formatting spaces/tabs - auto removed
- **Essential whitespace**: Part of content - preserved

### Important Rules

#### ❌ Line Break Required
```java
String block = """doe"""; // DOES NOT COMPILE
```

#### ✅ Line Joining with Backslash
```java
String text = """
    Line 1 \
    Line 2
    """;
// Results in: "Line 1 Line 2"
```

#### Closing Position Affects Output
```java
// Closing """ on separate line adds extra blank line
String withBlankLine = """
    Content
    """;

// Closing """ on same line as content - no extra line  
String noBlankLine = """
    Content""";
```

### Text Block Features
- `formatted()` method works with text blocks (like `String.format()`)
- Produces `java.lang.String` objects
- All regular String methods applicable
- Line breaks are part of string content (`\n` included in length)

---

## 🌟 Java Characteristics

### Object-Oriented
- All code defined in classes
- Accessed via instantiation of class objects

### Platform Independence
- Code compiled once
- Possible to write code that doesn't run everywhere

### Compared to C++
- **More secure**: Runs inside JVM
- **No operator overloading**
- **Better memory management**: Auto garbage collector
