# Java Data Structures & APIs Reference

This document serves as a comprehensive reference for Java data structures, APIs, and related concepts, combining all information from **JAVA-Cheatsheet.md** and **Java_Data_Structures_and_APIs_Reference.markdown** into a single, detailed, and readable Markdown file. All content from both sources is preserved, with additional details and examples added for clarity and completeness.

---

# Java Data Structures - Common Pitfalls & Correct Usage

## 1. List Interface & Implementations

### Return Type Compatibility
```java
// ❌ WRONG - Cannot return more specific generic type
public List<List<Integer>> getMatrix() {
    return new ArrayList<ArrayList<Integer>>(); // Compilation Error
}

// ✅ CORRECT - Return compatible type
public List<List<Integer>> getMatrix() {
    return new ArrayList<List<Integer>>(); // Works perfectly
}

// ✅ CORRECT - Alternative with diamond operator
public List<List<Integer>> getMatrix() {
    return new ArrayList<>(); // Type inference works
}
```

### ArrayList vs List Declaration
```java
// ❌ WRONG - Tight coupling to implementation
ArrayList<String> names = new ArrayList<>(); // Limits flexibility

// ✅ CORRECT - Program to interface
List<String> names = new ArrayList<>(); // Can switch implementations easily
```

### Raw Types Usage
```java
// ❌ WRONG - Raw type, no type safety
List list = new ArrayList(); // Compiler warning, runtime ClassCastException risk

// ✅ CORRECT - Parameterized type
List<String> list = new ArrayList<>(); // Type safe
```

### Initialization Pitfalls
```java
// ❌ WRONG - Null initialization
List<Integer> numbers = null; // NullPointerException waiting to happen

// ✅ CORRECT - Proper initialization
List<Integer> numbers = new ArrayList<>(); // Safe to use immediately
```

## 2. Set Interface & Implementations

### HashSet vs LinkedHashSet vs TreeSet
```java
// ❌ WRONG - Using HashSet when order matters
Set<String> orderedNames = new HashSet<>(); // No guaranteed order

// ✅ CORRECT - Use LinkedHashSet for insertion order
Set<String> orderedNames = new LinkedHashSet<>(); // Maintains insertion order

// ✅ CORRECT - Use TreeSet for sorted order
Set<String> sortedNames = new TreeSet<>(); // Natural ordering
```

### equals() and hashCode() Pitfall
```java
// ❌ WRONG - Custom object without proper equals/hashCode
class Person { String name; } // HashSet won't work correctly
Set<Person> people = new HashSet<>(); // Duplicates possible

// ✅ CORRECT - Override equals and hashCode
class Person { 
    String name; 
    @Override public boolean equals(Object o) { /* implementation */ }
    @Override public int hashCode() { /* implementation */ }
}
```

## 3. Map Interface & Implementations

### Generic Type Specification
```java
// ❌ WRONG - Raw types
Map map = new HashMap(); // No type safety

// ✅ CORRECT - Proper generics
Map<String, Integer> scores = new HashMap<>(); // Type safe
```

### Null Key/Value Handling
```java
// ❌ WRONG - Assuming all Maps handle nulls
Map<String, String> config = new ConcurrentHashMap<>();
config.put(null, "value"); // Throws NullPointerException

// ✅ CORRECT - Use HashMap for null keys
Map<String, String> config = new HashMap<>();
config.put(null, "value"); // Works fine
```

### Key Mutability Pitfall
```java
// ❌ WRONG - Using mutable objects as keys
List<String> key = new ArrayList<>();
Map<List<String>, String> map = new HashMap<>();
map.put(key, "value");
key.add("item"); // Key changed, value now unreachable!

// ✅ CORRECT - Use immutable keys
String key = "immutableKey";
Map<String, String> map = new HashMap<>();
map.put(key, "value"); // Safe
```

## 4. Queue Interface & Implementations

### Queue vs Deque
```java
// ❌ WRONG - Using wrong interface for double-ended operations
Queue<Integer> queue = new ArrayDeque<>();
// queue.addLast(5); // Method doesn't exist in Queue interface

// ✅ CORRECT - Use Deque for double-ended operations
Deque<Integer> deque = new ArrayDeque<>();
deque.addLast(5); // Works perfectly
```

### PriorityQueue Pitfalls
```java
// ❌ WRONG - Expecting FIFO behavior
Queue<Integer> pq = new PriorityQueue<>(); // Actually a heap!
pq.offer(3); pq.offer(1); pq.offer(2);
// pq.poll() returns 1, not 3 (min-heap behavior)

// ✅ CORRECT - Understanding heap behavior
PriorityQueue<Integer> pq = new PriorityQueue<>(); // Min-heap by default
```

### Null Elements
```java
// ❌ WRONG - Adding null to PriorityQueue
PriorityQueue<String> pq = new PriorityQueue<>();
pq.offer(null); // Throws NullPointerException

// ✅ CORRECT - Avoid nulls in PriorityQueue
PriorityQueue<String> pq = new PriorityQueue<>();
pq.offer("valid"); // Works fine
```

## 5. Stack vs Deque

### Deprecated Stack Class
```java
// ❌ WRONG - Using legacy Stack class
Stack<Integer> stack = new Stack<>(); // Extends Vector, synchronized overhead

// ✅ CORRECT - Use Deque as stack
Deque<Integer> stack = new ArrayDeque<>(); // Better performance
```

## 6. Arrays Utility Pitfalls

### asList() Pitfall
```java
// ❌ WRONG - Trying to modify Arrays.asList() result
List<String> list = Arrays.asList("a", "b", "c");
list.add("d"); // Throws UnsupportedOperationException

// ✅ CORRECT - Create mutable list
List<String> list = new ArrayList<>(Arrays.asList("a", "b", "c"));
list.add("d"); // Works fine
```

### Primitive Arrays
```java
// ❌ WRONG - Arrays.asList with primitive array
int[] arr = {1, 2, 3};
List<int[]> list = Arrays.asList(arr); // Creates List<int[]>, not List<Integer>

// ✅ CORRECT - Use wrapper array or streams
Integer[] arr = {1, 2, 3};
List<Integer> list = Arrays.asList(arr); // Correct behavior
```

## 7. Collections Utility Pitfalls

### Unmodifiable vs Immutable
```java
// ❌ WRONG - Thinking unmodifiable means immutable
List<StringBuilder> original = new ArrayList<>();
original.add(new StringBuilder("test"));
List<StringBuilder> unmod = Collections.unmodifiableList(original);
unmod.get(0).append("modified"); // Still modifies the content!

// ✅ CORRECT - Understanding the difference
// Unmodifiable prevents structural changes, not content changes
```

### Empty Collections
```java
// ❌ WRONG - Creating new empty collections repeatedly
List<String> empty1 = new ArrayList<>(); // Unnecessary object creation
List<String> empty2 = new ArrayList<>();

// ✅ CORRECT - Use Collections constants
List<String> empty1 = Collections.emptyList(); // Singleton, efficient
List<String> empty2 = Collections.emptyList();
```

## 8. Iterator Pitfalls

### ConcurrentModificationException
```java
// ❌ WRONG - Modifying collection while iterating
List<String> list = new ArrayList<>(Arrays.asList("a", "b", "c"));
for (String item : list) {
    if (item.equals("b")) {
        list.remove(item); // Throws ConcurrentModificationException
    }
}

// ✅ CORRECT - Use iterator's remove method
List<String> list = new ArrayList<>(Arrays.asList("a", "b", "c"));
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if (it.next().equals("b")) {
        it.remove(); // Safe removal
    }
}
```

## 9. Generics Wildcards

### Upper Bounded Wildcards
```java
// ❌ WRONG - Cannot add to upper bounded wildcard
List<? extends Number> numbers = new ArrayList<Integer>();
numbers.add(5); // Compilation error

// ✅ CORRECT - Can only read from upper bounded wildcards
List<? extends Number> numbers = new ArrayList<Integer>();
Number n = numbers.get(0); // Reading is safe
```

### Lower Bounded Wildcards
```java
// ❌ WRONG - Cannot read specific type from lower bounded wildcard
List<? super Integer> numbers = new ArrayList<Number>();
Integer i = numbers.get(0); // Compilation error

// ✅ CORRECT - Can add to lower bounded wildcards
List<? super Integer> numbers = new ArrayList<Number>();
numbers.add(5); // Adding is safe
```

## 10. Stream API Pitfalls

### Parallel Streams with Non-Thread-Safe Operations
```java
// ❌ WRONG - Using non-thread-safe collection with parallel streams
List<Integer> result = new ArrayList<>(); // Not thread-safe
IntStream.range(0, 1000).parallel().forEach(result::add); // Race conditions

// ✅ CORRECT - Use thread-safe operations
List<Integer> result = IntStream.range(0, 1000)
    .parallel()
    .boxed()
    .collect(Collectors.toList()); // Thread-safe collector
```

### Stream Reuse
```java
// ❌ WRONG - Reusing streams
Stream<String> stream = list.stream();
stream.filter(s -> s.length() > 3).collect(Collectors.toList());
stream.map(String::toUpperCase).collect(Collectors.toList()); // IllegalStateException

// ✅ CORRECT - Create new stream for each operation
list.stream().filter(s -> s.length() > 3).collect(Collectors.toList());
list.stream().map(String::toUpperCase).collect(Collectors.toList());
```

## 11. Memory and Performance Pitfalls

### Initial Capacity
```java
// ❌ WRONG - Not specifying capacity when size is known
List<String> largeList = new ArrayList<>(); // Will resize multiple times
for (int i = 0; i < 10000; i++) {
    largeList.add("item" + i);
}

// ✅ CORRECT - Specify initial capacity
List<String> largeList = new ArrayList<>(10000); // Avoids resizing
for (int i = 0; i < 10000; i++) {
    largeList.add("item" + i);
}
```

### String Concatenation in Loops
```java
// ❌ WRONG - String concatenation in loop
String result = "";
for (int i = 0; i < 1000; i++) {
    result += "item" + i; // Creates new String objects repeatedly
}

// ✅ CORRECT - Use StringBuilder
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("item").append(i);
}
String result = sb.toString();
```

## 12. Thread Safety Pitfalls

### Synchronized vs Concurrent Collections
```java
// ❌ WRONG - Using synchronized wrappers unnecessarily
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
// Still need external synchronization for compound operations

// ✅ CORRECT - Use concurrent collections when needed
List<String> concurrentList = new CopyOnWriteArrayList<>(); // Thread-safe
Map<String, String> concurrentMap = new ConcurrentHashMap<>(); // Better performance
```

## Key Takeaways

