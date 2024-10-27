# Program Flow and OOP approach
- Some methods are under certain conditions that it can only be evaluated at runtime depends on the input
- control flow statement allows application to selectively execute particular segments of code
## if-statement
- Indentation: are just white space and are not evaluated as part of the execution -> IGNORE
- Pattern matching : Code that first checks if a variable is of a particular type and then immediately casts it to that type is extremely common in the Java world
    for example: .compareTo is defined on Integer, not on Number so we need to check for its type before using the method
    ```java
    void compareIntegers(Number number) {
    if(number instanceof Integer) {
    Integer data = (Integer)number;
    System.out.print(data.compareTo(5));
    }
    }
    ```
    -> become:
    ```java
    void compareIntegers(Number number) {
    if(number instanceof Integer data) {
    System.out.print(data.compareTo(5));
    }
    }
    ```
    -> possible to use with expressions to filter data
    ```java
    void printIntegersGreaterThan5(Number number) {
    if(number instanceof Integer data && data.compareTo(5)>0)
    System.out.print(data);
    }
    ```
    --> The variable data in this example is referred to as the pattern variable
    --> this code also avoids any potential ClassCastException because the cast operation is executed only if the implicit instanceof operator returns true.

    NOTE: 
    1. it is a bad practice to reassign a pattern variable -> can add final modifier -> but it's better not to reassign
    2. The type of the pattern variable must be a subtype of the variable on the left side of the expression.It also cannot be the same type. However this is not a requirement of instanceof without pattern matching
    ```java
    Integer value = 123;
    if(value instanceof Integer) {} //COMPILE
    if(value instanceof Integer data) {} // DOES NOT COMPILE
    ```
    3. Above rule does not apply when we mix classes and interfaces, it still compiles eventhough these two are unrelated
    ```java
    Number value = 123;
    if(value instanceof List) {}
    if(value instanceof List data) {}
    ```
- flow scoping: scope of pattern matching variable defined by the compiler based on the branching and flow of the program.

```java
void printIntegersOrNumbersGreaterThan5(Number number) {
if(number instanceof Integer data || data.compareTo(5)>0)
System.out.print(data);
}
```
--> the above code does not compile because scope of data is only defined if number is an instance of Integer. If it is not, the type of data is undefined, hence the second statment on the right of conditional operator is out of scope. 

Note: be careful with !(NOT operator)
```java
void printOnlyIntegers(Number number) {
if (!(number instanceof Integer data))
return;
System.out.print(data.intValue());
}
```
Above code compiled because the method returns if the input does not inherit Integer -> this means  the input must inherit Integer to reach the last line -> data stays in scope even after the if statement ends. The code is equivalent to
```java
void printOnlyIntegers(Number number) {
if (!(number instanceof Integer data))
return;
else
System.out.print(data.intValue());
}
```
or 
```java
void printOnlyIntegers(Number number) {
if (number instanceof Integer data)
System.out.print(data.intValue());
else
return;
}
```
- The scope ends where the branches end. In this example, re-declaration of pattern matching variable causes compile error
```java
40: void getFish(Object fish) {
41: if (!(fish instanceof String guppy))
42: System.out.print("Eat!");
43: else if (!(fish instanceof String guppy)) {
44: throw new RuntimeException();
45: }
46: System.out.print("Swim!");
47: }
```
## switch-case: stating with Java 14, case values can now be combined
```java
switch(animal) {
case 1,2: System.out.print("Lion");
case 3: System.out.print("Tiger");
}
```
Equivalent code before java 14
```java
switch(animal) {
case 1: case 2: System.out.print("Lion");
case 3: System.out.print("Tiger");
}
```
- break: terminates the switch statement and returns flow control to the enclosing process. Without break statements in each branch, the order of case and default statements is now extremely important because the code will executes in order of code flow each case and default statement
- fall-throgh behaviour: when a case in a switch statement is executed, and then execution continues into the next case(without break or return statement!!). All subsequent cases are executed sequentially, regardness of whether they match the switch expression or not. This continue until a break, return or the end of the switch block is encountered. 

