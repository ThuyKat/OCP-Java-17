# Core APIs
- create and manipulate text data with String and StringBuilder
- Math and date/time APIs
- Arrays
## String
1. Concatenating
- The addition operator + is evaluated from left to right -> 1 + 2 + "c" -> 3c
2. Important String methods: ALL RETURNS STRING OBJECT OUTSIDE OF STRING POOL < NOT STRING LITERALS > 
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
##  String pool 
-  collect repetitive string and reuse common ones -> contains literal values and constants appear in your program.
- String Pool, also known as the String constant pool, is a special area in Java heap memory where the JVM stores String literals. This mechanism saves memory by avoiding the creation of dupliate String objects. 
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

->> tell Java to use string pool if the string is present, and add into pool if it is not -->  intern()
```java
var name = "Hello World";
var name2 = new String("Hello World").intern();
System.out.println(name == name2); // true
```
--> The intern() method checks if an equal String exists in  the String pool. If it is exist, returns a reference to that pooled String. If it doesnt, add the String to the pool and return a reference to it. 

```java
15: var first = "rat" + 1; // rat1 in spring pool
16: var second = "r" + "a" + "t" + "1"; // reuse rat1 from spring pool
System.out.println(first == second) //true
```
## Array
area of memory on the heap with space for designated number of elements. 
1. Construct an array
- definite length
- all elements known in advance: int[] moreNumbers = new int[] {42, 55, 99}; OR int[] moreNumbers = {42, 55, 99};
- all of these are valid declaration of array: 
```java
int[] numAnimals;
int [] numAnimals2;
int []numAnimals3;
int numAnimals4[];
int numAnimals5 [];
```
Becareful: 
```java
int[] ids, types; // create two arrays
int ids[], types; // one array and one integer
```
2. Inspecting array
 - length: this is not a method like String's method, but a property - it considers how many slots have been allocated in the memory and not the elements of arrays.
 - equals (): does not look at the elements but only reference of array object
 - Print out array: Arrays.toString(my_array)
3. Transforming array
-  Casting 
```java
3: String[] strings = { "stringValue" };
4: Object[] objects = strings;
5: String[] againStrings = (String[]) objects;
6: againStrings[0] = new StringBuilder(); // DOES NOT COMPILE
7: objects[0] = new StringBuilder(); // Careful! its ok at compile time but throw error at runtime because it's actually String[] as a result of line 4. The Java runtime maintains the actual type of the array (String[] in this case) and performs runtime checks when you try to store elements into it. That's why line 7 compiles (because the compiler only sees Object[]) but fails at runtime (because it's actually still a String[]).
```
---> Use ArrayList<>() instead of array initialization so we dont have to specify the type to avoid this issue
- Sort array: Arrays.sort() -> require importing java.util.Arrays;
- Search array: array needs to be sorted before searching. 
```java
3: int[] numbers = {2,4,6,8};
4: System.out.println(Arrays.binarySearch(numbers, 2)); // 0
5: System.out.println(Arrays.binarySearch(numbers, 4)); // 1
6: System.out.println(Arrays.binarySearch(numbers, 1)); // -1 - should be inserted at index 0, subtract 1 
7: System.out.println(Arrays.binarySearch(numbers, 3)); // -2 - should be inserted at index 1, negate 1 and subtract 1
8: System.out.println(Arrays.binarySearch(numbers, 9)); // -5 - should be inserted at index 4, negate 4 and subtract 1
```
- Compare two arrays : Arrays.compare(arr1,arr2)

--> first, arr1 and arr2 should be comparable that they are of same type -> otw: DOES NOT COMPLILE

--> negative means array1 < array 2 and positive means otherwise

    - arr1[i ]  vs arr2[i ]
    - all elements are same -> compare length 
    - for each compared pair : null< number < uppercase letter < lowercase letter
        - null is small than any other value
        - numbers are smaller than letters
        - uppercase is smaller than lowercase
- Return the first differ index otw -1 if equal: Arrays.mismatch(arr1,arr2)
4. Passing array as argument:  using varargs as if it were a normal array -> able to apply array's methods to it such as args.length and args[0 ]. 

## Multidimentional Arrays
- Declaration:
```java
int[][] vars1; // 2D array
int vars2 [][]; // 2D array
int[] vars3[]; // 2D array
int[] vars4 [], space [][]; // a 2D AND a 3D array
```
NOTE: at least one array size must be provided on right hand side of the assignment.
- Create asymmetric array
```java
int [][] args = new int[4][];
args[0] = new int[5];
args[1] = new int[3];
```
## Math APIs
- Min, max, round, floor, ceil
- Exponent: pow() -> return double
- Math.random() -> return double value, 0<=value<1
## Dates and Times
We need to import the Java time class:  import java.time.*;
1.  checking date time
```java
System.out.println(LocalDate.now()); // 2024-10-27
System.out.println(LocalTime.now());//09:13:07.768
System.out.println(LocalDateTime.now());//2021–10–25T09:13:07.768
System.out.println(ZonedDateTime.now());//2021–10–25T09:13:07.769–05:00[America/New_York ]
```