1. **Always program to interfaces** (List, Set, Map) rather than implementations
2. **Use proper generics** to ensure type safety
3. **Understand collection characteristics** (ordering, nulls, thread-safety)
4. **Be careful with wildcards** in generics
5. **Don't modify collections during iteration** without using Iterator.remove()
6. **Consider initial capacity** for better performance
7. **Use appropriate collection types** for your use case
8. **Be aware of thread safety** requirements
9. **Understand the difference** between unmodifiable and immutable
10. **Use modern alternatives** (Deque instead of Stack, ConcurrentHashMap instead of Hash

---


# String Conversion in Java: `.toString()` vs `String.valueOf()`

## Primitives (int, float, double, boolean, etc.)

```java
String.valueOf(42);         // ✓ Works fine, converts to "42"
String.valueOf(3.14f);      // ✓ Works fine, converts to "3.14"
String.valueOf(true);       // ✓ Works fine, converts to "true"

42.toString();              // ✗ Won't compile, primitives don't have methods
3.14f.toString();           // ✗ Won't compile, primitives don't have methods
true.toString();            // ✗ Won't compile, primitives don't have methods

Integer.toString(42);       // ✓ Alternative static method for int
Float.toString(3.14f);      // ✓ Alternative static method for float
```

## Wrapper Classes (Integer, Float, Boolean, etc.)

```java
Integer num = 42;
num.toString();             // ✓ Works fine, instance method
String.valueOf(num);        // ✓ Works fine, unwraps and converts

Integer nullInt = null;
nullInt.toString();         // ✗ NullPointerException
String.valueOf(nullInt);    // ✓ Returns "null" (as a string)
```

## Objects

```java
Object obj = new Object();
obj.toString();             // ✓ Works fine, calls object's toString() method
String.valueOf(obj);        // ✓ Works fine, internally calls obj.toString()

Object nullObj = null;
nullObj.toString();         // ✗ NullPointerException
String.valueOf(nullObj);    // ✓ Returns "null" (as a string)
```

## StringBuilder/StringBuffer

```java
StringBuilder sb = new StringBuilder("hello");
sb.toString();              // ✓ Preferred, directly returns the string
String.valueOf(sb);         // ✓ Works but less efficient (calls toString() internally)
```

## Arrays

```java
int[] array = {1, 2, 3};
array.toString();           // ✗ Wrong result, returns something like "[I@1a2b3c" (memory address)
String.valueOf(array);      // ✗ Wrong result, also returns memory address
Arrays.toString(array);     // ✓ Correct way to convert arrays to string
```

## Null Handling

```java
String.valueOf(null);       // ✓ Returns "null" (as a string)
null.toString();            // ✗ NullPointerException
```

## Best Practices

- Use `String.valueOf()` when the value might be null
- Use `.toString()` when you're sure the value is not null
- For primitives, both `String.valueOf()` and wrapper class's static `toString()` methods work well
- For arrays, always use `Arrays.toString()` or `Arrays.deepToString()`
- For `StringBuilder`/`StringBuffer`, prefer direct `.toString()`


---

# Java Number Parsing and Conversion Guide

This guide covers common methods for parsing strings to numbers and converting between numeric types in Java, including best practices and common pitfalls.

## String to Primitive: Parsing Methods

### Integer (String to int)

```java
int num1 = Integer.parseInt("123");      // ✓ Returns primitive int (123)
int num2 = Integer.parseInt("  123  ");  // ✓ Handles whitespace automatically
int num3 = Integer.parseInt("123.45");   // ✗ NumberFormatException - no decimals allowed
int num4 = Integer.parseInt("123A");     // ✗ NumberFormatException - no letters allowed
int num5 = Integer.parseInt(null);       // ✗ NullPointerException
int num6 = Integer.parseInt("");         // ✗ NumberFormatException - empty string not allowed
```

### Long (String to long)

```java
long num1 = Long.parseLong("123");             // ✓ Returns primitive long (123L)
long num2 = Long.parseLong("9223372036854775807"); // ✓ Max long value
long num3 = Long.parseLong("9223372036854775808"); // ✗ NumberFormatException - exceeds max value
```

### Float (String to float)

```java
float num1 = Float.parseFloat("123.45");    // ✓ Returns primitive float (123.45f)
float num2 = Float.parseFloat("123");       // ✓ Works with integers too (123.0f)
float num3 = Float.parseFloat("1.23e2");    // ✓ Works with scientific notation (123.0f)
float num4 = Float.parseFloat("Infinity");  // ✓ Special value Float.POSITIVE_INFINITY
float num5 = Float.parseFloat("NaN");       // ✓ Special value Float.NaN (Not a Number)
```

### Double (String to double)

```java
double num1 = Double.parseDouble("123.45");   // ✓ Returns primitive double (123.45d)
double num2 = Double.parseDouble("1.23e10");  // ✓ Works with scientific notation (12300000000.0d)
```

### Boolean (String to boolean)

```java
boolean b1 = Boolean.parseBoolean("true");   // ✓ Returns true
boolean b2 = Boolean.parseBoolean("TRUE");   // ✓ Case insensitive, returns true
boolean b3 = Boolean.parseBoolean("false");  // ✓ Returns false
boolean b4 = Boolean.parseBoolean("yes");    // ✓ Returns false (anything besides "true" is false)
boolean b5 = Boolean.parseBoolean(null);     // ✓ Returns false (doesn't throw exception)
```

## String to Wrapper: valueOf Methods

### Integer.valueOf()

```java
Integer num1 = Integer.valueOf("123");    // ✓ Returns Integer object (boxed int)
Integer num2 = Integer.valueOf(123);      // ✓ Can also convert from primitive
Integer num3 = Integer.valueOf("123", 8); // ✓ Octal conversion (value = 83)
Integer num4 = Integer.valueOf(null);     // ✗ NullPointerException
```

### Long.valueOf()

```java
Long num1 = Long.valueOf("123");          // ✓ Returns Long object (boxed long)
Long num2 = Long.valueOf("123", 16);      // ✓ Hex conversion (value = 291)
```

### Float.valueOf()

```java
Float num1 = Float.valueOf("123.45");     // ✓ Returns Float object (boxed float)
Float num2 = Float.valueOf(123.45f);      // ✓ Can also convert from primitive
```

### Double.valueOf()

```java
Double num1 = Double.valueOf("123.45");   // ✓ Returns Double object (boxed double)
```

## Primitive vs Wrapper Differences

```java
int primitive = 0;                 // Default value is 0
Integer wrapper = null;            // Can be null

int a = Integer.parseInt("123");   // Returns primitive int
Integer b = Integer.valueOf("123"); // Returns Integer object

// Auto-boxing and unboxing
int c = Integer.valueOf("123");    // ✓ Auto-unboxing works
Integer d = Integer.parseInt("123"); // ✓ Auto-boxing works
```

## Numeric Type Conversions

### Widening Conversions (No Data Loss)

```java
byte b = 100;
short s = b;       // ✓ byte to short - automatic
int i = s;         // ✓ short to int - automatic
long l = i;        // ✓ int to long - automatic
float f = l;       // ✓ long to float - automatic (may lose precision for very large values)
double d = f;      // ✓ float to double - automatic
```

### Narrowing Conversions (Potential Data Loss)

```java
double d = 123.45;
float f = (float) d;     // ✓ Requires explicit cast, may lose precision
long l = (long) f;       // ✓ Requires explicit cast, truncates decimal
int i = (int) l;         // ✓ Requires explicit cast if l > Integer.MAX_VALUE
short s = (short) i;     // ✓ Requires explicit cast if i > Short.MAX_VALUE
byte b = (byte) s;       // ✓ Requires explicit cast if s > Byte.MAX_VALUE

int tooLarge = 128;
byte smallByte = (byte) tooLarge; // ✗ Results in -128 (overflow)
```

## Common Pitfalls

### Numeric Overflow

```java
int maxInt = Integer.MAX_VALUE;        // 2147483647
int overflow = maxInt + 1;             // ✗ Becomes -2147483648 (wraps around)

long safeMath = (long) maxInt + 1;     // ✓ Use wider type to prevent overflow
```

### Floating Point Precision

```java
double a = 0.1 + 0.2;                 // ✗ Result is 0.30000000000000004, not exactly 0.3
boolean isEqual = (a == 0.3);         // ✗ This will be false!

// Correct approach for equality
boolean isCorrect = Math.abs(a - 0.3) < 0.0000001;  // ✓ Use epsilon for comparison
```

### Integer Division

```java
int result1 = 5 / 2;                  // ✗ Result is 2, not 2.5 (truncates decimal)
double result2 = 5 / 2;               // ✗ Result is still 2.0 (division happens as int first)
double result3 = 5.0 / 2;             // ✓ Result is 2.5 (one operand is double)
double result4 = (double) 5 / 2;      // ✓ Result is 2.5 (cast to double first)
```

### String Formatting and Conversion

```java
int value = 123;
String str1 = value + "";             // ✓ Works but not recommended (less efficient)
String str2 = String.valueOf(value);  // ✓ Better approach
String str3 = Integer.toString(value); // ✓ Also good

// Formatting with specific format
String formatted = String.format("%.2f", 123.456);  // ✓ "123.46" (rounded)
```

### NaN and Infinity

```java
double result1 = 1.0 / 0.0;           // ✓ Results in Infinity (no exception thrown)
double result2 = 0.0 / 0.0;           // ✓ Results in NaN (Not a Number)

boolean check1 = (result1 == Double.POSITIVE_INFINITY);  // ✓ True
boolean check2 = (result2 == Double.NaN);                // ✗ Always false! NaN is not equal to anything
boolean check3 = Double.isNaN(result2);                  // ✓ Correct way to check for NaN
```

### Parsing Errors

```java
try {
    int value = Integer.parseInt(userInput);  // ✓ Handle potential parsing errors
} catch (NumberFormatException e) {
    // Handle invalid input
}
```

## Best Practices

1. Always use parsing methods (`parseInt()`, `parseDouble()`) when you need primitive types
2. Use `valueOf()` methods when you need wrapper objects
3. Handle potential exceptions when parsing user input
4. Be careful with type conversions that may cause overflow or precision loss
5. Use proper comparison techniques for floating-point values
6. Consider using `BigInteger` and `BigDecimal` for precise calculations with very large numbers
7. When dividing integers, be explicit about when you want floating-point results
8. Use `Math.addExact()`, `Math.subtractExact()`, etc. to catch overflow situations


---



---

# Circular array

| **Concept**                                | **Formula / Trick**                   | **Notes / Use Case**                                      |
|-------------------------------------------|---------------------------------------|-----------------------------------------------------------|
| Next index                                 | `(i + 1) % n`                         | Move one step forward                                     |
| Previous index                             | `(i - 1 + n) % n`                     | Move one step backward                                    |
| Move forward `k` steps                     | `(i + k) % n`                         | General forward movement                                  |
| Move backward `k` steps                    | `(i - k + n) % n`                     | General backward movement                                 |
| Distance forward from `start` to `i`       | `(i - start + n) % n`                | Steps from `start` to `i` in forward direction            |
| Distance backward from `start` to `i`      | `(start - i + n) % n`                | Steps from `start` to `i` in backward direction           |
| Check if `i` is between `a` and `b`        | `(i - a + n) % n <= (b - a + n) % n` | Returns true if `i` lies between `a` and `b` circularly   |
| Circular iteration over full array         | `for (int i = 0; i < n; i++)`        | Use index `(start + i) % n` inside the loop               |
| Double loop for circular problems          | `for (int i = 0; i < 2 * n; i++)`    | Useful for next greater/monotonic stack problems          |
| Simulate circular string                   | `s + s`                              | Helps in circular substring search (e.g., `abcab`)        |




---

# Common Pitfalls with Java Data Types

## Double Pitfalls

### 1. Precision & Equality

```java
// ❌ PITFALL: Direct equality comparison with doubles
double a = 0.1 + 0.2;
double b = 0.3;
if (a == b) {  // WRONG: This will be false!
    System.out.println("Equal");
}
System.out.println(a);  // Outputs: 0.30000000000000004

// ✅ CORRECT: Use epsilon comparison
double epsilon = 0.0000001;
if (Math.abs(a - b) < epsilon) {
    System.out.println("Equal within tolerance");
}

// ✅ ALTERNATIVE: Use BigDecimal for precise decimal arithmetic
BigDecimal x = new BigDecimal("0.1").add(new BigDecimal("0.2"));
BigDecimal y = new BigDecimal("0.3");
if (x.compareTo(y) == 0) {  // Exact comparison
    System.out.println("BigDecimals are equal");
}
```

### 2. Division & Infinity

```java
// ❌ PITFALL: Division by zero
double result = 1.0 / 0.0;  // No exception! Results in Infinity
System.out.println(result);  // Outputs: Infinity

// ❌ PITFALL: NaN behavior
double nan = 0.0 / 0.0;  // Results in NaN
System.out.println(nan);  // Outputs: NaN
System.out.println(nan == nan);  // FALSE! NaN is not equal to anything, even itself

// ✅ CORRECT: Check for special values
if (Double.isInfinite(result)) {
    System.out.println("Result is infinite");
}
if (Double.isNaN(nan)) {
    System.out.println("Result is NaN");
}
```

### 3. Subtractive Cancellation

```java
// ❌ PITFALL: Loss of precision with subtraction of similar numbers
double big = 10000000.0;
double result = (big + 0.1) - big;  // Should be 0.1
System.out.println(result);  // May not be exactly 0.1

// ✅ CORRECT: Rearrange expressions when possible
// Or use BigDecimal for critical calculations
```

## Integer Pitfalls

### 1. Integer Overflow

```java
// ❌ PITFALL: Integer overflow
int max = Integer.MAX_VALUE;
int result = max + 1;  // Silent overflow!
System.out.println(result);  // Outputs: -2147483648 (negative!)

// ✅ CORRECT: Check for potential overflow
if (max > Integer.MAX_VALUE - 1) {
    System.out.println("Operation would overflow");
    // Handle differently, maybe use long
}

// ✅ ALTERNATIVE: Use Math.addExact to throw exception on overflow
try {
    int sum = Math.addExact(max, 1);
} catch (ArithmeticException e) {
    System.out.println("Overflow detected");
}
```

### 2. Division & Modulo

```java
// ❌ PITFALL: Integer division truncates
int result = 5 / 2;
System.out.println(result);  // Outputs: 2 (not 2.5)

// ✅ CORRECT: Convert to double before division if decimal result needed
double properResult = 5.0 / 2;
System.out.println(properResult);  // Outputs: 2.5

// ❌ PITFALL: Unexpected modulo behavior with negative numbers
System.out.println(-5 % 2);  // Outputs: -1
System.out.println(5 % -2);  // Outputs: 1

// ✅ CORRECT: Use Math.floorMod for consistent behavior
System.out.println(Math.floorMod(-5, 2));  // Outputs: 1
```

### 3. Parsing Failures

```java
// ❌ PITFALL: Uncaught NumberFormatException
String input = "123abc";
int num = Integer.parseInt(input);  // Throws exception

// ✅ CORRECT: Validate or use try-catch
try {
    int num = Integer.parseInt(input);
} catch (NumberFormatException e) {
    System.out.println("Invalid number format");
}

// ✅ ALTERNATIVE: Validate with regex first
if (input.matches("\\d+")) {
    int num = Integer.parseInt(input);
}
```

## Float Pitfalls

### 1. Precision Loss

```java
// ❌ PITFALL: Precision loss due to limited bits
float f = 16777216.0f;
System.out.println(f + 1.0f);  // Outputs: 16777216.0 (can't represent the +1)

// ✅ CORRECT: Use double for better precision
double d = 16777216.0;
System.out.println(d + 1.0);  // Outputs: 16777217.0

// ✅ ALTERNATIVE: Use BigDecimal for critical precision
```

### 2. Mixed Type Calculations

```java
// ❌ PITFALL: Implicit conversion can cause precision loss
float f = 0.1f;
double d = 0.1;
System.out.println(f == d);  // FALSE! 0.1f is not exactly equal to 0.1d

// ✅ CORRECT: Convert explicitly when comparing
System.out.println(Math.abs((double)f - d) < 1e-6);
```

### 3. Literal Suffixes

```java
// ❌ PITFALL: Missing 'f' suffix causes double→float conversion
float f = 3.14;  // Compilation error: double cannot be converted to float

// ✅ CORRECT: Use 'f' suffix for float literals
float f = 3.14f;
```

## String Pitfalls

### 1. Equality Comparison

```java
// ❌ PITFALL: Using == with strings
String s1 = "hello";
String s2 = new String("hello");
if (s1 == s2) {  // FALSE! Compares references, not content
    System.out.println("Same");
}

// ✅ CORRECT: Use equals() method
if (s1.equals(s2)) {  // TRUE
    System.out.println("Same content");
}
```

### 2. String Concatenation in Loops

```java
// ❌ PITFALL: Inefficient string concatenation
String result = "";
for (int i = 0; i < 10000; i++) {
    result += i;  // Creates a new String object each time
}

// ✅ CORRECT: Use StringBuilder
StringBuilder builder = new StringBuilder();
for (int i = 0; i < 10000; i++) {
    builder.append(i);
}
result = builder.toString();
```

### 3. Null Handling

```java
// ❌ PITFALL: NullPointerException
String str = null;
if (str.equals("something")) {  // Throws NullPointerException
    // ...
}

// ✅ CORRECT: Check for null or reverse the comparison
if (str != null && str.equals("something")) {
    // ...
}
// OR
if ("something".equals(str)) {  // Safe even if str is null
    // ...
}
```

### 4. String Immutability

```java
// ❌ PITFALL: Not understanding immutability
String str = "hello";
str.toUpperCase();
System.out.println(str);  // Still "hello" - original not changed

// ✅ CORRECT: Assign the result
str = str.toUpperCase();
System.out.println(str);  // Now "HELLO"
```

## Character Pitfalls

### 1. Char vs String Confusion

```java
// ❌ PITFALL: Using quotes incorrectly
char c = "A";  // WRONG - "A" is a String, not char
String s = 'A';  // WRONG - 'A' is a char, not String

// ✅ CORRECT: Single quotes for char, double quotes for String
char c = 'A';
String s = "A";
```

### 2. Implicit Type Conversion

```java
// ❌ PITFALL: Unexpected results with arithmetic
char c = 'A';
c = c + 1;  // Compilation error: cannot convert from int to char

// ✅ CORRECT: Use explicit cast
c = (char)(c + 1);  // 'B'

// ❌ PITFALL: Numeric calculations
char digit = '5';
int value = digit;  // value is 53 (ASCII for '5'), not 5

// ✅ CORRECT: Convert digits to int values
int value = digit - '0';  // Now value is 5
// OR
int value = Character.getNumericValue(digit);  // Also 5
```

### 3. Unicode Issues

```java
// ❌ PITFALL: Assuming char is always one "character"
char c = '😀';  // WRONG - Won't compile, emoji requires two char values

// ✅ CORRECT: Use String for Unicode beyond BMP
String emoji = "😀";  // Works fine
// For processing, use codePoints():
emoji.codePoints().forEach(cp -> System.out.printf("U+%04X ", cp));
```

## Type Pitfalls in Mixed Operations

### 1. Silent Type Promotions

```java
// ❌ PITFALL: Unexpected type promotion
byte b1 = 10;
byte b2 = 20;
byte sum = b1 + b2;  // Compilation error: int cannot be converted to byte

// ✅ CORRECT: Explicit cast back to byte
byte sum = (byte)(b1 + b2);

// ❌ PITFALL: char + int = int, not char
char c = 'A';
int i = 1;
char next = c + i;  // Compilation error: int cannot be converted to char

// ✅ CORRECT: Cast back to char
char next = (char)(c + i);
```

### 2. Format String Issues

```java
// ❌ PITFALL: Wrong format specifiers
double d = 5.12345;
System.out.printf("%d", d);  // IllegalFormatConversionException: d != double

// ✅ CORRECT: Use matching format specifiers
System.out.printf("%f", d);  // 5.123450
System.out.printf("%.2f", d);  // 5.12

// ❌ PITFALL: Missing arguments
System.out.printf("Value 1: %d, Value 2: %d", 42);  // Missing second argument

// ✅ CORRECT: Provide all arguments
System.out.printf("Value 1: %d, Value 2: %d", 42, 43);
```

### 3. Auto-boxing and Unboxing

```java
// ❌ PITFALL: NullPointerException with auto-unboxing
Integer i = null;
int j = i;  // NullPointerException when unboxing null

// ✅ CORRECT: Check for null before unboxing
if (i != null) {
    int j = i;
}

// ❌ PITFALL: Unexpected equality behavior
Integer a = 127;
Integer b = 127;
System.out.println(a == b);  // TRUE - Integer caches small values

Integer c = 1000;
Integer d = 1000;
System.out.println(c == d);  // FALSE - Outside cache range (-128 to 127)

// ✅ CORRECT: Always use equals() for wrapper objects
System.out.println(c.equals(d));  // TRUE
```

## Best Practices Summary

1. **Floating-point (double/float):**
   - Never use == for equality comparisons
   - Check for NaN/Infinity with Double.isNaN/isInfinite
   - Use BigDecimal for precise decimal arithmetic
   - Understand limitations in representing exact decimal values

2. **Integers:**
   - Be aware of overflow in calculations
   - Use Math.addExact, Math.multiplyExact for safe arithmetic
   - Remember that division truncates, convert to double if needed
   - Handle parsing exceptions

3. **Strings:**
   - Use equals() not == for content comparison
   - Use StringBuilder for concatenation in loops
   - Remember strings are immutable
   - Handle null values defensively

4. **Characters:**
   - Use single quotes for char literals
   - Cast results of char arithmetic back to char
   - Remember the difference between digit characters and numeric values
   - Use String for Unicode characters beyond BMP

5. **General:**
   - Understand implicit type conversions
   - Watch for auto-boxing/unboxing pitfalls
   - Use proper format specifiers with printf/String.format
   - Be cautious with wrapper equality (==)


---

## Character Class Methods

```java
Character.isDigit('5')      // true
Character.isLetter('A')     // true
Character.isLetterOrDigit('a')  // true
Character.isUpperCase('A')  // true
Character.isLowerCase('a')  // true
Character.toLowerCase('A')  // 'a'
Character.toUpperCase('a')  // 'A'
Character.toString('A')     // "A"
```

**Additional Notes:**

- `Character.isDigit(char c)`: Determines if the specified character is a digit.
- `Character.isLetter(char c)`: Determines if the specified character is a letter.
- `Character.isLetterOrDigit(char c)`: Determines if the specified character is a letter or digit.
- `Character.isUpperCase(char c)`: Determines if the specified character is uppercase.
- `Character.isLowerCase(char c)`: Determines if the specified character is lowercase.
- `Character.toLowerCase(char c)`: Converts the character to lowercase.
- `Character.toUpperCase(char c)`: Converts the character to uppercase.
- `Character.toString(char c)`: Returns a String object representing the specified character.

**Handling Unicode Characters:**

- These methods handle Unicode characters correctly, not just ASCII.

**Example:**

```java
Character.isDigit('①'); // false, as '①' is a circled digit one, not a standard digit
Character.isLetter('π'); // true, Greek letter pi is considered a letter
```

---

# Java StringBuilder API Reference

`StringBuilder` is a mutable sequence of characters, providing an efficient way to concatenate and manipulate strings in Java without creating multiple objects.

## Core Methods

### Creation

```java
// Create empty StringBuilder
StringBuilder sb1 = new StringBuilder();

// Create with initial capacity
StringBuilder sb2 = new StringBuilder(100);

// Create with initial content
StringBuilder sb3 = new StringBuilder("Hello");

// Create from another CharSequence
CharSequence cs = "World";
StringBuilder sb4 = new StringBuilder(cs);
```

### Appending

```java
StringBuilder sb = new StringBuilder("Hello");

// Append string
sb.append(" World");  // "Hello World"

// Append primitive types
sb.append(123);       // "Hello World123"
sb.append(true);      // "Hello World123true"
sb.append(3.14f);     // "Hello World123true3.14"

// Append char array
char[] chars = {'J','a','v','a'};
sb.append(chars);     // "Hello World123true3.14Java"

// Append char array portion
sb.append(chars, 0, 2);  // "Hello World123true3.14JavaJa"

// Chain multiple appends
sb.append(" is ").append("awesome");  // "Hello World123true3.14JavaJa is awesome"
```

### Inserting

```java
StringBuilder sb = new StringBuilder("Hello World");

// Insert at specific position (0-indexed)
sb.insert(5, " Beautiful");  // "Hello Beautiful World"

// Insert primitive types
sb.insert(0, 123);           // "123Hello Beautiful World"
sb.insert(3, true);          // "123trueHello Beautiful World"

// Insert char array
char[] chars = {'J','a','v','a'};
sb.insert(0, chars);         // "Java123trueHello Beautiful World"
```

### Deleting

```java
StringBuilder sb = new StringBuilder("Hello Beautiful World");

// Delete range (inclusive, exclusive)
sb.delete(5, 16);            // "Hello World"

// Delete single character
sb.deleteCharAt(5);          // "HelloWorld"
```

### Replacing

```java
StringBuilder sb = new StringBuilder("Hello World");

// Replace range with new string
sb.replace(6, 11, "Java");   // "Hello Java"
```

### Substring

```java
StringBuilder sb = new StringBuilder("Hello World");

// Get substring (inclusive, exclusive)
String sub = sb.substring(0, 5);    // "Hello"

// Get substring from index to end
String sub2 = sb.substring(6);      // "World"
```

### Reverse

```java
StringBuilder sb = new StringBuilder("Hello");
sb.reverse();   // "olleH"
```

### Capacity Management

```java
StringBuilder sb = new StringBuilder();

// Get current capacity
int cap = sb.capacity();    // Default is usually 16

// Ensure minimum capacity
sb.ensureCapacity(100);     // Increases capacity if needed

// Trim to size (reduces to current length)
sb.trimToSize();
```

### Length and Size

```java
StringBuilder sb = new StringBuilder("Hello");

// Get current length
int len = sb.length();      // 5

// Set length (truncates or pads with null chars)
sb.setLength(3);            // "Hel"
sb.setLength(10);           // "Hel\0\0\0\0\0\0\0" (null chars)
```

### Character Access

```java
StringBuilder sb = new StringBuilder("Hello");

// Get character at position
char c = sb.charAt(1);      // 'e'

// Set character at position
sb.setCharAt(0, 'J');       // "Jello"

// Get code point at position
int cp = sb.codePointAt(0); // Unicode code point for 'J'
```

### String Conversion

```java
StringBuilder sb = new StringBuilder("Hello");

// Convert to String
String str = sb.toString();
```

## Performance Note

StringBuilder is not synchronized. For thread-safe operations, use StringBuffer instead (with identical API but synchronization overhead).

```java
// Thread-safe version
StringBuffer sbuf = new StringBuffer("Hello");
```

## Common Use Case: Building Delimited List

```java
String[] items = {"Apple", "Banana", "Cherry"};
StringBuilder sb = new StringBuilder();

for (int i = 0; i < items.length; i++) {
    sb.append(items[i]);
    
    // Add delimiter except after last item
    if (i < items.length - 1) {
        sb.append(", ");
    }
}

String result = sb.toString();  // "Apple, Banana, Cherry"
```

---

# Java Type Conversion Guide

## 1. String ↔ int

### Converting String to int
```java
// ✅ Correct
int i = Integer.parseInt("123");  // Standard way
int i = Integer.valueOf("123");   // Returns Integer object, autoboxed to int

// ❌ Incorrect
// int i = "123";  // Error: String cannot be converted to int automatically

// ✅ Special cases
int i = Integer.parseInt("123", 16);  // Parse as hexadecimal (result: 291)
int i = Integer.parseInt("+123");     // Handles + sign
int i = Integer.parseInt("-123");     // Handles negative numbers

// ❌ Watch out!
// int i = Integer.parseInt("12.5");  // Error: NumberFormatException
// int i = Integer.parseInt("123f");  // Error: NumberFormatException
// int i = Integer.parseInt("");      // Error: NumberFormatException
// int i = Integer.parseInt(null);    // Error: NullPointerException
```

### Converting int to String
```java
// ✅ Correct
String s = String.valueOf(123);    // Preferred method
String s = Integer.toString(123);  // Alternative
String s = "" + 123;               // Works but less efficient
String s = new Integer(123).toString(); // Works but unnecessary object creation

// ❌ Incorrect
// String s = 123;  // Error: int cannot be directly assigned to String

// ✅ Special cases
String s = Integer.toHexString(123);  // Convert to hex (result: "7b")
String s = Integer.toBinaryString(123);  // Convert to binary
String s = String.format("%d", 123);  // Using format
String s = String.format("%04d", 123);  // Padding zeros (result: "0123")
```

## 2. String ↔ double

### Converting String to double
```java
// ✅ Correct
double d = Double.parseDouble("12.34");  // Standard
double d = Double.valueOf("12.34");      // Returns Double object, autoboxed

// ❌ Incorrect
// double d = "12.34";  // Error: String cannot be converted to double

// ✅ Special cases
double d = Double.parseDouble("12");     // Works with integers (result: 12.0)
double d = Double.parseDouble("1.2e3");  // Scientific notation (result: 1200.0)
double d = Double.parseDouble("-0.5");   // Negative numbers
double d = Double.parseDouble("Infinity"); // Special values
double d = Double.parseDouble("NaN");    // Not a Number

// ❌ Watch out!
// double d = Double.parseDouble("12,34");  // Error: Uses '.' not ','
// double d = Double.parseDouble("12.34f"); // Error: Don't include type suffix
```

### Converting double to String
```java
// ✅ Correct
String s = String.valueOf(12.34);    // Standard
String s = Double.toString(12.34);   // Alternative
String s = "" + 12.34;               // Works but less efficient

// ❌ Incorrect
// String s = 12.34;  // Error: double cannot be directly assigned to String

// ✅ Formatting options
String s = String.format("%.2f", 12.34);  // Fixed precision (result: "12.34")
String s = String.format("%e", 12.34);    // Scientific notation
String s = new DecimalFormat("#.##").format(12.345);  // Custom format (result: "12.35")
```

## 3. String ↔ float

### Converting String to float
```java
// ✅ Correct
float f = Float.parseFloat("3.14");
float f = Float.valueOf("3.14");

// ❌ Incorrect
// float f = "3.14";  // Error: String cannot be converted to float

// ✅ Special cases
float f = Float.parseFloat("3.14f");  // 'f' suffix is accepted (unlike Double)
float f = Float.parseFloat("3");      // Integer strings work too
```

### Converting float to String
```java
// ✅ Correct
String s = String.valueOf(3.14f);
String s = Float.toString(3.14f);
String s = "" + 3.14f;               // Works but less efficient

// ❌ Incorrect
// String s = 3.14f;  // Error: float cannot be assigned to String

// ✅ Formatting options
String s = String.format("%.3f", 3.14f);  // With specific precision
```

## 4. String ↔ char

### Converting String to char
```java
// ✅ Correct
char c = "A".charAt(0);  // Get first character
char c = "ABC".charAt(2);  // Get character at index (result: 'C')

// ❌ Incorrect
// char c = "A";  // Error: Cannot assign String to char
// char c = "ABC";  // Error: Multi-character string can't convert to single char

// ❌ Watch out!
// char c = "".charAt(0);  // Error: StringIndexOutOfBoundsException
```

### Converting char to String
```java
// ✅ Correct
String s = String.valueOf('A');
String s = Character.toString('A');
String s = "" + 'A';  // Works but less efficient

// ❌ Incorrect
// String s = 'A';  // Error: char cannot be directly assigned to String

// ✅ Multiple chars to String
String s = new String(new char[]{'A', 'B', 'C'});  // Create from char array
```

## 5. int ↔ double/float

### Converting int to double/float
```java
// ✅ Correct (Widening conversions - always safe)
double d = 10;        // Implicit conversion works
float f = 10;         // int to float is safe

// ✅ Explicit but unnecessary
double d = (double) 10;  // Cast works but isn't needed
float f = (float) 10;    // Cast works but isn't needed
```

### Converting double/float to int
```java
// ✅ Correct (Narrowing conversions - require casting)
int i = (int) 10.5;    // Truncates to 10
int i = (int) 10.99f;  // Truncates to 10

// ❌ Incorrect
// int i = 10.5;       // Error: Possible loss of precision
// int i = 10.5f;      // Error: Possible loss of precision

// ✅ Alternatives
int i = (int) Math.round(10.5);  // Rounds to 11
int i = Math.round(10.5f);       // float version returns int directly
```

### Converting float to double and vice versa
```java
// ✅ Correct (float to double is widening)
double d = 3.14f;         // Implicit conversion works
double d = (double) 3.14f;  // Explicit cast works but isn't needed

// ✅ Correct (double to float requires cast)
float f = (float) 3.14;   // Explicit cast required

// ❌ Incorrect
// float f = 3.14;        // Error: double cannot be converted to float
```

## 6. int ↔ char

### Converting int to char
```java
// ✅ Correct
char c = (char) 65;  // Explicit cast required unless it's a char literal
char c = 'A';        // Character literal (ASCII 65)

// ❌ Incorrect
// char c = 65;      // Error: Possible loss of precision
// char c = -1;      // Error: Possible loss of precision

// ✅ Special cases
char c = '\u0041';   // Unicode literal for 'A'
```

### Converting char to int
```java
// ✅ Correct (Widening primitive conversion)
int i = 'A';         // Result: 65 (ASCII value)
int i = (int) 'A';   // Explicit cast works but isn't needed

// ✅ Getting numeric value of digit characters
int digit = Character.getNumericValue('5');  // Result: 5
int digit = '5' - '0';  // Common trick (result: 5)

// ❌ Watch out!
// int digit = (int)'5';  // Result: 53 (ASCII value), not 5
```

## 7. Common Mistakes and Solutions

### Double ↔ String ↔ int Problems
```java
// ❌ Mistakes
// Integer.valueOf(42.5);       // Error: No overload for double
// Integer.valueOf("42.5");     // Error: NumberFormatException
// int i = Double.valueOf("42.5").intValue();  // Cumbersome

// ✅ Solutions
int i = (int) Double.parseDouble("42.5");  // Parse as double then cast to int
Double d = Double.valueOf("42");  // Works with integer strings

// ❌ More mistakes
// String s = 42;             // Error: Cannot convert
// double d = "42.5";         // Error: Cannot convert

// ✅ Solutions
String s = String.valueOf(42);
double d = Double.parseDouble("42.5");
```

### Boxing/Unboxing Issues
```java
// ✅ Autoboxing (primitive to wrapper)
Integer iObj = 100;         // Implicitly converted to Integer
Double dObj = 3.14;         // double → Double

// ✅ Unboxing (wrapper to primitive)
int i = new Integer(100);   // Implicitly converted to int
double d = new Double(3.14);  // Double → double

// ❌ Watch out!
// Integer iObj = null;
// int i = iObj;  // Error: NullPointerException when unboxing null
```

## 8. Summary Table: What Works & How

| From → To | Conversion Method |
|----------|-------------------|
| `String` → `int` | `Integer.parseInt(s)` |
| `String` → `double` | `Double.parseDouble(s)` |
| `String` → `float` | `Float.parseFloat(s)` |  
| `String` → `char` | `s.charAt(0)` |
| `int` → `String` | `String.valueOf(i)` or `Integer.toString(i)` |
| `double` → `String` | `String.valueOf(d)` or `Double.toString(d)` |
| `float` → `String` | `String.valueOf(f)` or `Float.toString(f)` |
| `char` → `String` | `String.valueOf(c)` or `Character.toString(c)` |
| `char` → `int` | Implicit (`int i = 'A';`) |
| `int` → `char` | Cast (`char c = (char) i;`) |
| `double` → `int` | Cast (`int i = (int) d;`) |
| `float` → `int` | Cast (`int i = (int) f;`) |
| `int` → `double` | Implicit (`double d = i;`) |
| `int` → `float` | Implicit (`float f = i;`) |
| `float` → `double` | Implicit (`double d = f;`) |
| `double` → `float` | Cast (`float f = (float) d;`) |



---

## Number Conversions

```java
// String to numeric
int num = Integer.parseInt("123");    // 123
double d = Double.parseDouble("3.14"); // 3.14
long l = Long.parseLong("1234567890"); // 1234567890

// Binary conversions
int binToInt = Integer.parseInt("1010", 2);  // 10
String intToBin = Integer.toBinaryString(10); // "1010"

# Binary Conversion Methods in Java

## Using Built-in Java Methods

```java
String binary = "1101";
BigInteger result = new BigInteger(binary, 2); // 13

String binary = "1101";
int result = Integer.parseInt(binary, 2); 

long result = Long.parseLong(binary, 2);

Integer result = Integer.valueOf(binary, 2);

Long result = Long.valueOf(binary, 2);
```

## Manual Binary Conversion Implementation

```java
String binary = "1101";
int result = 0;
for (int i = 0; i < binary.length(); i++) {
    result = result * 2 + Character.getNumericValue(binary.charAt(i));
}
```


// Other bases
int hexToInt = Integer.parseInt("1A", 16);   // 26
String intToHex = Integer.toHexString(26);   // "1a"

// Using valueOf (returns boxed objects)
Integer i = Integer.valueOf("123");
Double d2 = Double.valueOf("3.14");

// BigInteger for binary operations
import java.math.BigInteger;
String result = new BigInteger("1010", 2)
    .add(new BigInteger("1111", 2))
    .toString(2);  // "11001"
```

**Additional Notes:**

- `Integer.parseInt(String s, int radix)`: Parses the string as a signed integer in the specified radix (e.g., 2 for binary, 16 for hexadecimal).
- `Integer.toBinaryString(int i)`: Returns a string representation of the integer in base 2.
- `BigInteger`: Useful for arbitrary-precision integers and operations on large numbers.

**Example: Adding Two Binary Strings**

```java
String a = "1010";
String b = "1111";
String sum = new BigInteger(a, 2).add(new BigInteger(b, 2)).toString(2);  // "11001"
```

**Error Handling:**

- `NumberFormatException` is thrown if the string does not contain a parsable number.

**Example:**

```java
try {
    int num = Integer.parseInt("abc");
} catch (NumberFormatException e) {
    System.out.println("Invalid number format");
}
```

**Additional Example: Octal Conversion**

```java
int octToInt = Integer.parseInt("777", 8);  // 511
String intToOct = Integer.toOctalString(511); // "777"
```

---

## Math Class

```java
Math.max(5, 10);           // 10
Math.min(5, 10);           // 5
Math.abs(-10);             // 10
Math.sqrt(16);             // 4.0
Math.pow(2, 3);            // 8.0
Math.round(3.7);           // 4
Math.floor(3.7);           // 3.0
Math.ceil(3.2);            // 4.0
Math.random();             // random between 0.0 and 1.0
int random = (int)(Math.random() * 100); // random between 0-99

// Calculate number of digits
int digits = (int) Math.log10(num) + 1;
```

**Examples of Math.sqrt:**

```java
Math.sqrt(16);    // 4.0
Math.sqrt(2);     // 1.4142135623730951
Math.sqrt(0);     // 0.0
Math.sqrt(-4);    // NaN
Math.sqrt(25.5);  // 5.049752469181039
```

**Additional Math Methods:**

- `Math.sin(double a)`: Returns the sine of an angle (in radians).
- `Math.cos(double a)`: Returns the cosine of an angle.
- `Math.tan(double a)`: Returns the tangent of an angle.

**Example:**

```java
double angle = Math.PI / 4; // 45 degrees in radians
double sine = Math.sin(angle); // 0.7071067811865475
double cosine = Math.cos(angle); // 0.7071067811865476
double tangent = Math.tan(angle); // 1.0
```

**Practical Application:**

- Generate a random integer within a specific range:

```java
int min = 5, max = 15;
int randomInRange = min + (int)(Math.random() * (max - min + 1)); // Random between 5-15
```

---

## String Methods

```java
String s = "Hello World";
int length = s.length();        // 11
char c = s.charAt(0);           // 'H'
String sub = s.substring(6);    // "World"
String subRange = s.substring(0, 5); // "Hello"
int index = s.indexOf("World");  // 6
int lastIndex = s.lastIndexOf("o"); // 7

// Comparisons
boolean equals = s.equals("Hello World");      // true
boolean ignoreCase = s.equalsIgnoreCase("hello world"); // true
int comparison = s.compareTo("Abc");           // positive value
int compareIgnoreCase = s.compareToIgnoreCase("HELLO WORLD"); // 0

// Modifications
String upper = s.toUpperCase();        // "HELLO WORLD"
String lower = s.toLowerCase();        // "hello world"
String trimmed = "  Hello  ".trim();   // "Hello"
String replaced = s.replace('l', 'L'); // "HeLLo WorLd"
String replacedStr = s.replace("World", "Java"); // "Hello Java"
String replaceAll = s.replaceAll("l", "L"); // "HeLLo WorLd"

// Check for patterns
boolean startsWith = s.startsWith("He"); // true
boolean endsWith = s.endsWith("ld");     // true
boolean contains = s.contains("lo");     // true
boolean isEmpty = s.isEmpty();           // false

// Split
String[] parts = "a,b,c".split(",");     // ["a", "b", "c"]
String[] splitLimit = "a,b,c,d".split(",", 2); // ["a", "b,c,d"]

// Join
String joined = String.join("-", "a", "b", "c"); // "a-b-c"

// Converting to char array and back
char[] chars = s.toCharArray();
String fromChars = new String(chars);

// StringBuilder for efficient string manipulation
StringBuilder sb = new StringBuilder();
sb.append("Hello").append(" ").append("World");
String result = sb.toString();  // "Hello World"
sb.insert(5, "!");              // "Hello! World"
sb.delete(5, 6);                // "Hello World"
sb.reverse();                   // "dlroW olleH"
sb.setCharAt(0, 'h');           // "hlroW olleH"
```

**Additional Notes on String.split:**

- The `limit` parameter controls the number of splits:
  - `limit = -1`: Splits into all possible substrings, including trailing empty strings.
  - `limit = 0`: Splits into all possible substrings, discarding trailing empty strings.
  - `limit > 0`: Limits the number of splits to `limit`.

**Example:**

```java
String s = "a,b,c,";
String[] arr1 = s.split(",", -1); // ["a", "b", "c", ""]
String[] arr2 = s.split(",", 0);  // ["a", "b", "c"]
String[] arr3 = s.split(",", 2);  // ["a", "b,c,"]
```

- For special characters, escape them or use `Pattern.quote()`:

```java
String[] parts = "a.b.c".split("\\."); // ["a", "b", "c"]
String[] parts2 = "a.b.c".split(Pattern.quote(".")); // ["a", "b", "c"]
```

# Comprehensive Guide to Java String.split()

## Basic Syntax

```java
public String[] split(String regex)
public String[] split(String regex, int limit)
```

## Parameters

- `regex`: A regular expression delimiting the splits
- `limit`: Controls the number of splits and array length

## The `limit` Parameter Explained

- `limit < 0`: Splits into all possible substrings, including trailing empty strings
- `limit = 0`: Splits into all possible substrings, discarding trailing empty strings (default behavior)
- `limit > 0`: Limits the number of splits to `limit-1` (resulting array length will be at most `limit`)

### Examples with Different Limit Values

```java
String s = "a,b,c,";

String[] arr1 = s.split(",", -1); // ["a", "b", "c", ""]
String[] arr2 = s.split(",", 0);  // ["a", "b", "c"]
String[] arr3 = s.split(",", 2);  // ["a", "b,c,"]
String[] arr4 = s.split(",", 3);  // ["a", "b", "c,"]
String[] arr5 = s.split(",");     // Same as limit=0: ["a", "b", "c"]
```

## Handling Special Regex Characters

When splitting by characters that have special meaning in regex:

```java
// Using backslash to escape special characters
String[] parts1 = "a.b.c".split("\\.");  // ["a", "b", "c"]
String[] parts2 = "a|b|c".split("\\|");  // ["a", "b", "c"]
String[] parts3 = "a(b)c".split("\\("); // ["a", "b)c"]

// Using Pattern.quote() to escape automatically
import java.util.regex.Pattern;
String[] parts4 = "a.b.c".split(Pattern.quote(".")); // ["a", "b", "c"]
String[] parts5 = "a$b$c".split(Pattern.quote("$")); // ["a", "b", "c"]
```

## Common Regex Patterns for Split

### Whitespace Splitting

```java
// Split by any whitespace (space, tab, newline)
String text = "Hello World\tJava\nProgramming";
String[] words1 = text.split("\\s");      // Splits on single whitespace
String[] words2 = text.split("\\s+");     // Splits on one or more whitespaces
String[] words3 = text.split("\\s*");     // Splits on zero or more whitespaces (results in individual characters)

// Split by specific whitespace characters
String[] words4 = text.split(" ");        // Split only by space
String[] words5 = text.split("\t");       // Split only by tab
String[] words6 = text.split("\n");       // Split only by newline

// Using POSIX character class for whitespace
String[] words7 = text.split("\\p{Space}"); // Equivalent to \s
```

### Multiple Delimiters

```java
// Split by multiple different delimiters
String data = "apple,banana;orange|grape";
String[] fruits = data.split("[,;|]");  // ["apple", "banana", "orange", "grape"]

// Split by any non-word character
String sentence = "Hello, World! Java: Programming.";
String[] words = sentence.split("\\W+"); // ["Hello", "World", "Java", "Programming"]

// Split by character ranges
String text = "a1b2c3d4";
String[] parts1 = text.split("[0-9]");  // ["a", "b", "c", "d", ""]
String[] parts2 = text.split("[a-z]");  // ["", "1", "2", "3", "4"]

// Split by negated character classes
String mixed = "a:b,c;d";
String[] parts = mixed.split("[^a-z]");  // ["a", "b", "c", "d"]
```

### Character Classes

```java
// Split by digits
String alphanumeric = "abc123def456";
String[] parts = alphanumeric.split("\\d+"); // ["abc", "def", ""]

// Split by non-digits
String mixed = "123abc456def";
String[] numbers = mixed.split("\\D+"); // ["123", "456", ""]

// Split by word boundaries
String text = "HelloWorldJava";
String[] words = text.split("(?=[A-Z])"); // ["", "Hello", "World", "Java"]

// Split by word characters
String code = "int x = 10;";
String[] tokens = code.split("\\w+"); // ["", " ", " = ", ";"]

// Split by boundaries between word and non-word characters
String phrase = "Hello, World!";
String[] parts = phrase.split("\\b"); // ["", "Hello", ", ", "World", "!"]

// Split by Java identifiers
String declaration = "int count=0;double average=7.5;";
String[] vars = declaration.split("\\b(?=\\w+\\s*=)"); // ["int ", "count=0;double ", "average=7.5;"]
```

### Capturing Groups

```java
// Using positive lookahead to keep delimiters
String csv = "a,b,c";
String[] withCommas = csv.split("(?<=,)"); // ["a,", "b,", "c"]

// Using lookbehind for complex splits
String data = "key1=value1;key2=value2";
String[] keyValues = data.split("(?<==)|(?=;)"); // ["key1", "=", "value1", ";", "key2", "=", "value2"]

// Split at positions, keeping the delimiter
String text = "apple,banana,cherry";
String[] parts = text.split("(?<=,)|(?=,)"); // ["apple", ",", "banana", ",", "cherry"]

// Split by word boundaries without losing delimiters
String sentence = "Hello, world!";
String[] tokens = sentence.split("(?<=\\b)|(?=\\b)"); // Complex tokenization
```

## Empty Results and Edge Cases

```java
// Empty string
String empty = "";
String[] parts1 = empty.split(",");     // [""]
String[] parts2 = empty.split(",", -1); // [""]

// Delimiter not found
String text = "hello";
String[] parts = text.split(",");       // ["hello"]

// String contains only delimiters
String delims = "...";
String[] parts1 = delims.split("\\.");  // [] (empty array)
String[] parts2 = delims.split("\\.", -1); // ["", "", "", ""]
```

## Performance Considerations

```java
// Precompile regex for better performance
import java.util.regex.Pattern;

Pattern pattern = Pattern.compile(",");
String data = "a,b,c,d,e,f,g,h,i,j";
String[] parts = pattern.split(data);  // More efficient for repeated use
```

## Practical Examples

### CSV Parsing

```java
String csvLine = "John,Doe,\"New York, NY\",USA";

// Simple split - won't handle quoted fields correctly
String[] fields1 = csvLine.split(","); // ["John", "Doe", "\"New York", " NY\"", "USA"]

// Better to use dedicated CSV libraries for complex cases
```

### Parsing Key-Value Pairs

```java
String config = "key1=value1;key2=value2;key3=value3";

// Two-step split
String[] pairs = config.split(";");
for (String pair : pairs) {
    String[] keyValue = pair.split("=");
    System.out.println("Key: " + keyValue[0] + ", Value: " + keyValue[1]);
}
```

### URL Parsing

```java
String url = "https://example.com/path?param1=value1&param2=value2";
String[] parts = url.split("\\?");
String domain = parts[0]; // "https://example.com/path"

if (parts.length > 1) {
    String[] queryParams = parts[1].split("&");
    // ["param1=value1", "param2=value2"]
}
```

### Tokenizing Code

```java
String code = "int x = 10; x += 5; System.out.println(x);";
String[] statements = code.split(";\\s*");
// ["int x = 10", "x += 5", "System.out.println(x)", ""]
```

## Common Pitfalls

### 1. Forgetting to Escape Special Characters

```java
// Wrong
String[] parts = "1.2.3".split(".");  // Results in empty array []

// Correct
String[] parts = "1.2.3".split("\\.");  // ["1", "2", "3"]
```

### 2. Ignoring Empty Strings

```java
String data = "a,,b,c,";
String[] parts1 = data.split(",");    // ["a", "b", "c"]
String[] parts2 = data.split(",", -1); // ["a", "", "b", "c", ""]
```

### 3. Using Incorrect Regex Quantifiers

```java
String text = "a   b     c";
// Wrong expectation
String[] parts1 = text.split(" ");  // ["a", "", "", "b", "", "", "", "", "c"]

// Correct approach
String[] parts2 = text.split("\\s+");  // ["a", "b", "c"]
```

### 4. Splitting Unicode Characters

```java
// For Unicode-aware splitting
String text = "Hello😊World🌍Java⭐";
String[] parts = text.split("(?U)\\P{L}+");  // ["Hello", "World", "Java"]
```

## Related String Methods

- `String.join()`: The reverse operation of split
  ```java
  String[] parts = {"a", "b", "c"};
  String joined = String.join(",", parts);  // "a,b,c"
  ```

- `StringTokenizer`: An alternative (legacy) way to split strings
  ```java
  import java.util.StringTokenizer;
  
  StringTokenizer st = new StringTokenizer("a:b:c", ":");
  while (st.hasMoreTokens()) {
      System.out.println(st.nextToken());
  }
  ```


**Performance Tip:**

- Use `StringBuilder` for efficient string concatenation in loops to avoid creating multiple immutable strings.

**Example:**

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i).append(",");
}
String result = sb.toString(); // "0,1,2,...,999,"
```

---

## Arrays

```java
// Declaration and initialization
int[] numbers = new int[5];
int[] initialized = {1, 2, 3, 4, 5};
int[][] matrix = new int[3][3];
int[][] mat = {{1,2,3}, {4,5,6}, {7,8,9}};