---> always use break/return statemenet at the end of each case UNLESS it's intentionally used to group cases with the same behaviour
```java
6: String instrument = "violin";
7: final String CELLO = "cello";
8: String viola = "viola";
9: int p = -1;
10: switch(instrument) {
11: case "bass" : break;
12: case CELLO : p++;
13: default: p++;
14: case "VIOLIN": p++;
15: case "viola" : ++p; break;
16: }
17: System.out.print(p);
```
OUTPUT: 2

- switch data types: 
    ->  byte,short,int and char and their wrapper classes, String, enum, var( if the type resolves to one of the preceding types ). 
    NOTE: boolean, long, float are either having too small a range of value or too wide range of values so they are not allowed
    
- Acceptable case values: only literals, enum constants, for final constant variable of the same data type as switch data type

-->  the values in each case statement must be **compile-time constant values** of the same data type as the switch values. Final variables, enum constants, and literals are generally resolved at compile-time rather than runtime so only use these as values in case statements. 
```java
final int getCookies() { return 4; }
void feedAnimals() {
final int bananas = 1;
int apples = 2;
int numberOfAnimals = 3;
final int cookies = getCookies();
switch(numberOfAnimals) {
case bananas:
case apples: // DOES NOT COMPILE - not a final variable
case getCookies(): // DOES NOT COMPILE - not evaluated until runtime so they are not allowed as case values
case cookies : // DOES NOT COMPILE - has to be constant
case 3 * 5 :
} }
```
- Switch expression( Java 14 and beyond) : 
    * Result of a switch expression can be assigned to a variable result. For this to work, all case and default branches must return a data type that is compatible with the assignment.
    * arrow operator is commonly used in lambda expressions, when it is used in a switch expression, the case branches are not lambdas. A switch expression supports both an expression and a block in the case and default branches.Block statement must includes a yield statement if the switch expression returns a value 

    -> The yield keyword is equivalent to a return statement within a switch expression and is used to avoid ambiguity about whether you meant to exit the block or method around the switch expression. Yield statement must be there in all if-else cases inside each case of the switch statement.

    ```java
    int fish = 5;
    int length = 12;
    var name = switch(fish) {
    case 1 ->"Goldfish";
    case 2 ->{yield "Trout";}
    case 3 ->{
        if(length > 10) yield "Blobfish";
        else yield "Green";
    }
    default ->"Swordfish";
    };
    ```
    ```java
    int fish = 5;
    int length = 12;
    var name = switch(fish) {
    case 1 ->"Goldfish";
    case 2 ->{} // DOES NOT COMPILE
    case 3 ->{
    if(length > 10) yield "Blobfish";
    } // DOES NOT COMPILE if length <=10 there wont be any yield statment to return a specific value
    default ->"Swordfish";
    };
    ```
    * a semicolon is required after each case expression, but cannot be used with case blocks. Each case or default expression requires a semicolon as well as the assignment itself.
    
    ```java
    var result = switch(bear) {
    case 30 ->"Grizzly";
    default ->"Panda";
    };
    ```
    ```java
    var name = switch(fish) {
    case 1 ->
    "Goldfish" // DOES NOT COMPILE (missing semicolon)
    case 2 ->{yield "Trout";}; // DOES NOT COMPILE (extra semicolon outside the block)
    ...
    } // DOES NOT COMPILE (missing semicolon)
    ```

    * we don’t have to worry about break statements, since only one branch is executed.
    ```java
    public void printSeason(int month) {
    switch(month) {
    case 1, 2, 3 ->System.out.print("Winter");
    case 4, 5, 6 ->System.out.print("Spring");
    case 7, 8, 9 ->System.out.print("Summer");
    case 10, 11, 12 ->System.out.print("Fall");
    } }
    ```
    --> Calling printSeason(2) prints the single value Winter. This time we don’t have to worry about break statements, since only one branch is executed. Return type of this switch expression is void -> it cannot be assigned to a variable

    * A default branch is required unless all cases are covered or no value is returned( like above).a switch expression that returns a value must handle all possible input values.
        NOTE: when you use enum, its possible that additional value can be added later. This will lead to uncomplied code --> always include a default branch in every switch expression, even those that involve enum values. 
