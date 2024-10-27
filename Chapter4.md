# Core APIs
- create and manipulate text data with String and StringBuilder
- Math and date/time APIs
- Arrays
## String
1. Concatenating
- The addition operator + is evaluated from left to right -> 1 + 2 + "c" -> 3c
2. Important String methods
- String elements are indexted
- String is immutable 
- Methods to inspect strings: 
    * length(),
    *  charAt(int index) - if index >=length -> StringIndexOutOfBoundsException,
    *  indexOf(int ch/String tr, [int fromIndex]) -> return the first matched index/ if search a string, return the index where the string starts -> never throw exception but -1 if nothing is found
    * startsWith(String prefix), endsWith(String suffix), contains(CharSequence charseq)
    * equals(Object obj)/equalsIgnoreCase(String str)
        - When comparing two objects, the hashCode() method is called first.This is a quick operation that narrows down the potential matches.If the hash codes are different, the objects are immediately considered not equal.If the hash codes are the same, it doesn't necessarily mean the objects are equal.This is where collisions can occur - different objects may have the same hash code.In this case, the equals() method is then used for a more detailed comparison.Calculating hash codes is generally faster than performing a full equality check.By checking hash codes first, many non-equal objects can be quickly eliminated without the need for a more expensive equals() comparison.This approach is particularly important for the efficient operation of hash-based data structures like HashMap and HashSet.These collections use hash codes to organize objects into "buckets" for quick retrieval.
        --> Overriding equals() requires overriding hashCode() to maintain consistency.
        --> Equal objects must have the same hash code, but objects with the same hash code are not necessarily equal.
    * isEmpty(): check if a String has a length of zero
    * isBlank(): check if a String has a length of zero or contains only whitespace
- Methods to extract from a string:
    *  substring(int beginIndex, int endIndex) -> [ beginIndex, endIndex )
- Methods to transform a string:
    *  toUpperCase() / toLowerCase()
    *  public String replace(char oldChar, char newChar)
        OR: public String replace(CharSequence target, CharSequence replacement)  
    ```java
    System.out.println("abcabc".replace('a', 'A')); // AbcAbc - replace char
    System.out.println("abcabc".replace("a", "A")); // AbcAbc - replace string
    ```  
    *  Removing whitespace: whitespace consists of spaces along with the \t (tab) and \n (newline) characters.
    --> The strip() and trim() methods remove whitespace from the beginning and end of a String.The strip() method does everything that trim() does, but it supports Unicode.
    --> stripLeading(): removes whitespace from the beginning of the String and leaves it at the end.
    --> stripTrailing():removes whitespace from the end of the String and leaves it at the beginning.

    * indent(int numberSpaces): adds the number of blank spaces to the beginning of EACH LINE if positive number is passed and remove no of blank space if a negative number is passed; Normalizes line breaks to '\n'; Add extra '\n' at the end if missing
    ```java
    String textBlock = """
    Hello
      World
        Java
    """;

    System.out.println("Original:");
    System.out.println(textBlock);

    System.out.println("With extra indentation:");
    System.out.println(textBlock.indent(4)); // text block when printed ignore all incidental whitespace, if we want to maintain such incidental white space, we can use this method.
    ```
    OUTPUT: 
    ```
    Original:
    Hello
        World
            Java

    With extra indentation:
        Hello
            World
                Java
    ```

    * stripIndent(): remove incidental whitespace when a String was built with concatenation rather than using a text block; normalize line breaks to '\n' but DOES NOT add extra '\n' at the end

    ```java
    var block = """
            a
            b
            c""";
    var concat = " a\n"
    + " b\n"
    + " c";

    System.out.println(block.length()); // 6
    System.out.println(concat.length()); // 9
    System.out.println(block.indent(1).length()); // 10
    System.out.println(concat.indent(-1).length()); // 7
    System.out.println(concat.indent(-4).length()); // 6
    System.out.println(concat.stripIndent().length()); // 6
    
    ```
    * translateEscaptes() :  \ \t -> \t
    * String.format(my_string,args)/ my_string.formatted(args) : %s, %d, %f( displays exactly six digits past the decimal.), %n -> mixing data types cause exceptions at runtime

    ```java

    var name = "James";
    var score = 90.25;
    var total = 100;
    System.out.println("%s:%n Score: %f out of %d"
    .formatted(name, score, total));
     <!-- This prints the following:
    James:
    Score: 90.250000 out of 100  -->

    ```
    NOTE: 90.250000 will be displayed as 90.3 (not 90.2) when passed to format() with %.1f -> method relies on rounding rather than truncating when shortening numbers
    ```java
    var pi = 3.14159265359;
    System.out.format("[%f]",pi); // [3.141593]
    System.out.format("[%12.8f]",pi); // [ 3.14159265] - number before . represents length of formated string and number after . represents length of decimal part 
    System.out.format("[%012f]",pi); // [00003.141593] - blank space if presents are filled with 0 for integral part 
    System.out.format("[%12.2f]",pi); // [ 3.14]
    System.out.format("[%.3f]",pi); // [3.142]
    ```