// Important operations
int length = numbers.length;   // 5
Arrays.sort(numbers);          // Sort in ascending order

// Binary search (array must be sorted)
int index = Arrays.binarySearch(numbers, 3);

// Copy arrays
int[] copied = Arrays.copyOf(numbers, numbers.length);
int[] partial = Arrays.copyOfRange(numbers, 1, 3); // elements at index 1 and 2

// Fill array
Arrays.fill(numbers, 0);       // Fill entire array with 0

// Compare arrays
boolean equal = Arrays.equals(numbers, copied);

// Convert array to String for debugging
String arrayString = Arrays.toString(numbers);
String matrixString = Arrays.deepToString(matrix);

// Sort with custom comparator (only for object arrays)
Integer[] nums = {5, 2, 8, 1};
Arrays.sort(nums, (a, b) -> b - a);  // Sort in descending order

// For primitive arrays, box to Integer[] first
int[] primitives = {5, 2, 8, 1};
Integer[] boxed = new Integer[primitives.length];
for (int i = 0; i < primitives.length; i++) {
    boxed[i] = primitives[i];
}
Arrays.sort(boxed, Collections.reverseOrder());
// Unbox back to int[]
for (int i = 0; i < primitives.length; i++) {
    primitives[i] = boxed[i];
}

// Sort 2D array - no boxing needed as int[] is an object
int[][] points = {{3, 4}, {1, 2}, {5, 0}};
Arrays.sort(points, (a, b) -> a[0] - b[0]);  // Sort by first element
```

**Note on 2D Arrays:**

- Access elements with `matrix[row][col]`.
- Get number of rows: `matrix.length`
- Get number of columns: `matrix[0].length`

**Performance Considerations:**

- Arrays are fixed-size, so operations like insertion or deletion require creating new arrays or shifting elements, which can be costly for large arrays.

**Example: Multi-dimensional Arrays**

```java
int[][][] threeD = new int[2][3][4]; // 2 layers, 3 rows, 4 columns
threeD[0][1][2] = 5; // Set element at layer 0, row 1, column 2
System.out.println(Arrays.deepToString(threeD));
```

---

## ArrayList

```java
import java.util.ArrayList;