NOTE: summary of the key advantages of switch expressions compared to traditional switch statements
1. Exhaustiveness:
Switch expressions require that all possible cases are covered, or a default case is provided.
This helps prevent bugs by ensuring no cases are accidentally omitted.
2. No fall-through:
Each case in a switch expression is self-contained and doesn't fall through to the next case.
This eliminates a common source of errors in traditional switch statements.
3. Ability to assign to a variable:
Switch expressions can be directly assigned to a variable or used as part of a larger expression.
This allows for more concise and expressive code.
4. Compatible with pattern matching:
Switch expressions work well with pattern matching, allowing for more complex and flexible case conditions.
This includes type patterns, property patterns, and relational patterns.
* Type patterns: with binding variable, null handling
```java
public String describeObject(Object obj) {
    return switch (obj) {
        case String s -> "A String of length " + s.length();
        case Integer i -> "An Integer with value " + i;
        case Long l -> "A Long with value " + l;
        case Double d -> "A Double with value " + d;
        case List<?> list -> "A List with " + list.size() + " elements";
        case Point p -> "A Point at (" + p.x + ", " + p.y + ")";
        case null -> "A null value";
        default -> "An object of type " + obj.getClass().getSimpleName();
    };
}
//USING THE METHOD:
Object[] objects = {
    "Hello", 42, 3.14, new ArrayList<>(), new Point(5, 10), null, new Date()
};

for (Object obj : objects) {
    System.out.println(describeObject(obj));
}
```
* Relational patterns
```java
String describeTemperature(int temperature) {
    return switch (temperature) {
        case 0 -> "Freezing point of water";
        case > 0 && < 20 -> "Cool";
        case >= 20 && < 30 -> "Warm";
        case >= 30 && < 40 -> "Hot";
        case >= 40 -> "Very hot";
        case < 0 -> "Below freezing";
        default -> "Invalid temperature";
    };
}

```
* Property patterns
```java
record Point(int x, int y) {}

String describePoint(Point p) {
    return switch (p) {
        case Point(int x, int y) && x == 0 && y == 0 -> "Origin";
        case Point(int x, int y) && x == 0 -> "On Y-axis";
        case Point(int x, int y) && y == 0 -> "On X-axis";
        case Point(var x, var y) -> "Point at (" + x + ", " + y + ")";
    };
}
```
## do-while vs while loop
- Unlike a while loop, do/while execute the statement block first and then check the loop condition. 
## for loop vs for-each loop
### for loop
- Variables declared in initialisation block are accessible only within the for loop
- Components of the for-loop are each optional
```java
for( ; ; )
System.out.println("Hello World"); //COMPILES
```
--> Above codes will print the same statement repeatedly
- Variables in the initialization block must all be of the same type
- It is a poor coding practice to modify loop variables
### for-each loop
- Right side of the for-each loop must be either a built in Java array or an object whose type implements java.lang.Iterable ( collection such as List or Set)
## nested-loop applied for multi-dimentional array
### Optional Labels
- Allow the application flow to jump to it or break from it. for example: OUTER_LOOP: for(..){INNER_LOOP:for(...)}
- Uppercase letter + snake-case
- Position: head of a statement that allows the application flow to jump to or break from it. It is possible to add to control or block statements but its uncommon
- Extremely useful for nested structure, using with break statement
```java
optionalLabel: while(booleanExpression) {
// Body
// Somewhere in the loop
break optionalLabel;
}
```
--> allows us to break out of a higher-level outer loop

### Continue statement
- It ends the current iteration of the loop (skip to next interation)
- As break statement, it can be used with optional labels
- Can not be used with switch-case
## Return statement
- Use instead of break

NOTE: Unreachable code is any code placed immediately after break, continue or return. If unreachable code is present, it will lead to compilation error.