## StringBuilder class
1. Why StringBuilder??
- Unlike String, StringBuilder is mutable. It is efficient when we want to use a variable of string type to initialise a value that can be changed later in our program:
```java
10: String alpha = "";
11: for(char current = 'a'; current <= 'z'; current++)
12: alpha += current;
13: System.out.println(alpha);
```

-> after 26 iterations through the loop, a total of 27 objects are instantiated, most of which are immediately eligible for garbage collection.
-> Unlike primitives, objects (except for immutable classes like String) can be mutable.You can change the state of an object without creating a new object.
-> use StringBuilder type of data instead: (StringBuffer in old code which supports threads)

```java
15: StringBuilder alpha = new StringBuilder();
16: for(char current = 'a'; current <= 'z'; current++)
17: alpha.append(current);
18: System.out.println(alpha);
```
- Garbage collection is concerned with objects on the heap, not primitive values on the stack, hence System.gc() does not directly collect primitive values. When you reassign a primitive variable, you are simply changing the value stored in that variable's memory location. There's no object to be garbage collected.
- When a primitive variable goes out of scope, its memory is auto reclaimed as part of normal stack management, not through garbage collection
2. Construct instance of StringBuilder
- 3 ways to construct a StringBuilder instance : take no argument, take inital string as argument and take capacity number as argument
3. StringBuilder methods
- Inspecting String : similar to String, methods include: substring() -> return a string not StringBuilder, indexOf, length, charAt
- Transform: return a reference to the current StringBuilder
    - add element at the end : append(String str/int num/char c/etc) 
    - add element at an index: insert(int index,String str/int num/char c/etc) 
    - delete element form start index to end index: delete(int startIndex, int endIndex) - not include endIndex [ )
    - delete element at a specific index: deleteCharAt(int index)
    - delete element from start index to end index and replace with a new string in the position: replace(int startIndex, int endIndex, String newString) ->  endIndex can be > length of string
    - reverse string: reverse()
- Converting to String type: toString()
- equals() is not implemented on StringBuilder class --> reference point is used to compare objects -> convert using toString() to check for equality of value. 
4. String pool : collect repetitive string and reuse common ones -> contains literal values and constants appear in your program.
- If the same string literals are created at compiled time, only one String object is created, hence equalilty. Otherwise even if two strings seems to have equal value at runtime are not actually equal :
```java
var x = "Hello World";
var z = " Hello World".trim();
System.out.println(x == z); // false

```
```java
var x = "Hello World"; // in string pool
var y = new String("Hello World"); // not in string pool
System.out.println(x == y); // false
```

->> tell Java to use string pool if the string is present with intern()
```java
var name = "Hello World";
var name2 = new String("Hello World").intern();
System.out.println(name == name2); // true
```
```java
15: var first = "rat" + 1; // rat1 in spring pool
16: var second = "r" + "a" + "t" + "1"; // reuse rat1 from spring pool
System.out.println(first == second) //true