ArrayList<Integer> list = new ArrayList<>();

// Basic operations
list.add(10);                   // Add to end
list.add(0, 5);                 // Add at index 0
int value = list.get(1);        // Get element at index 1
list.set(0, 7);                 // Replace element at index 0
list.remove(0);                 // Remove element at index 0
int size = list.size();         // Get size
boolean contains = list.contains(10); // Check if element exists
list.clear();                   // Remove all elements
boolean isEmpty = list.isEmpty(); // Check if empty

// Finding elements
int index = list.indexOf(10);   // First index of element, -1 if not found
int lastIndex = list.lastIndexOf(10); // Last index of element

// Iteration options
for (Integer num : list) { /* process num */ }
for (int i = 0; i < list.size(); i++) { int num = list.get(i); }

// Using Iterator
Iterator<Integer> iter = list.iterator();
while (iter.hasNext()) {
    Integer num = iter.next();
    if (num % 2 == 0) iter.remove(); // Safe removal during iteration
}

// Utilities
Collections.sort(list);                        // Sort ascending
Collections.sort(list, Collections.reverseOrder()); // Sort descending
Collections.reverse(list);                     // Reverse order
Collections.shuffle(list);                     // Random order
int max = Collections.max(list);               // Find max element
int min = Collections.min(list);               // Find min element
Collections.fill(list, 0);                     // Fill with value

