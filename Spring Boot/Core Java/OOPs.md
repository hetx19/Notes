### Object-Oriented Programming (OOP)

#java #oop #important #interview

---

### What is OOP? 
Object-Oriented Programming (OOP) is a programming paradigm based on objects and classes.

OOP helps in:
- Code reusability
- Scalability
- Maintainability
- Security
- Real-world modeling

Java is primarily an Object-Oriented language.

---

### Real World Analogy

| Real World    | Programming |
| ------------- | ----------- |
| Car           | Object      |
| Car Blueprint | Class       |
| Driving       | Method      |
| Color, Speed  | Attributes  |

---

### Class and Object

#### Class
A class is a blueprint for creating objects.

##### Example

```java
class Car {
    String color;
    
    void drive() {
        System.out.println("Car is driving");
    }
}
```

---

#### Object
An object is an instance of a class.

```java
Car car1 = new Car();
car1.color = "Red";
car1.drive();
```

---

### OOP Principles
There are 4 major pillars of OOP:
1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

---

### 1. Encapsulation

#### Definition
Binding data and methods together into a single unit.

Encapsulation also means:
- Hiding internal implementation
- Providing controlled access

---

#### Benefits
- Data security
- Better maintainability
- Controlled modification

---

#### Example

```java
class Employee {
    private double salary;
    
    public double getSalary() {
        return salary;
    }
    
    public void setSalary(double salary) {
        if(salary > 0) {
            this.salary = salary;
        }
    }
}
```

---

#### Important Points
- Use `private` variables
- Use getters and setters
- Data hiding achieved

---

### 2. Inheritance

#### Definition
Inheritance allows one class to acquire properties and methods of another class.

---

#### Benefits
- Code reusability
- Reduces duplication
- Improves maintainability

---

#### Example

```java
class Animal {
    void sound() {
        System.out.println("Animal makes sound");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}
```

---

#### Types of Inheritance in Java

| Type         | Supported           |
| ------------ | ------------------- |
| Single       | ✅                   |
| Multilevel   | ✅                   |
| Hierarchical | ✅                   |
| Multiple     | ❌ (through classes) |
| Hybrid       | ❌                   |

---

#### Why Multiple Inheritance is Not Supported?
To avoid ambiguity problem (Diamond Problem).

Java supports multiple inheritance using interfaces.

---

### 3. Polymorphism

#### Definition
Polymorphism means "many forms".
One method can behave differently depending on context.

---

### Types of Polymorphism

#### Compile-Time Polymorphism
(Method Overloading)

##### Example

```java
class MathUtils {
    int add(int a, int b) {
        return a + b;
    }
    
    double add(double a, double b) {
        return a + b;
    }
}
```

---

#### Runtime Polymorphism
(Method Overriding)

##### Example
```java
class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Dog barks");
    }
}
```

---

#### Method Overloading vs Overriding

|Feature|Overloading|Overriding|
|---|---|---|
|Same Class|✅|❌|
|Different Parameters|✅|❌|
|Runtime Decision|❌|✅|
|Inheritance Required|❌|✅|

---

### 4. Abstraction

#### Definition
Hiding implementation details and showing only essential functionality.

---

#### Why Abstraction?
- Reduces complexity
- Improves security
- Easier maintenance

---

### Abstraction Using Abstract Class

#### Example

```java
abstract class Vehicle {
    abstract void start();
    
    void stop() {
        System.out.println("Vehicle stopped");
    }
}
```

---

### Abstraction Using Interface

#### Example

```java
interface Payment {
    void pay();
}

class CreditCardPayment implements Payment {
    @Override
    public void pay() {
        System.out.println("Paid using Credit Card");
    }
}
```

---

### Abstract Class vs Interface

|Feature|Abstract Class|Interface|
|---|---|---|
|Constructors|✅|❌|
|Multiple Inheritance|❌|✅|
|Variables|Any Type|public static final|
|Methods|Abstract + Concrete|Abstract (default/static allowed)|

---

### Constructor

#### Definition
A constructor initializes objects.

---

### Types of Constructors

#### Default Constructor

```java
class Student {
    Student() {
        System.out.println("Default Constructor");
    }
}
```

---

#### Parameterized Constructor