2.  Create date and time
```java
var date1 = LocalDate.of(2022, Month.JANUARY, 20);
var date2 = LocalDate.of(2022, 1, 20);
var time1 = LocalTime.of(6, 15); // hour and minute
var time2 = LocalTime.of(6, 15, 30); // + seconds
var time3 = LocalTime.of(6, 15, 30, 200); // + nanoseconds
var dateTime1 = LocalDateTime.of(2022, Month.JANUARY, 20, 6, 15, 30);
var dateTime2 = LocalDateTime.of(date1, time1);
```
3.  getting zone
```java
var zone = ZoneId.of("US/Eastern");
```

NOTE: The date and time classes have private constructors along with static methods that return instances. This is known as the factory pattern. Traditional factories are separate classes, but static factory methods are within the class they create. 
Example : 
```java
public class Shop {
    private String type;

    private Shop(String type) {
        this.type = type;
    }

    public static Shop createShoeShop() {
        return new Shop("shoe");
    }

    public static Shop createBakeryShop() {
        return new Shop("bakery");
    }
}
```

4.  Adding/ Go forward in time
```java
12: var date = LocalDate.of(2022, Month.JANUARY, 20);
13: System.out.println(date); // 2022–01–20
14: date = date.plusDays(2);
15: System.out.println(date); // 2022–01–22
16: date = date.plusWeeks(1);
17: System.out.println(date); // 2022–01–29
18: date = date.plusMonths(1);
19: System.out.println(date); // 2022–02–28
20: date = date.plusYears(5);
21: System.out.println(date); // 2027–02–28
```
NOTE: Java acknowledge leap year. For example, Java realizes that February 29, 2022 does not exist, and it gives us February 28, 2022, instead
5.  Go backward in time
```java
22: var date = LocalDate.of(2024, Month.JANUARY, 20);
23: var time = LocalTime.of(5, 15);
24: var dateTime = LocalDateTime.of(date, time);
25: System.out.println(dateTime); // 2024–01–20T05:15
26: dateTime = dateTime.minusDays(1);
27: System.out.println(dateTime); // 2024–01–19T05:15
28: dateTime = dateTime.minusHours(10);
29: System.out.println(dateTime); // 2024–01–18T19:15
30: dateTime = dateTime.minusSeconds(30);
31: System.out.println(dateTime); // 2024–01–18T19:14:30
```
6.  Method chaining
```java
var date = LocalDate.of(2024, Month.JANUARY, 20);
var time = LocalTime.of(5, 15);
var dateTime = LocalDateTime.of(date, time)
.minusDays(1).minusHours(10).minusSeconds(30);
```
7. Period
- EPOCH:January 1, 1970, the point from which to calculate number of miliseconds when converting LocalDate and LocalDateTime
- var period = Period.ofMonths(int num): set a time interval of num month
            = Period.ofYears(int num)
            = Period.ofWeeks(int num)
            =Period.ofDays(int num)
            =Period.of(int numOfYears,int numOfMonths, int numOfDays) : every n years, n months, n days
- It's impossible to chain method using Peirod
- Print out format: example: P1Y2M3D. Java omits any measures that are zero -> System.out.println(Period.ofMonths(3)); -> PM3
```java
3: var date = LocalDate.of(2022, 1, 20);
4: var time = LocalTime.of(6, 15);
5: var dateTime = LocalDateTime.of(date, time);
6: var period = Period.ofMonths(1);
7: System.out.println(date.plus(period)); // 2022–02–20
8: System.out.println(dateTime.plus(period)); // 2022–02–20T06:15
9: System.out.println(time.plus(period)); // Exception
```
- Period has methods : 
    - plus(TemporalAmount):  TemporalAmount which includes Period, Duration, ChronoPeriod
    - plusYears(long),plusMonth(long),plusDays(long)
    - minus(TemporalAmount)
    - minusYears(long),minusMonths(long),minusDays(long)