// Convert to array
Integer[] array = list.toArray(new Integer[0]);
```

**Safe Removal During Iteration:**

```java
Iterator<Integer> iter = list.iterator();
while (iter.hasNext()) {
    Integer num = iter.next();
    if (num % 2 == 0) {
        iter.remove(); // Removes even numbers safely
    }
}
```

**Capacity Management:**

- `ArrayList` automatically resizes, but you can set initial capacity to optimize performance:

```java
ArrayList<Integer> list = new ArrayList<>(100); // Initial capacity of 100
```

**Comparison with LinkedList:**

- `ArrayList` is better for random access (O(1)), while `LinkedList` is better for frequent insertions/deletions at both ends (O(1)).

---

## LinkedList

```java
import java.util.LinkedList;

LinkedList<String> linked = new LinkedList<>();

// Adds to both ends
linked.add("middle");           // Add to end
linked.addFirst("first");       // Add to beginning
linked.addLast("last");         // Add to end

// Retrieval from both ends
String first = linked.getFirst();
String last = linked.getLast();

// Removal from both ends
String removed = linked.removeFirst();
String removedLast = linked.removeLast();

// Queue operations
linked.offer("item");           // Add to tail
String head = linked.poll();    // Remove & return head
String peek = linked.peek();    // View head without removal

