### Java Collections Framework

#java #collections #important #interview

---

#### What is Collections Framework?
The Java Collections Framework (JCF) is a unified architecture for storing and manipulating groups of objects.

It provides:
- Dynamic data structures
- Ready-made algorithms
- Better performance
- Easier data manipulation

---

#### Why Collections?
Arrays have limitations:
- Fixed size
- No built-in methods
- Difficult insertion/deletion

Collections solve these problems.

---

## Collection Hierarchy

```text
                            Iterable
                               |
                           Collection
                    ┌────────┬──────────┐
                   List     Set       Queue
                    |        |          |
                 ArrayList  HashSet   PriorityQueue
                 LinkedList LinkedHashSet
                 Vector     TreeSet
```

Map is separate from Collection hierarchy.

```text
Map
 ├── HashMap
 ├── LinkedHashMap
 ├── TreeMap
 └── Hashtable
```

---

### List Interface

#### Definition
An ordered collection that allows duplicates.

### Characteristics
- Maintains insertion order
- Allows duplicate elements
- Index-based access

---

#### 1. ArrayList

##### Features
- Dynamic array
- Fast random access
- Slow insertion/deletion in middle

##### Internal Working
Uses resizable array internally.

##### Example

```java
List<String> names = new ArrayList<>();

names.add("John");
names.add("Alice");

System.out.println(names);
```

---

##### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Get       | O(1)       |
| Add End   | O(1)       |
| Insert    | O(n)       |
| Remove    | O(n)       |

---

##### When to Use?
Use when:
- Frequent reads
- Less modifications

---

#### 2. LinkedList

##### Features
- Doubly linked list
- Fast insertion/deletion
- Slow random access

##### Example

```java
LinkedList<Integer> list = new LinkedList<>();

list.add(10);
list.add(20);

System.out.println(list);
```

---

##### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Get       | O(n)       |
| Add       | O(1)       |
| Remove    | O(1)       |

---

##### When to Use?
Use when:
- Frequent insertions/deletions
- Less random access

---

#### 3. Vector

##### Features
- Synchronized
- Thread-safe
- Slower than ArrayList

```java
Vector<Integer> vector = new Vector<>();
```

---

#### ArrayList vs LinkedList

| Feature        | ArrayList     | LinkedList         |
| -------------- | ------------- | ------------------ |
| Data Structure | Dynamic Array | Doubly Linked List |
| Access Speed   | Fast          | Slow               |
| Insert/Delete  | Slow          | Fast               |
| Memory Usage   | Low           | High               |

---

### Set Interface

#### Definition
A collection that does NOT allow duplicates.

---

#### 1. HashSet

##### Features
- No duplicates
- Unordered
- Uses hashing

##### Example

```java
Set<Integer> set = new HashSet<>();

set.add(10);
set.add(20);
set.add(10);

System.out.println(set);
```

Output:

```text
[10, 20]
```

---

##### Internal Working
Uses `HashMap` internally.

---

#### 2. LinkedHashSet

##### Features
- Maintains insertion order
- No duplicates

```java
Set<String> set = new LinkedHashSet<>();
```

---

#### 3. TreeSet

##### Features
- Sorted order
- Implements Red-Black Tree

```java
Set<Integer> set = new TreeSet<>();
```

---

##### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Add       | O(log n)   |
| Remove    | O(log n)   |
| Search    | O(log n)   |

---

#### HashSet vs LinkedHashSet vs TreeSet

| Feature      | HashSet | LinkedHashSet | TreeSet |
| ------------ | ------- | ------------- | ------- |
| Order        | No      | Insertion     | Sorted  |
| Performance  | Fastest | Fast          | Slower  |
| Null Allowed | 1       | 1             | ❌       |

---

### Queue Interface

#### Definition
Follows FIFO (First In First Out).

---

#### PriorityQueue

##### Features
- Elements sorted by priority
- Min-heap internally

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();

pq.add(30);
pq.add(10);
pq.add(20);

System.out.println(pq.poll());
```

Output:

```text
10
```

---

### Map Interface

#### Definition
Stores key-value pairs.
Keys are unique.

---

#### 1. HashMap

##### Features
- Unordered
- Allows one null key
- Fast performance

##### Example

```java
Map<Integer, String> map = new HashMap<>();

map.put(1, "John");
map.put(2, "Alice");

