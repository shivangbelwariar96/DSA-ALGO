# Java Data Structures & APIs Reference

This document serves as a comprehensive reference for Java data structures, APIs, and related concepts, combining all information from **JAVA-Cheatsheet.md** and **Java_Data_Structures_and_APIs_Reference.markdown** into a single, detailed, and readable Markdown file. All content from both sources is preserved, with additional details and examples added for clarity and completeness.

---

## Table of Contents

1. [Character Class Methods](#character-class-methods)
2. [Number Conversions](#number-conversions)
3. [Math Class](#math-class)
4. [String Methods](#string-methods)
5. [Arrays](#arrays)
6. [ArrayList](#arraylist)
7. [LinkedList](#linkedlist)
8. [HashSet](#hashset)
9. [TreeSet](#treeset)
10. [HashMap](#hashmap)
11. [TreeMap](#treemap)
12. [PriorityQueue](#priorityqueue)
13. [Stack](#stack)
14. [Queue and Deque](#queue-and-deque)
15. [Collections Utility Methods](#collections-utility-methods)
16. [Regular Expressions](#regular-expressions)
17. [Date and Time (pre-Java 8)](#date-and-time-pre-java-8)
18. [Custom Data Structure Implementations](#custom-data-structure-implementations)
19. [Time Complexity Reference](#time-complexity-reference)
20. [Additional Notes](#additional-notes)
21. [Conclusion](#conclusion)

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

## Number Conversions

```java
// String to numeric
int num = Integer.parseInt("123");    // 123
double d = Double.parseDouble("3.14"); // 3.14
long l = Long.parseLong("1234567890"); // 1234567890

// Binary conversions
int binToInt = Integer.parseInt("1010", 2);  // 10
String intToBin = Integer.toBinaryString(10); // "1010"

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