// Stack operations
linked.push("top");             // Add to head
String popped = linked.pop();   // Remove & return head
```

**Use Cases:**

- Implementing a deque (double-ended queue).
- Efficient insertion/deletion at both ends.

**Performance:**

- O(1) for add/remove at ends, O(n) for access by index.

**Example: Using LinkedList as a Deque**

```java
LinkedList<Integer> deque = new LinkedList<>();
deque.addFirst(1);
deque.addLast(2);
System.out.println(deque.pollFirst()); // 1
System.out.println(deque.pollLast());  // 2
```

---

## HashSet

```java
import java.util.HashSet;

HashSet<String> set = new HashSet<>();

// Basic operations
set.add("apple");              // Add element
set.remove("apple");           // Remove element
boolean contains = set.contains("apple"); // Check if exists
int size = set.size();         // Number of elements
set.clear();                   // Remove all elements

// Set operations
HashSet<String> set2 = new HashSet<>();
set2.add("banana");
set.addAll(set2);              // Union
set.retainAll(set2);           // Intersection
set.removeAll(set2);           // Difference

// Iteration
for (String item : set) { /* process item */ }

// Convert to array
String[] array = set.toArray(new String[0]);
```

**Handling Duplicates:**

- `HashSet` automatically handles duplicates; adding an existing element does nothing.

**Hash Collisions:**

- Internally, `HashSet` uses a `HashMap` to store elements, handling collisions with chaining.

**Example: Set Operations**

```java
HashSet<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3));
HashSet<Integer> set2 = new HashSet<>(Arrays.asList(2, 3, 4));
set1.addAll(set2);    // Union: [1, 2, 3, 4]
set1.retainAll(set2); // Intersection: [2, 3]
set1.removeAll(set2); // Difference: [1]
```

---

## TreeSet

```java
import java.util.TreeSet;

TreeSet<Integer> treeSet = new TreeSet<>();

// Basic operations (maintains sorted order)
treeSet.add(5);
treeSet.add(1);
treeSet.add(10);

// Navigation methods
Integer first = treeSet.first();      // Lowest element
Integer last = treeSet.last();        // Highest element
Integer lower = treeSet.lower(5);     // Greatest element < 5
Integer higher = treeSet.higher(5);   // Smallest element > 5
Integer floor = treeSet.floor(5);     // Greatest element <= 5
Integer ceiling = treeSet.ceiling(5); // Smallest element >= 5

// Range views
SortedSet<Integer> headSet = treeSet.headSet(5);   // All elements < 5
SortedSet<Integer> tailSet = treeSet.tailSet(5);   // All elements >= 5
SortedSet<Integer> subSet = treeSet.subSet(1, 10); // Range 1 <= e < 10

