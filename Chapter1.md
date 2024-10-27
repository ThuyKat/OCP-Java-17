* Java building blocks are classes/interfaces -> interfaces is in chapter 5. 
* An object is a runtime instance of a class.All objects of all different classes represent state of a program
* Two components of a class: methods -> functions; fields -> variables
   ->> keywords: class, public, void, parameters
   ->> method signature: full declaration of a method includes: return type, parameters, method's name, access modifier
* a main() method: gateway between startup of java process and programmer'code. 
* compile java class and run commands : javac file.class -->  java file
* java package, imports, naming conflict (e.g java.Util.Date)
        --> import statement tells Java which packages to look for a class.
        --> only java.lang is a special class that it's automatically imported
        --> packages allow us to use the same name for different classes. For example: java.util.Date and java.sql.Date. In case both are required in code, we use fully qualified class name(i.e java.util.Date) in variable declaration
* constructor: initialize fields, no return type 
* block of code, field vs constructors: which executed first?
    -->Fields and instance initializer blocks are run in the order in which they appear in the file.
    -->The constructor runs after all fields and instance initializer blocks have run.
    ```java
    public class Chick {

    private String name = "Fluffy";
    { System.out.println("setting field"); } 

    public Chick() {
        name = "Tiny";
        System.out.println("setting constructor"); 
    }
    public static void main(String[] args) { 
        Chick chick = new Chick();
        System.out.println(chick.name); } }
    ```
    OUTPUT: setting field
        setting constructor
        Tiny

* primitive type of data (8 types) -> recognize valid values that can be assigned to numbers
* reference type: can be assigned null which means the variable is currently not refering to any object, when it is not null, it can be used to call methods
* variable declaration: declare many variables in the same declaration as long as they are all of the same type
* conventions for identifier names:
   --> Method and variables names begin with a lowercase letter followed by CamelCase.
   --> Class names begin with an uppercase letter followed by CamelCase. Don’t start any identifiers with $. The compiler uses this symbol for some files.
   --> allowed: A-Za-z0-9_$, 0-9 cannot be the first character.
* Multiple classes can be defined in the same file, but only one of them is allowed to be public whose name matches the name of the file

* garbage collector: process of automatically freeing memory on the HEAP by deleteing objects that are no longer reachable in your program. 
    --> method called System.gc() suggests Java to run garbage collection but Java can ignore it. 
    Garbage collection typically runs when:
        1. The heap memory is full or nearly full
        2. The JVM decides it's an optimal time based on its algorithms
    Objects become candidates for garbage collection when the code is no longer reachable in 2 situations: 
        1. the object no longer has any references pointing to it/variable that has been declared or passed to a method or returned from a method that can be used to access the content of an object --> if an object sits on the heap and does not have a name -> no longer accessible--> it will become garbage and needs to be cleared out of the heap. NOTE: its the object which has got collected, not the variable/reference. 
        2. All reference to the object have gone out of scope: local variable -> when method ends, class variable -> till end of program or unless the class itself is unloaded, instance variable -> when their containing object becomes unreachable.
NOTE: 

        - Garbage collection is concerned with objects on the heap, not primitive values on the stack, hence System.gc() does not directly collect primitive values. When you reassign a primitive variable, you are simply changing the value stored in that variable's memory location. There's no object to be garbage collected.

        - When a primitive variable goes out of scope, its memory is auto reclaimed as part of normal stack management, not through garbage collection
* Object-oriented: all code is defined in classes and it is accessed via instantiation of class object.
* Java code get compiled only once though it's possible to write code that does not run everywhere
* Compared to C++: more secured since it runs inside JVM, get rid of operator overloading, better at memory management via auto garbage collector. 

# Scope of variable: 

1. class variable: static variable outside method, inside a class,
2. local variable: variable inside a method or passed to method as argument,
3. instance variable: non-static variable outside method inside a class

    NOTE: Local variables (variables declared within methods) do not receive default values.
They must be explicitly initialized before use, or the compiler will generate an error.

* var identifier(not a keyword) : local variable type reference. Only use for local variable, the compiler will infer the type of variable for you, so re-assign to different type is not allowed. This is different to Javascript, In Java, var is still a specific type defined at compile time. It does not change type at runtime.

    NOTE: Remember that for local variable type inference, the compiler looks only at the line with the declaration.If values are not assigned/ have null values on the line where they are defined, the code is not compiled

    NOTE: var cannot be used in multiple variable declaration

    NOTE: var is not a reserved word and allowed to be used as an identifier.

    NOTE: It is considered a reserved type name. A reserved type name means it cannot be used to define a type, such as a class, interface, or enum.

* text-block: is a multiple line string -> we dont need to use \" or \n any more

    -> incidential whitespace: extra spaces/tabs/line breaks that are added for formatting but not essential to the contents, hence is auto removed or adjusted when processing text blocks
    -> essential whitespace: part of content, is not removed

    NOTE: Text blocks require a line break after the opening """
    -> String block = """doe"""; // DOES NOT COMPILE 

    NOTE: the \ tells Java not to add a new line but join the current line with the next line, effectively removing the line break

    NOTE:the closing """ on a separate line in a text block leads to an extra blank line at the end of the resulting string

    NOTE: The formatted() method can be used with text blocks, similar to String.format().
Text blocks produce java.lang.String objects, so all regular String methods are applicable

    NOTE: the line breaks are part of the string's content, but they are implicitly added by the text block, not explicitly written as \n --> \n is included in length of textblock

