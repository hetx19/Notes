## Class - [[Classes]]
Blueprint for creating objects.

```cpp
class Student {
public:
    string name;
};
```

---

## Object - [[Objects]]
Instance of a class.

```cpp
Student s1;
```

---

## Attribute (Data Member) - [[Attributes and Methods]]
Represents state.

```cpp
string name;
int age;
```

---

## Method (Member Function) - [[Attributes and Methods]]
Represents behavior.

```cpp
void study() {}
```

---

## Instance
Another term for an object.

```cpp
Student s1;
Student s2;
```

Both are instances.

---

## Constructor - [[Constructors]]
Special function invoked when an object is created.

```cpp
Student() {
    cout << "Created";
}
```

---

## Destructor - [[Destructors]]
Special function invoked when an object is destroyed.

```cpp
~Student() {
    cout << "Destroyed";
}
```

---

## Access Modifier - [[Access Modifiers]]
Controls visibility.

### public
Accessible everywhere.

### private
Accessible only within class.

### protected
Accessible within class and derived classes.

---

## Encapsulation
Combining data and methods together while restricting direct access.

---

## Abstraction
Showing essential details while hiding implementation.

---

## Inheritance
Acquiring properties and behaviors from another class.

```cpp
class Dog : public Animal {
};
```

---

## Base Class
Parent class.

```cpp
class Animal {
};
```

---

## Derived Class
Child class.

```cpp
class Dog : public Animal {
};
```

---

## Polymorphism
One interface, many forms.

---

## Method Overloading
Same function name, different parameters.

```cpp
void print(int x);
void print(string s);
```

Compile-time polymorphism.

---

## Method Overriding
Derived class redefines parent behavior.

```cpp
class Animal {
public:
    virtual void sound();
};
```

Runtime polymorphism.

---

## Virtual Function
Allows runtime dispatch.

```cpp
virtual void sound();
```

---

## Pure Virtual Function
Creates an abstract class.

```cpp
virtual void draw() = 0;
```

---

## Abstract Class
Cannot be instantiated.
Contains at least one pure virtual function.

---

## Interface
Contract defining behavior.

Java:
```java
interface Payment {
    void pay();
}
```

C++ equivalent:
Abstract class with pure virtual functions.

---

## Association
Objects know about each other.

Example:
Teacher ↔ Student

---

## Aggregation
Weak HAS-A relationship.

Example:
Department → Professors
Professors can exist independently.

---

## Composition
Strong HAS-A relationship.

Example:
House → Rooms
Destroy house → rooms are destroyed.

---

## Dependency
Temporary usage relationship.

Example:
```cpp
void process(PaymentGateway gateway);
```

---

## Coupling
Measure of dependency between components.

Goal:
Low Coupling

---

## Cohesion
Measure of how related responsibilities are.

Goal:
High Cohesion

---

## SOLID
Five principles of maintainable OOP design.
- Single Responsibility
- Open Closed
- Liskov Substitution
- Interface Segregation
- Dependency Inversion

---

## Most Important FAANG Terms
Memorize deeply:
1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism
5. Composition
6. Aggregation
7. Coupling
8. Cohesion
9. Interface
10. Virtual Function
11. Dependency Injection
12. SOLID Principles

These appear repeatedly in LLD and senior-engineer interviews.