## Procedural Programming
Procedural Programming organizes code around:
- Functions
- Procedures
- Sequential execution

Example:

```cpp
void deposit(Account& acc, double amount) {
    acc.balance += amount;
}
```

Data and behavior are separate.

---

## Object-Oriented Programming
OOP organizes code around objects.

```cpp
class Account {
private:
    double balance;

public:
    void deposit(double amount) {
        balance += amount;
    }
};
```

Data and behavior are bundled together.

---

## Comparison

| Feature       | Procedural | OOP     |
| ------------- | ---------- | ------- |
| Focus         | Functions  | Objects |
| Data Security | Low        | High    |
| Reusability   | Limited    | High    |
| Scalability   | Moderate   | High    |
| Maintenance   | Harder     | Easier  |
| Abstraction   | Weak       | Strong  |
| Encapsulation | No         | Yes     |

---

## Example: Banking System

### Procedural Approach

```cpp
struct Account {
    double balance;
};

void deposit(Account& acc, double amount) {
    acc.balance += amount;
}
```

Problem:
Any function can modify balance.

---

### OOP Approach

```cpp
class Account {
private:
    double balance;

public:
    void deposit(double amount) {
        balance += amount;
    }
};
```

Benefit:
Control over state changes.

---

## Real-World Analogy

### Procedural
Recipe Book

Follow steps one after another.

### OOP
Restaurant

Objects:
- Chef
- Customer
- Waiter
- Order

Each object has responsibilities.

---

## When Procedural Programming Works Well
- Small scripts
- Competitive programming
- Utility tools
- Data transformation tasks

---

## When OOP Works Well
- Enterprise applications
- Backend systems
- Games
- Mobile apps
- Distributed systems

---

## FAANG Perspective
Interviewers usually prefer:
- Strong object modeling
- Encapsulation
- Clear ownership of responsibilities

Most Low-Level Design (LLD) interviews rely heavily on OOP.

Examples:
- Parking Lot Design
- Elevator System
- Library Management
- Ride Sharing Service

---

## Common Interview Question
Why not make everything public?

Answer:
Because encapsulation prevents invalid state and reduces coupling between components.

---

## Key Takeaways
- Procedural programming focuses on functions.
- OOP focuses on objects.
- OOP scales better for large systems.
- Modern software engineering heavily uses OOP principles.

---

Next Topic - [[OOP Terminology]]