// Descending order
NavigableSet<Integer> descending = treeSet.descendingSet();
```

**Custom Comparators:**

- `TreeSet` can use a custom comparator for ordering:

```java
TreeSet<String> set = new TreeSet<>((s1, s2) -> s2.compareTo(s1)); // Descending order
set.add("apple");
set.add("banana");
System.out.println(set); // [banana, apple]
```



# TreeSet.ceiling(E e) and TreeMap.ceilingKey(K key)

## TreeSet.ceiling(E e)
**Description**:  
Returns the least element in this set greater than or equal to the given element, or `null` if there is no such element.

**Example**:
```
TreeSet<Integer> set = new TreeSet<>(Arrays.asList(1, 3, 6, 10));
System.out.println(set.ceiling(5));  // Output: 6
System.out.println(set.ceiling(10)); // Output: 10
System.out.println(set.ceiling(11)); // Output: null (edge case)
```

## TreeMap `ceilingKey(K key)`

**Description:**  
Returns the least key **greater than or equal to** the given key, or `null` if there is no such key.

---

### Example:

java
```
TreeMap<Integer, String> map = new TreeMap<>();
map.put(1, "One");
map.put(3, "Three");
map.put(6, "Six");

System.out.println(map.ceilingKey(4)); // Output: 6
System.out.println(map.ceilingKey(6)); // Output: 6
System.out.println(map.ceilingKey(7)); // Output: null (edge case)
```




**Red-Black Tree:**

- Internally, `TreeSet` is implemented as a red-black tree, ensuring O(log n) operations.

---

## HashMap

```java
import java.util.HashMap;

HashMap<String, Integer> map = new HashMap<>();

// Basic operations
map.put("apple", 5);            // Add key-value pair
map.put("apple", 10);           // Update existing key
Integer count = map.get("apple"); // Get value for key
Integer defaultVal = map.getOrDefault("banana", 0); // Get with default
map.remove("apple");            // Remove key-value pair
boolean containsKey = map.containsKey("apple"); // Check if key exists
boolean containsValue = map.containsValue(5);   // Check if value exists
int size = map.size();          // Number of entries
map.clear();                    // Remove all entries

// Iteration over keys
for (String key : map.keySet()) { /* process key */ }

// Iteration over values
for (Integer value : map.values()) { /* process value */ }

// Iteration over entries
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
}

// Using Iterator for entries
Iterator<Map.Entry<String, Integer>> it = map.entrySet().iterator();
while (it.hasNext()) {
    Map.Entry<String, Integer> entry = it.next();
    if (entry.getValue() < 3) {
        it.remove(); // Safe removal during iteration
    }
}
```

**Load Factor:**

- The load factor determines when the map resizes (default is 0.75).

**Concurrency:**

- `HashMap` is not thread-safe; use `ConcurrentHashMap` for concurrent access.

**Example: Counting Frequencies**

```java
HashMap<String, Integer> freq = new HashMap<>();
String[] words = {"apple", "banana", "apple"};
for (String word : words) {
    freq.put(word, freq.getOrDefault(word, 0) + 1);
}
System.out.println(freq); // {apple=2, banana=1}
```

---

## TreeMap

```java
import java.util.TreeMap;

TreeMap<String, Integer> treeMap = new TreeMap<>();

// Basic operations (maintains sorted key order)
treeMap.put("banana", 2);
treeMap.put("apple", 5);
treeMap.put("cherry", 3);

// Navigation methods
String firstKey = treeMap.firstKey();      // Lowest key
String lastKey = treeMap.lastKey();        // Highest key
String lowerKey = treeMap.lowerKey("banana"); // Greatest key < "banana"
String higherKey = treeMap.higherKey("banana"); // Smallest key > "banana"
String floorKey = treeMap.floorKey("banana");   // Greatest key <= "banana"
String ceilingKey = treeMap.ceilingKey("banana"); // Smallest key >= "banana"

// Entry navigation
Map.Entry<String, Integer> firstEntry = treeMap.firstEntry();
Map.Entry<String, Integer> lastEntry = treeMap.lastEntry();

// Range views
SortedMap<String, Integer> headMap = treeMap.headMap("cherry"); // Keys < "cherry"
SortedMap<String, Integer> tailMap = treeMap.tailMap("banana"); // Keys >= "banana"
SortedMap<String, Integer> subMap = treeMap.subMap("apple", "cherry"); // Range
```

**Balancing Mechanism:**

- Like `TreeSet`, `TreeMap` uses a red-black tree to maintain balance.

**Use Cases:**

- When you need a map with sorted keys or navigation based on key order.

**Example: Range Query**

```java
TreeMap<Integer, String> scores = new TreeMap<>();
scores.put(90, "A");
scores.put(85, "B");
scores.put(95, "A+");
SortedMap<Integer, String> range = scores.subMap(85, 95);
System.out.println(range); // {85=B, 90=A}
```

---

## PriorityQueue

```java
import java.util.PriorityQueue;

// Min heap (default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.add(5);
minHeap.add(2);
minHeap.add(8);
int min = minHeap.peek();       // View minimum element (2)
int removed = minHeap.poll();   // Remove and return minimum (2)

// Max heap using custom comparator
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
maxHeap.add(5);
maxHeap.add(2);
maxHeap.add(8);
int max = maxHeap.peek();       // View maximum element (8)

// Priority queue with custom comparator for objects
PriorityQueue<Task> taskQueue = new PriorityQueue<>((a, b) -> a.priority - b.priority);
taskQueue.add(new Task("Task 1", 3));
taskQueue.add(new Task("Task 2", 1));
Task highestPriority = taskQueue.poll(); // Task with lowest priority number

// Process queue in priority order
while (!minHeap.isEmpty()) {
    int current = minHeap.poll();
    // Process current
}
```

**Applications:**

- Dijkstra's algorithm, Huffman coding, and other algorithms requiring priority-based processing.

**Note:**

- `PriorityQueue` does not guarantee order when iterating; use `poll()` to get elements in order.

**Example: Task Class**

```java
class Task {
    String name;
    int priority;
    Task(String name, int priority) {
        this.name = name;
        this.priority = priority;
    }
}
```

---

## Stack

```java
import java.util.Stack;

Stack<String> stack = new Stack<>();

// Basic operations
stack.push("first");           // Add to top
stack.push("second");
String top = stack.peek();     // View top element without removal
String popped = stack.pop();   // Remove and return top element
boolean empty = stack.isEmpty(); // Check if empty
int size = stack.size();       // Number of elements
int position = stack.search("first"); // 1-based position from top, -1 if not found
```

**Use Cases:**

- Expression evaluation, undo mechanisms, and backtracking algorithms.

**Note:**

- `Stack` extends `Vector`, which is synchronized, so it might be slower than using `Deque` implementations for stack operations.

**Example: Reverse a String**

```java
Stack<Character> stack = new Stack<>();
for (char c : "hello".toCharArray()) {
    stack.push(c);
}
StringBuilder reversed = new StringBuilder();
while (!stack.isEmpty()) {
    reversed.append(stack.pop());
}
System.out.println(reversed); // "olleh"
```

---

## Queue and Deque

```java
import java.util.Queue;
import java.util.LinkedList;
import java.util.Deque;
import java.util.ArrayDeque;

// Queue implementation using LinkedList
Queue<String> queue = new LinkedList<>();
queue.offer("first");           // Add to end (preferred over add())
queue.offer("second");
String front = queue.peek();    // View front element
String removed = queue.poll();  // Remove and return front element
boolean empty = queue.isEmpty();

// Deque (double-ended queue) implementation
Deque<String> deque = new ArrayDeque<>();

// Add operations
deque.addFirst("first");        // Add to front
deque.addLast("last");          // Add to end
deque.offerFirst("new first");  // Add to front (preferred)
deque.offerLast("new last");    // Add to end (preferred)

// View operations (without removal)
String peekFirst = deque.peekFirst(); // View front element
String peekLast = deque.peekLast();   // View last element

// Remove operations
String pollFirst = deque.pollFirst(); // Remove and return front
String pollLast = deque.pollLast();   // Remove and return last

// Use as stack
deque.push("top");              // Same as addFirst
String popped = deque.pop();    // Same as removeFirst
```

**Circular Buffers:**

- `ArrayDeque` can be used as a circular buffer by adding and removing from both ends.

**Example:**

```java
Deque<String> buffer = new ArrayDeque<>(3);
buffer.addLast("a");
buffer.addLast("b");
buffer.addLast("c");
buffer.addLast("d"); // "a" is removed, buffer now ["b", "c", "d"]
System.out.println(buffer);
```

---

## Collections Utility Methods

```java
import java.util.Collections;
import java.util.List;
import java.util.ArrayList;

List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 1, 5, 9));

// Sorting
Collections.sort(list);                        // Natural order
Collections.sort(list, Collections.reverseOrder()); // Reverse order
Collections.sort(list, (a, b) -> a % 2 - b % 2); // Custom comparator

// Searching (list must be sorted for binarySearch)
int index = Collections.binarySearch(list, 4); // Returns index or negative value

// Min and Max
int min = Collections.min(list);
int max = Collections.max(list);
int customMin = Collections.min(list, (a, b) -> b - a); // Max using custom comparator

// Modifying operations
Collections.reverse(list);       // Reverses order
Collections.shuffle(list);       // Random order
Collections.swap(list, 0, 1);    // Swap elements at indices
Collections.fill(list, 0);       // Fill with value
Collections.copy(dest, src);     // Copy src to dest (dest must be sized already)
Collections.replaceAll(list, 1, 99); // Replace all occurrences of 1 with 99
Collections.rotate(list, 2);     // Rotate right by distance

// Collection creation
List<String> singletonList = Collections.singletonList("one"); // Immutable one element
Set<String> singleton = Collections.singleton("one");
Map<String, Integer> singletonMap = Collections.singletonMap("one", 1);
List<Integer> emptyList = Collections.emptyList();

// Unmodifiable views
List<Integer> unmodifiableList = Collections.unmodifiableList(list);
```

**Note:**

- `Collections.sort` works with `List`, not arrays. For arrays, use `Arrays.sort`.

**Practical Example:**

- Using `Collections.binarySearch` after sorting:

```java
Collections.sort(list);
int index = Collections.binarySearch(list, 4);
if (index >= 0) {
    System.out.println("Found at index " + index);
} else {
    System.out.println("Not found, insertion point: " + (-index - 1));
}
```

---

## Regular Expressions

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

// Pattern compilation
Pattern pattern = Pattern.compile("a*b");

// Matching
Matcher matcher = pattern.matcher("aaaab");
boolean matches = matcher.matches();  // true (entire string must match)
boolean found = matcher.find();       // Find next match within string

// Groups
Pattern groupPattern = Pattern.compile("(\\d{3})-(\\d{3})-(\\d{4})");
Matcher m = groupPattern.matcher("123-456-7890");
if (m.matches()) {
    String areaCode = m.group(1);    // "123"
    String exchange = m.group(2);    // "456"
    String lineNum = m.group(3);     // "7890"
}

// Replacement
String result = "date: 2023-05-01".replaceAll("\\d{4}-\\d{2}-\\d{2}", "YYYY-MM-DD");

// Split with pattern
String[] parts = "a,b;c".split("[,;]");  // ["a", "b", "c"]
```

**Splitting with Special Characters:**

- Use `\\` to escape special characters or `Pattern.quote()`:

```java
String[] parts = "a.b.c".split("\\."); // ["a", "b", "c"]
String[] parts2 = "a.b.c".split(Pattern.quote(".")); // ["a", "b", "c"]
```

