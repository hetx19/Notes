## Definition
Object-Oriented Programming (OOP) is a programming paradigm that organizes software around **objects**, which combine:
- State (data)
- Behavior (methods)

Instead of focusing on functions and procedures, OOP focuses on modeling real-world entities and their interactions.

---

## Why OOP Exists
As software systems grow:
- Functions become difficult to manage
- Global state becomes dangerous
- Code reuse becomes harder
- Maintenance costs increase

OOP addresses these issues by introducing:
- Encapsulation
- Abstraction
- Inheritance
- Polymorphism

---

## Core Idea
A program is composed of objects.

Example:
Banking System

Objects:
- Customer
- Account
- Transaction
- Card

Each object owns its own data and exposes specific behaviors.

---

## Four Pillars of OOP

### 1. Encapsulation
Bundle data and methods together.

```cpp
class BankAccount {
private:
    double balance;

public:
    void deposit(double amount) {
        balance += amount;
    }
};
```

Benefits:
- Data protection
- Easier maintenance
- Reduced coupling

---

### 2. Abstraction
Hide implementation details.

```cpp
class Shape {
public:
    virtual void draw() = 0;
};
```

Users know WHAT happens.
They don't need to know HOW.

---

### 3. Inheritance
Acquire features from another class.

```cpp
class Animal {
public:
    void eat() {}
};

class Dog : public Animal {
};
```

Benefits:
- Reusability
- Reduced duplication

---

### 4. Polymorphism
One interface, many implementations.

```cpp
Animal* animal = new Dog();
animal->sound();
```

Output depends on runtime type.

---

## OOP in Real Systems
Examples at FAANG scale:

### Uber
Objects:
- Driver
- Rider
- Trip
- Payment

### Amazon
Objects:
- Product
- Cart
- Order
- Customer

### Netflix
Objects:
- User
- Movie
- Subscription
- Recommendation

---

## Advantages of OOP

### Modularity
Components can be developed independently.

### Reusability
Inheritance and composition reduce duplication.

### Maintainability
Changes are localized.

### Scalability
Large systems become easier to manage.

---

## Disadvantages

### Overengineering
Not every problem needs OOP.

### More Memory Usage
Objects require additional metadata.

### Inheritance Complexity
Deep hierarchies become difficult to understand.

---

## FAANG Interview Insights
Candidates are expected to:
- Understand all four pillars deeply
- Know composition vs inheritance
- Design scalable object models
- Apply SOLID principles
- Write maintainable abstractions

---

## Key Takeaways
- OOP models software as interacting objects.
- Objects contain state and behavior.
- The four pillars are Encapsulation, Abstraction, Inheritance, and Polymorphism.
- Modern large-scale systems heavily use OOP concepts.

---
Next Topic - [[Procedural vs OOP]]