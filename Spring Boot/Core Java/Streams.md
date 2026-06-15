### Java Streams API

#java #streams #functionalprogramming #important #interview

---

### What is Stream API?
Stream API was introduced in Java 8.

It is used for:
- Processing collections
- Functional programming
- Declarative coding
- Parallel processing

---

### Why Streams?
Without streams:

```java
List<Integer> nums = Arrays.asList(1,2,3,4);

List<Integer> even = new ArrayList<>();

for(int n : nums) {
    if(n % 2 == 0) {
        even.add(n);
    }
}
```

With streams:

```java
List<Integer> even = nums.stream().filter(n -> n % 2 == 0).toList();
```

Cleaner and more readable.

---

### Stream Characteristics
- Does not store data
- Processes data lazily
- Supports functional style
- Can be parallelized

---

### How Streams Work

```text
Source → Intermediate Operations → Terminal Operation
```

Example:

```java
list.stream().filter(x -> x > 10).map(x -> x * 2).collect(Collectors.toList());
```

---

### Creating Streams

#### From Collection

```java
List<String> list = List.of("A", "B");
Stream<String> stream = list.stream();
```

---

#### From Array

```java
Arrays.stream(arr);
```

---

#### Using Stream.of()

```java
Stream.of(1,2,3,4);
```

---

#### Infinite Streams

```java
Stream.iterate(1, n -> n + 1);
```

---

### Intermediate Operations
These return another stream.

---

#### filter()
Used to filter elements.

```java
list.stream().filter(x -> x > 5);
```

---

#### map()
Transforms elements.

```java
list.stream().map(String::toUpperCase);
```

---

#### sorted()
Sorts elements.

```java
list.stream().sorted();
```

---

#### distinct()
Removes duplicates.

```java
list.stream().distinct();
```

---

#### limit()
Limits stream size.

```java
Stream.iterate(1, n -> n+1).limit(5);
```

---

#### skip()
Skips elements.

```java
list.stream().skip(2);
```

---

### Terminal Operations
Produce final result.

---

#### collect()
Collects stream result.

```java
List<Integer> result = nums.stream().collect(Collectors.toList());
```

---

#### forEach()

```java
nums.stream().forEach(System.out::println);
```

---

#### reduce()
Reduces stream to single value.

```java
int sum = nums.stream().reduce(0, Integer::sum);
```

---

#### count()

```java
long count = nums.stream().count();
```

---

#### anyMatch()

```java
boolean found = nums.stream().anyMatch(x -> x > 10);
```

---

### Stream Pipeline

Example:

```java
List<String> names =
    list.stream()
        .filter(x -> x.length() > 3)
        .map(String::toUpperCase)
        .sorted()
        .toList();
```

---

### Lazy Evaluation
Intermediate operations execute only when terminal operation is called.

---

### Functional Interfaces
A functional interface contains only one abstract method.

Examples:
- Predicate
- Function
- Consumer
- Supplier

---

### Predicate
Takes input and returns boolean.

```java
Predicate<Integer> isEven = n -> n % 2 == 0;
```

---

### Function
Takes input and returns result.

```java
Function<String, Integer> length = s -> s.length();
```

---

### Consumer
Consumes value.

```java
Consumer<String> print = System.out::println;
```

---

### Supplier
Provides value.

```java
Supplier<Double> random = () -> Math.random();
```

---

### Lambda Expressions

#### Definition
Short way of implementing functional interfaces.

---

#### Example

```java
(a, b) -> a + b
```

Equivalent:

```java
public int add(int a, int b) {
    return a + b;
}
```

---

### Method References
Shortcut for lambda expressions.

---

#### Types
##### Static Method Reference

```java
Integer::parseInt
```

---

##### Instance Method Reference

```java
System.out::println
```

---

##### Constructor Reference

```java
ArrayList::new
```

---

### Collectors - [[Collections]]
Utility class for collecting results.

---

#### toList()

```java
.collect(Collectors.toList())
```

---

#### groupingBy()

```java
Map<String, List<Employee>> map = employees.stream().collect(Collectors.groupingBy(Employee::getDept));
```

---

#### counting()

```java
Collectors.counting()
```

---

#### joining()

```java
Collectors.joining(", ")
```

---

### flatMap()
Used to flatten nested collections.

---

#### Example

```java
List<List<String>> list = List.of(
    List.of("A", "B"),
    List.of("C", "D")
);

List<String> result = list.stream().flatMap(Collection::stream).toList();
```

---

### Optional Class
Avoids NullPointerException.

---

#### Example

```java
Optional<String> name = Optional.of("John");
name.ifPresent(System.out::println);
```

---

### Parallel Streams
Used for parallel processing.

```java
nums.parallelStream().forEach(System.out::println);
```

---

### Sequential vs Parallel Stream

|Feature|Sequential|Parallel|
|---|---|---|
|Thread|Single|Multiple|
|Performance|Normal|Faster for large data|
|Order|Maintained|May vary|

---

### Stream vs Collection

|Feature|Stream|Collection|
|---|---|---|
|Storage|❌|✅|
|Traversal|One Time|Multiple|
|Lazy|✅|❌|

---

### Common Stream Operations

| Operation | Purpose           |
| --------- | ----------------- |
| filter    | Filter data       |
| map       | Transform         |
| reduce    | Aggregate         |
| sorted    | Sorting           |
| distinct  | Remove duplicates |

---

### Real World Example

#### Find Even Numbers
```java
List<Integer> even = nums.stream().filter(n -> n % 2 == 0).toList();
```

---

#### Find Max Salary

```java
Optional<Integer> max = salaries.stream().max(Integer::compare);
```

---

### Streams in Spring Boot

Streams are heavily used in:
- DTO mapping
- Data transformation
- API responses
- Filtering repository data

Example:

```java
List<UserDTO> users = entities.stream().map(UserDTO::from).toList();
```

---

### Important Interview Questions

#### Basic
- What is Stream API?
- Difference between map() and filter()?
- What is lambda expression?

---

#### Intermediate
- Difference between stream and parallelStream?
- What is lazy evaluation?
- What is flatMap()?

---

#### Advanced
- Explain internal working of streams.
- Difference between reduce and collect?
- When should parallel streams be avoided?

---

### Best Practices

#### Avoid Side Effects

Bad:

```java
list.stream().forEach(x -> globalList.add(x));
```

---

#### Prefer Method References

```java
list.forEach(System.out::println);
```

---

#### Use Parallel Carefully

Good for:
- CPU intensive tasks
- Large datasets

Avoid for:
- Small collections
- Shared mutable state

---

### Memory Tips

| Concept | Easy Memory |
| ------- | ----------- |
| filter  | Remove      |
| map     | Transform   |
| reduce  | Combine     |
| collect | Store       |

---

### Related Topics
- [[Collections]]
- [[Multithreading]]
- [[Functional Interfaces]]
- [[Lambda Expressions]]
- [[Spring Boot]]

---

### Quick Revision Summary

| Concept  | Key Point       |
| -------- | --------------- |
| Stream   | Data Processing |
| filter   | Select Data     |
| map      | Transform Data  |
| reduce   | Aggregate       |
| collect  | Convert Result  |
| Optional | Avoid Null      |

---