System.out.println(map);
```

---

##### Internal Working
###### Java 7
Uses:
- Array + Linked List

###### Java 8
Uses
- Array + Linked List + Balanced Tree

Tree conversion improves performance.

---

##### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Put       | O(1)       |
| Get       | O(1)       |
| Remove    | O(1)       |

Worst case:

```text
O(n)
```

---

#### 2. LinkedHashMap

##### Features
- Maintains insertion order

---

#### 3. TreeMap

##### Features
- Sorted keys
- Red-Black Tree

```java
Map<Integer, String> map = new TreeMap<>();
```

---

#### 4. Hashtable

##### Features
- Thread-safe
- Legacy class
- No null key/value

---

#### HashMap vs Hashtable

|Feature|HashMap|Hashtable|
|---|---|---|
|Thread Safe|❌|✅|
|Performance|Fast|Slow|
|Null Key|✅|❌|

---

### Comparable vs Comparator

#### Comparable
Used for natural sorting.

```java
class Student implements Comparable<Student> {
    int age;

    @Override
    public int compareTo(Student s) {
        return this.age - s.age;
    }
}
```

---

#### Comparator
Custom sorting logic.

```java
Comparator<Student> comp = (a, b) -> a.age - b.age;
```

---

#### Comparable vs Comparator

| Feature | Comparable  | Comparator |
| ------- | ----------- | ---------- |
| Package | java.lang   | java.util  |
| Method  | compareTo() | compare()  |
| Sorting | Natural     | Custom     |

---

### Iterator

#### Definition
Used to iterate collections.

```java
Iterator<String> itr = list.iterator();

while(itr.hasNext()) {
    System.out.println(itr.next());
}
```

---

### Fail Fast vs Fail Safe

#### Fail Fast
Throws exception during modification.

Example:
- ArrayList iterator

```java
ConcurrentModificationException
```

---

#### Fail Safe
Works on cloned collection.

Example:
- CopyOnWriteArrayList

---

### Generics

#### Definition
Allows type safety.

```java
List<String> names = new ArrayList<>();
```

Benefits:
- Compile-time safety
- No casting required

---

### Immutable Collections

#### Java 9 Factory Methods

```java
List<String> list = List.of("A", "B", "C");
```

---

### Collections Utility Class

#### Common Methods

```java
Collections.sort(list);
Collections.reverse(list);
Collections.shuffle(list);
```

---

### Concurrent Collection
Used in multithreading environments.

Examples:
- ConcurrentHashMap
- CopyOnWriteArrayList
- BlockingQueue

---

### Synchronization in Collections

#### Synchronized List

```java
List<Integer> list = Collections.synchronizedList(new ArrayList<>());
```

---

### Important Interview Questions

#### Basic
- Difference between List and Set?
- Difference between ArrayList and LinkedList?
- What is HashMap?

---

#### Intermediate
- How HashMap works internally?
- Difference between HashSet and TreeSet?
- What is fail-fast iterator?

---

#### Advanced
- Explain ConcurrentHashMap.
- Why equals() and hashCode() are important?
- Difference between Comparable and Comparator?

---

### equals() and hashCode()

#### Rule
If two objects are equal:

```text
hashCode must also be equal
```

---

#### Example

```java
class Employee {
    int id;

    @Override
    public boolean equals(Object o) {
        return true;
    }

    @Override
    public int hashCode() {
        return id;
    }
}
```

---

### Best Practices

#### Prefer Interface Reference

```java
List<String> list = new ArrayList<>();
```

Instead of:

```java
ArrayList<String> list = new ArrayList<>();
```

---

#### Use Appropriate Collection

| Use Case    | Best Choice       |
| ----------- | ----------------- |
| Fast Search | HashSet           |
| Sorted Data | TreeSet           |
| Key-Value   | HashMap           |
| Thread Safe | ConcurrentHashMap |

---

### Collections in Spring Boot
Collections are heavily used in:
- Dependency Injection
- Request Parameters
- DTOs
- Repository Results

Example:

```java
List<User> users = userRepository.findAll();
```

---

### Memory Tips

| Collection | Memory Trick |
| ---------- | ------------ |
| List       | Ordered      |
| Set        | Unique       |
| Queue      | FIFO         |
| Map        | Key-Value    |

---

### Related Topics
- [[OOPs]]
- [[Streams]]
- [[Multithreading]]
- [[Exception Handling]]
- [[Generics]]

---

### Quick Revision Summary

| Collection | Property           |
| ---------- | ------------------ |
| ArrayList  | Fast Access        |
| LinkedList | Fast Insert/Delete |
| HashSet    | Unique Elements    |
| TreeSet    | Sorted Set         |
| HashMap    | Key-Value Storage  |
| TreeMap    | Sorted Map         |

---