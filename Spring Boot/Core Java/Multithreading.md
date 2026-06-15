### Java Multithreading

#java #multithreading #concurrency #important #interview

---

### What is Multithreading?
Multithreading allows multiple threads to execute concurrently.

Thread = lightweight sub-process.

---

### Why Multithreading?

Benefits:
- Better CPU utilization
- Faster execution
- Parallel task processing
- Improved responsiveness    

---

### Process vs Thread

| Feature       | Process  | Thread |
| ------------- | -------- | ------ |
| Memory        | Separate | Shared |
| Communication | Slow     | Fast   |
| Creation Cost | High     | Low    |

---

### Thread Lifecycle

```text
NEW → RUNNABLE → RUNNING → WAITING/BLOCKED → TERMINATED
```

---

### Creating Threads

#### 1. Extending Thread Class

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread running");
    }
}
```

Usage:

```java
MyThread t = new MyThread();
t.start();
```

---

#### 2. Implementing Runnable

Preferred approach.

```java
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Task running");
    }
}
```

Usage:

```java
Thread t = new Thread(new MyTask());
t.start();
```

---

### Why Runnable Preferred?
Because Java does not support multiple inheritance.

Runnable:
- Better design
- Flexible
- Reusable    

---

### Thread Methods

| Method      | Purpose             |
| ----------- | ------------------- |
| start()     | Starts thread       |
| run()       | Thread logic        |
| sleep()     | Pause thread        |
| join()      | Wait for completion |
| interrupt() | Interrupt thread    |

---

### sleep()

```java
Thread.sleep(1000);
```

Pauses current thread for 1 second.

---

### join()

```java
t1.join();
```

Main thread waits until `t1` completes.

---

### Thread Priority

```java
t.setPriority(Thread.MAX_PRIORITY);
```

Priority range:

```text
1 to 10
```

---

### Synchronization

#### Problem
Multiple threads accessing shared data may cause inconsistency.

---

#### Example Problem

```java
count++;
```

Not atomic.

---

### Synchronized Method

```java
synchronized void increment() {
    count++;
}
```

---

### Synchronized Block

```java
synchronized(this) {
    count++;
}
```

---

### Advantages of Synchronization
- Prevents race condition
- Ensures thread safety    

---

### Race Condition
Occurs when multiple threads modify shared data simultaneously.

---

### Inter Thread Communication

Methods:
- wait()
- notify()
- notifyAll()    

---

### wait()
Releases lock and waits.

```java
wait();
```

---

### notify()
Wakes one waiting thread.

```java
notify();
```

---

### Deadlock

#### Definition
Two or more threads waiting forever for each other.

---

#### Example

```text
Thread 1 holds Lock A and waits for Lock B
Thread 2 holds Lock B and waits for Lock A
```

---

### Preventing Deadlock
- Lock ordering
- Timeout mechanisms
- Avoid nested locks    

---

### Volatile Keyword
Ensures visibility between threads.

```java
volatile boolean running = true;
```

---

### Atomic Classes

Located in:

```text
java.util.concurrent.atomic
```

Example:

```java
AtomicInteger count = new AtomicInteger(0);
count.incrementAndGet();
```

---

### Executor Framework
Introduced for thread pool management.

---

### Why Executor Framework?
Problems with manual threads:
- Expensive creation
- Difficult management
- Resource wastage    

---

### Thread Pool
Collection of reusable threads.

---

### Fixed Thread Pool

```java
ExecutorService service = Executors.newFixedThreadPool(5);
```

---

### Submit Task

```java
service.submit(() -> {
    System.out.println("Task");
});
```

---

### Shutdown Executor

```java
service.shutdown();
```

---

### Callable vs Runnable

|Feature|Runnable|Callable|
|---|---|---|
|Return Value|❌|✅|
|Throws Exception|❌|✅|

---

### Callable Example

```java
Callable<Integer> task = () -> 10;
```

---

### Future
Represents async result.

```java
Future<Integer> future = service.submit(task);

System.out.println(future.get());
```

---

### Concurrent Collections
Thread-safe collections.

Examples:
- ConcurrentHashMap
- CopyOnWriteArrayList
- BlockingQueue    

---

### ConcurrentHashMap
Better than Hashtable.

Features:
- Thread-safe
- High performance
- Segment locking (Java 7)    

---

### CountDownLatch
Allows threads to wait until tasks complete.

```java
CountDownLatch latch = new CountDownLatch(3);
```

---

### Semaphore
Controls access to limited resources.

```java
Semaphore semaphore = new Semaphore(2);
```

---

### ReentrantLock
Alternative to synchronized.

```java
Lock lock = new ReentrantLock();
lock.lock();

try {
    // code
} finally {
    lock.unlock();
}
```

---

### synchronized vs Lock

| Feature     | synchronized | Lock |
| ----------- | ------------ | ---- |
| Flexibility | Low          | High |
| Try Lock    | ❌            | ✅    |
| Fairness    | ❌            | ✅    |

---

### Fork Join Framework
Used for recursive parallel tasks.

```java
ForkJoinPool pool = new ForkJoinPool();
```

---

### CompletableFuture
Used for async programming.

---

#### Example

```java
CompletableFuture.supplyAsync(() -> {
    return "Hello";
}).thenAccept(System.out::println);
```

---

### Parallel Stream
Internally uses ForkJoinPool.

```java
list.parallelStream()
```

---

### Daemon Thread
Background service thread.

```java
t.setDaemon(true);
```

Examples:
- Garbage Collector    

---

### Thread Safety
A class is thread-safe if multiple threads can access it safely.

---

### Immutable Objects
Immutable objects are naturally thread-safe.

Example:

```java
String
```

---

### Common Multithreading Problems

|Problem|Description|
|---|---|
|Race Condition|Shared data inconsistency|
|Deadlock|Infinite waiting|
|Starvation|Thread never gets CPU|
|Livelock|Threads active but stuck|

---

### Real World Uses
- Web servers
- Spring Boot APIs
- Gaming
- Banking systems
- Parallel processing    

---

### Multithreading in Spring Boot
Spring Boot uses multithreading in:
- Async APIs
- Web request handling
- Scheduled tasks
- Kafka consumers    

Example:

```java
@Async
public void sendEmail() {
}
```

---

### Important Interview Questions

#### Basic

- What is thread?
- Difference between process and thread?
- Difference between Runnable and Thread?    

---

#### Intermediate
- What is synchronization?
- What is deadlock?
- Difference between wait and sleep?    

---

#### Advanced
- Explain ExecutorService.
- Difference between synchronized and Lock?
- Explain CompletableFuture.

---

### Best Practices

#### Prefer ExecutorService
Avoid manual thread creation.

---

#### Minimize Shared Mutable State
Use immutable objects where possible.

---

#### Always Release Locks
Use:

```java
finally
```

---

#### Avoid Deadlocks
Acquire locks in consistent order.

---

### Memory Tips

|Concept|Easy Memory|
|---|---|
|synchronized|One at a time|
|volatile|Visibility|
|Atomic|Lock-free|
|Executor|Thread manager|

---

### Related Topics

- [[Collections]]
- [[Streams]]
- [[JVM Memory]]
- [[Spring Boot]]
- [[Concurrency]]

---

### Quick Revision Summary

|Concept|Key Point|
|---|---|
|Thread|Lightweight process|
|Synchronization|Thread safety|
|ExecutorService|Thread pool|
|Callable|Returns value|
|Future|Async result|
|CompletableFuture|Modern async|

---