```java
class Student {
    String name;
    
    Student(String name) {
        this.name = name;
    }
}
```

---

### this Keyword

#### Uses
- Refers current object
- Calls current constructor
- Resolves variable conflicts

---

#### Example

```java
class User {
    String name;
    
    User(String name) {
        this.name = name;
    }
}
```

---

### super Keyword

#### Uses
- Refers parent class object
- Calls parent constructor
- Access parent methods

---

#### Example

```java
class Animal {
    Animal() {
        System.out.println("Animal Constructor");
    }
}

class Dog extends Animal {
    Dog() {
        super();
        System.out.println("Dog Constructor");
    }
}
```

---

### Access Modifiers

| Modifier  | Same Class | Same Package | Subclass | Other Package |
| --------- | ---------- | ------------ | -------- | ------------- |
| private   | ✅          | ❌            | ❌        | ❌             |
| default   | ✅          | ✅            | ❌        | ❌             |
| protected | ✅          | ✅            | ✅        | ❌             |
| public    | ✅          | ✅            | ✅        | ✅             |

---

### Static Keyword

#### Static Variable
Shared among all objects.

```java
class Counter {
    static int count = 0;
}
```

---

#### Static Method
Can be called without object.

```java
class Utils {
    static void print() {
        System.out.println("Hello");
    }
}
```

---

### Final Keyword

| Usage          | Meaning         |
| -------------- | --------------- |
| final variable | Constant        |
| final method   | Cannot override |
| final class    | Cannot inherit  |

---

### Association, Aggregation, Composition

---

### Association
Relationship between two classes.

Example:
- Teacher ↔ Student

---

### Aggregation
Weak relationship.

Example:
- Department has Teachers

Teachers can exist independently.

---

### Composition
Strong relationship.

Example:
- House has Rooms

Rooms cannot exist without House.

---

### Object Class Methods
Every class in Java extends `Object`.

Important methods:

|Method|Purpose|
|---|---|
|toString()|String representation|
|equals()|Compare objects|
|hashCode()|Hash value|
|clone()|Create copy|

---

### SOLID Principles Overview

| Principle | Meaning               |
| --------- | --------------------- |
| S         | Single Responsibility |
| O         | Open Closed           |
| L         | Liskov Substitution   |
| I         | Interface Segregation |
| D         | Dependency Inversion  |

Very important for Spring Boot and system design.

---

### OOP in Spring Boot
Spring Boot heavily uses OOP concepts.

| OOP Concept   | Spring Boot Usage |
| ------------- | ----------------- |
| Encapsulation | Services, DTOs    |
| Inheritance   | Base entities     |
| Polymorphism  | Interfaces        |
| Abstraction   | Service layer     |

---

### Common Interview Questions

## Basic
- What is OOP?
- Difference between class and object?
- What are the 4 pillars of OOP?

---

#### Intermediate
- Difference between abstraction and encapsulation?
- Difference between overloading and overriding?
- Why multiple inheritance is not supported?

---

#### Advanced
- Explain runtime polymorphism.
- Difference between abstract class and interface?
- What is composition over inheritance?

---

### Important Best Practices

#### Prefer Composition Over Inheritance
Composition provides:
- Loose coupling
- Better flexibility
- Easier testing

---

#### Use Interfaces
Interfaces improve:
- Scalability
- Extensibility
- Testability

---

#### Keep Classes Small
Follow Single Responsibility Principle.

---

### Memory Tips

| Concept       | Easy Memory Trick |
| ------------- | ----------------- |
| Encapsulation | Data Hiding       |
| Inheritance   | IS-A              |
| Composition   | HAS-A             |
| Polymorphism  | Many Forms        |
| Abstraction   | Hide Complexity   |

---

### Related Topics
- [[Collections]]
- [[Exception Handling]]
- [[Multithreading]]
- [[SOLID Principles]]
- [[Design Patterns]]
- [[Spring IOC]]
- [[Dependency Injection]]

---

### Quick Revision Summary

| Concept       | Key Point           |
| ------------- | ------------------- |
| Class         | Blueprint           |
| Object        | Instance            |
| Encapsulation | Data hiding         |
| Inheritance   | Reusability         |
| Polymorphism  | Multiple behaviors  |
| Abstraction   | Hide implementation |

---