**Performance Tip:**

- Compile patterns once and reuse them for efficiency in loops.

**Example:**

```java
Pattern pattern = Pattern.compile("\\d+");
String[] numbers = pattern.split("1a2b3c"); // ["", "a", "b", "c"]
```

---


# Java Random Number Generation Guide

## Basic Usage

```java
Random rand = new Random();  // Create a Random object

// Basic random values
int a = rand.nextInt();      // Any int (positive or negative)
int b = rand.nextInt(5);     // 0-4 inclusive
long c = rand.nextLong();    // Any long
float d = rand.nextFloat();  // 0.0 ≤ d < 1.0
double e = rand.nextDouble(); // 0.0 ≤ e < 1.0
double f = rand.nextGaussian(); // Gaussian distribution with mean=0 and std=1
boolean g = rand.nextBoolean(); // true or false
```

## Common Range Patterns

### Integer Ranges

```java
// Random int between 10 (inclusive) and 20 (exclusive)
int h = 10 + rand.nextInt(10);  // [10, 19]

// Random int between min (inclusive) and max (inclusive)
int min = 5, max = 15;
int randomInRange = min + rand.nextInt(max - min + 1);  // [5, 15]
```

### Floating-Point Ranges

```java
// Random float between 1.5 and 2.5
float i = 1.5f + rand.nextFloat() * (2.5f - 1.5f);  // [1.5, 2.5)

// Random double between min and max
double min = 10.5, max = 20.5;
double randomDouble = min + rand.nextDouble() * (max - min);  // [min, max)
```

### Long Ranges

```java
// Random long between 100 and 200
long lower = 100L, upper = 200L;
long j = lower + (Math.abs(rand.nextLong()) % (upper - lower + 1));  // [100, 200]

// Better approach for large ranges (avoids modulo bias)
long randomLong = lower + (long)(rand.nextDouble() * (upper - lower + 1));
```

## Character and String Generation

```java
// Random uppercase letter A-Z
char k = (char) ('A' + rand.nextInt(26));

// Random lowercase letter a-z
char l = (char) ('a' + rand.nextInt(26));

// Random alphanumeric string of length 5
String alphaNum = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
StringBuilder sb = new StringBuilder(5);
for (int i = 0; i < 5; i++) {
    sb.append(alphaNum.charAt(rand.nextInt(alphaNum.length())));
}
String randomString = sb.toString();
```

## Array Operations

```java
// Shuffle an array (Fisher-Yates algorithm)
int[] arr = {1, 2, 3, 4, 5};
for (int i = arr.length - 1; i > 0; i--) {
    int j = rand.nextInt(i + 1);
    // Swap elements
    int temp = arr[i]; 
    arr[i] = arr[j]; 
    arr[j] = temp;
}

// Pick a random element from array
int[] pool = {10, 20, 30, 40, 50};
int randomPick = pool[rand.nextInt(pool.length)];
```

## Using java.util.Collections

```java
// Shuffle a list
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
Collections.shuffle(numbers, rand);  // Pass Random instance for reproducibility
```

## Pitfalls to Avoid

```java
// Limited range can reduce randomness
int a = rand.nextInt(1);  // Always returns 0

// Zero bound will cause exception
// int b = rand.nextInt(0);  // Throws IllegalArgumentException
```

---


## Date and Time (pre-Java 8)

```java
import java.util.Date;
import java.util.Calendar;
import java.text.SimpleDateFormat;

// Current date and time
Date now = new Date();

// Specific date
Calendar cal = Calendar.getInstance();
cal.set(2023, Calendar.MAY, 1, 12, 30, 0); // Year, month, day, hour, min, sec
Date specificDate = cal.getTime();

// Date formatting
SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String formattedDate = formatter.format(now);  // "2023-05-01 12:30:00"

// Parsing dates
try {
    Date parsedDate = formatter.parse("2023-05-01 12:30:00");
} catch (ParseException e) {
    e.printStackTrace();
}

// Date manipulation
Calendar calendar = Calendar.getInstance();
calendar.setTime(now);
calendar.add(Calendar.DAY_OF_MONTH, 5);  // Add 5 days
calendar.add(Calendar.HOUR, -2);         // Subtract 2 hours
Date modifiedDate = calendar.getTime();

// Date comparison
boolean before = date1.before(date2);
boolean after = date1.after(date2);
long diffMs = date2.getTime() - date1.getTime(); // Difference in milliseconds
```

**Note:**

- `Calendar.MONTH` is 0-based (January = 0), but `Calendar.DAY_OF_MONTH` is 1-based.

**Modern Alternative:**

- Use `java.time` package in Java 8+ for better date/time handling.

**Example:**

```java
import java.time.LocalDate;
import java.time.LocalDateTime;
LocalDate date = LocalDate.of(2023, 5, 1);
LocalDateTime dateTime = LocalDateTime.of(2023, 5, 1, 12, 30);
```

---

## Custom Data Structure Implementations

### Custom LinkedList

```java
class Node {
    int data;
    Node next;
    
    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedList {
    Node head;
    
    void add(int data) {
        Node newNode = new Node(data);
        if (head == null) {
            head = newNode;
            return;
        }
        
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        current.next = newNode;
    }
    
    void print() {
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
}
```

### Custom DoublyLinkedList

```java
class DNode {
    int data;
    DNode prev;
    DNode next;
    
    DNode(int data) {
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}

class DoublyLinkedList {
    DNode head;
    DNode tail;
    
    void add(int data) {
        DNode newNode = new DNode(data);
        
        if (head == null) {
            head = tail = newNode;
            return;
        }
        
        tail.next = newNode;
        newNode.prev = tail;
        tail = newNode;
    }
}
```

### Custom Binary Tree

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}

class BinaryTree {
    TreeNode root;
    
    void inorderTraversal(TreeNode node) {
        if (node == null) return;
        
        inorderTraversal(node.left);
        System.out.print(node.val + " ");
        inorderTraversal(node.right);
    }
    
    void preorderTraversal(TreeNode node) {
        if (node == null) return;
        
        System.out.print(node.val + " ");
        preorderTraversal(node.left);
        preorderTraversal(node.right);
    }
    
    void postorderTraversal(TreeNode node) {
        if (node == null) return;
        
        postorderTraversal(node.left);
        postorderTraversal(node.right);
        System.out.print(node.val + " ");
    }
}
```

**Additional Custom Structures:**

- **Trie (Prefix Tree)**: For efficient string searches.

**Example: Simple Trie Node**

```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord;
}

class Trie {
    TrieNode root = new TrieNode();
    
    void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            node.children.putIfAbsent(c, new TrieNode());
            node = node.children.get(c);
        }
        node.isEndOfWord = true;
    }
}
```

---

## Time Complexity Reference

### Data Structure Operations

| Data Structure | Access | Search | Insertion | Deletion |
|----------------|--------|--------|-----------|----------|
| ArrayList      | O(1)   | O(n)   | O(n)*     | O(n)*    |
| LinkedList     | O(n)   | O(n)   | O(1)**    | O(1)**   |
| HashMap        | N/A    | O(1)   | O(1)      | O(1)     |
| TreeMap        | N/A    | O(log n) | O(log n) | O(log n) |
| HashSet        | N/A    | O(1)   | O(1)      | O(1)     |
| TreeSet        | N/A    | O(log n) | O(log n) | O(log n) |
| PriorityQueue  | O(1)*** | O(n)   | O(log n)  | O(log n) |

* O(1) amortized for add at end, but O(n) for arbitrary position due to shifting  
** O(1) when adding/removing from ends, but O(n) for arbitrary position due to traversal  
*** Only for the min/max element

### Algorithm Complexities

| Algorithm             | Time Complexity | Space Complexity |
|-----------------------|----------------|-----------------|
| Binary Search         | O(log n)       | O(1)            |
| Bubble Sort           | O(n²)          | O(1)            |
| Insertion Sort        | O(n²)          | O(1)            |
| Selection Sort        | O(n²)          | O(1)            |
| Merge Sort            | O(n log n)     | O(n)            |
| Quick Sort            | O(n log n)*    | O(log n)        |
| Heap Sort             | O(n log n)     | O(1)            |
| BFS (Graph)           | O(V + E)       | O(V)            |
| DFS (Graph)           | O(V + E)       | O(V)            |
| Dijkstra's Algorithm  | O(V²)          | O(V)            |
| Prim's Algorithm      | O(V²)          | O(V)            |
| Floyd-Warshall        | O(V³)          | O(V²)           |

* O(n²) worst case for Quick Sort

### Tree Operations

| Operation         | BST (avg) | BST (worst) | Balanced BST |
|-------------------|-----------|-------------|--------------|
| Search            | O(log n)  | O(n)        | O(log n)     |
| Insert            | O(log n)  | O(n)        | O(log n)     |
| Delete            | O(log n)  | O(n)        | O(log n)     |
| Traversal         | O(n)      | O(n)        | O(n)         |

**Note:** For BST, average case is O(log n), but worst case (skewed tree) is O(n).

### Dynamic Programming Space-Time Tradeoffs

| Problem Type            | Time Before DP | Time After DP | Space |
|-------------------------|----------------|--------------|-------|
| Fibonacci               | O(2ⁿ)         | O(n)         | O(n)  |
| 0/1 Knapsack           | O(2ⁿ)         | O(n·W)       | O(n·W)|
| Longest Common Subseq.  | O(2ⁿ)         | O(n·m)       | O(n·m)|
| Matrix Chain Multiply   | O(n!)         | O(n³)        | O(n²) |
| Palindrome Partition    | O(2ⁿ)         | O(n³)        | O(n²) |
| Egg Drop Problem        | O(2ⁿ)         | O(n·k²)      | O(n·k)|

**Note:** DP memoization reduces time complexity from exponential to polynomial.

---

## Additional Notes

### Modifying Collections During Iteration

- **For-each loop**: Cannot remove elements; can modify mutable objects or replace elements in `List` by tracking index.
- **Iterator**: Can remove elements using `iterator.remove()`; `ListIterator` can add and set elements.

**Example with Iterator:**

```java
Iterator<Integer> iter = list.iterator();
while (iter.hasNext()) {
    Integer num = iter.next();
    if (num % 2 == 0) {
        iter.remove(); // Safely removes even numbers
    }
}
```

**Example with ListIterator:**

```java
ListIterator<String> listIter = list.listIterator();
while (listIter.hasNext()) {
    String s = listIter.next();
    if (s.equals("A")) {
        listIter.set("Modified"); // Replaces "A" with "Modified"
        listIter.add("New");      // Inserts "New" after "Modified"
    }
}
```

### Comparator and Comparable

- **Comparable**: Natural ordering via `compareTo`.
- **Comparator**: Custom ordering for sort methods.

**Example:**

```java
class Person implements Comparable<Person> {
    String name;
    int age;
    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public int compareTo(Person other) {
        return this.name.compareTo(other.name);
    }
}

// Custom comparator by age
Comparator<Person> ageComparator = (p1, p2) -> p1.age - p2.age;
```

### Thread Safety

- Most collections are not thread-safe. Use `Collections.synchronizedList`, `ConcurrentHashMap`, etc., for concurrent access.

**Example:**

```java
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
```

### Serialization

- Many collection classes implement `Serializable`, allowing them to be written to streams.

**Example:**

```java
try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("list.ser"))) {
    oos.writeObject(list);
}
```

---

## Conclusion

This document provides a comprehensive reference for Java data structures and APIs, combining all content from **JAVA-Cheatsheet.md** and **Java_Data_Structures_and_APIs_Reference.markdown**. It includes detailed explanations, code snippets, and examples to help you understand and use these concepts effectively, with additional details and examples added for enhanced clarity and utility.

---