8. Duration : for smaller unit of time 0> it is used with objects that have time. A Duration is stored in hours, minutes, and seconds.
```java
var daily = Duration.ofDays(1); // PT24H
var hourly = Duration.ofHours(1); // PT1H
var everyMinute = Duration.ofMinutes(1); // PT1M
var everyTenSeconds = Duration.ofSeconds(10); // PT10S
var everyMilli = Duration.ofMillis(1); // PT0.001S
var everyNano = Duration.ofNanos(1); // PT0.000000001S
```
NOTE: Duration doesnt have method that take multiple units like Period does
```java
7: var date = LocalDate.of(2022, 1, 20);
8: var time = LocalTime.of(6, 15);
9: var dateTime = LocalDateTime.of(date, time);
10: var duration = Duration.ofHours(6); //instead we can use Duration.of(6,ChronoUnit.HOURS) here
11: System.out.println(dateTime.plus(duration)); // 2022–01–20T12:15
12: System.out.println(time.plus(duration)); // 12:15
13: System.out.println(
14: date.plus(duration)); // UnsupportedTemporalTypeException
```
- TemporalUnit: an interface in java.time.temporal package with only one implementation named ChronoUnit. ChronoUnit is an enum that implements the TemporalUnit interface. 
```java
var daily = Duration.of(1, ChronoUnit.DAYS);
var hourly = Duration.of(1, ChronoUnit.HOURS);
var everyMinute = Duration.of(1, ChronoUnit.MINUTES);
var everyTenSeconds = Duration.of(10, ChronoUnit.SECONDS);
var everyMilli = Duration.of(1, ChronoUnit.MILLIS);
var everyNano = Duration.of(1, ChronoUnit.NANOS);
```
NOTE: ChronoUnit also includes some convenient units such as ChronoUnit.HALF_DAYS to
represent 12 hours.
--> it can be used to identify duration of time between two Temporal values
```java
var one = LocalTime.of(5, 15);
var two = LocalTime.of(6, 30);
var date = LocalDate.of(2016, 1, 20);
System.out.println(ChronoUnit.HOURS.between(one, two)); // 1 - this is a long value, truncate any fractional  value
System.out.println(ChronoUnit.MINUTES.between(one, two)); // 75
System.out.println(ChronoUnit.MINUTES.between(one, date)); // DateTimeException
```
NOTE: Compare with Duration.between()
    - Duration.between() returns Duration object, provides a more precise measurement including nanosecond instead of being truncated like ChronoUnit
    - Can be both positive and negative vs only positive in ChronoUnit
    - More flexible, since it can extract to hours (toHours()), toMinutes(),toMillis(), getSeconds(),getNano() vs ChronoUnit is only stays in pre-defined time units
```java
var now = Instant.now();
// do something time consuming
var later = Instant.now();
var duration = Duration.between(now, later);
System.out.println(duration.toMillis()); // Returns number milliseconds
```
--> it can be used to truncate element of time, for example : seconds
```java
LocalTime time = LocalTime.of(3,12,45);
System.out.println(time); // 03:12:45
LocalTime truncated = time.truncatedTo(ChronoUnit.MINUTES);
System.out.println(truncated); // 03:12
```
NOTE: Period vs Duration
- Duration supports LocalTime, LocalDateTime, ZonedDateTime
- Period supports LocalDate and LocalDateTime, ZonedDateTime
```java
var date = LocalDate.of(2022, 5, 25);
var period = Period.ofDays(1);
var days = Duration.ofDays(1);
System.out.println(date.plus(period)); // 2022–05–26
System.out.println(date.plus(days)); // Unsupported unit: Seconds
```
9. When to use Duration and when to use ChronoUnit??
- Duration allows for more complex time manipulations and calculations. It can be converted to other time units, can have both negative and positive values, provides methods for addition, subtraction, multiplication, and division.(plus(Duration duration),plusDays(long days), plusHours(long hours), plusMinutes(long minutes), plusSeconds(long seconds), plusMillis(long millis), plusNanos(long nanos) )
```java
Duration duration1 = Duration.ofHours(2);
Duration duration2 = Duration.ofMinutes(30);
Duration result = duration1.plus(duration2); // 2 hours 30 minutes
```
- ChronoUnit is simpler for basic time unit conversions and measurements.It is used for simpler whole unit measurement. It doesn't provide these arithmetic operations directly

10. Instant: to run a timer -> use Duration.between(now,later)
```java
var now = Instant.now();
// do something time consuming
var later = Instant.now();
var duration = Duration.between(now, later);
System.out.println(duration.toMillis()); // Returns number milliseconds
```
- ZonedDateTime can be converted into instant: toInstant() -> the Instant returned will turn the time into an Instant of time in GMT and get rids of the time zone.
- LocalDateTime cannot be converted into instant because zone info is required to convert time into GMT time

11. Daylight Saving Time ->  time changes and offset also changes auto by Java

# Exercise notes
1. In multi-dimentional array, only the left most dimension is required to be non-zero. For that reason the below 3D array is legal

```java
Object[][][] cubbies = new Object[3][0][5];

```

2. Best practice: use == for primitive comparisons and equals for object comparisons. Primitive Wrappers classes like Integer, Boolean have overriden equals to compare values. == can be used for object but it compares reference address not the value. 
    NOTE: for String specifically, == works for String literals though it compares reference addresses. It is because of String pool ( equal literal values lead to same reference address), but it does not work for String objects -> hence for consistency always use equals() instead. 

ADDITIONAL: When to use Wrapper Class?
    1. Collections only store objects, hence the need for wrapper primitives
    2. Ability to override equals and hashmap
    3. Extra methods for manipulating and converting. for example  Integer.parseInt(String str)
