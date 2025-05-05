# Java Data Structures & APIs Reference

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
int[] partial = Arrays.copyOfRange(numbers, 1, 3); // elements at index 1,2

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

// For primitive arrays need to box first
int[] primitives = {5, 2, 8, 1};
Integer[] boxed = new Integer[primitives.length];
for (int i = 0; i < primitives.length; i++) {
    boxed[i] = primitives[i];
}
Arrays.sort(boxed, Collections.reverseOrder());

// Sort 2D array - no boxing needed as int[] is an object
int[][] points = {{3, 4}, {1, 2}, {5, 0}};
Arrays.sort(points, (a, b) -> a[0] - b[0]);  // Sort by first element
```

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
    if (num == 5) iter.remove(); // Safe removal during iteration
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

## TreeSet
```java
import java.util.TreeSet;

TreeSet<Integer> treeSet = new TreeSet<>();

// Basic operations (same as HashSet, but maintains sorted order)
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

## TreeMap
```java
import java.util.TreeMap;

TreeMap<String, Integer> treeMap = new TreeMap<>();

// Basic operations (same as HashMap, but maintains sorted key order)
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

// Unmodifiable views (create views that cannot be modified)
List<Integer> unmodifiableList = Collections.unmodifiableList(list);
```

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
| Dijkstra's Algorithm  | O(V² + E)      | O(V)            |
| Prim's Algorithm      | O(V² + E)      | O(V)            |
| Floyd-Warshall        | O(V³)          | O(V²)           |

* O(n²) worst case for Quick Sort

### Tree Operations

| Operation         | BST (avg) | BST (worst) | Balanced BST |
|-------------------|-----------|-------------|--------------|
| Search            | O(log n)  | O(n)        | O(log n)     |
| Insert            | O(log n)  | O(n)        | O(log n)     |
| Delete            | O(log n)  | O(n)        | O(log n)     |
| Traversal         | O(n)      | O(n)        | O(n)         |

### Dynamic Programming Space-Time Tradeoffs

| Problem Type            | Time Before DP | Time After DP | Space |
|-------------------------|----------------|--------------|-------|
| Fibonacci               | O(2ⁿ)         | O(n)         | O(n)  |
| 0/1 Knapsack           | O(2ⁿ)         | O(n·W)       | O(n·W)|
| Longest Common Subseq.  | O(2ⁿ)         | O(n·m)       | O(n·m)|
| Matrix Chain Multiply   | O(n!)         | O(n³)        | O(n²) |
| Palindrome Partition    | O(2ⁿ)         | O(n³)        | O(n²) |
| Egg Drop Problem        | O(2ⁿ)         | O(n·k²)      | O(n·k)|
