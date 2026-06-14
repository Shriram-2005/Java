# Java Classes: The Production-Grade Blueprint

You already know that a Class is a blueprint, and an Object is the physical house built from that blueprint.

Now it is time to learn how to write a production-grade blueprint. If you write a sloppy class, every single object you generate from it will be vulnerable, buggy, and hard to maintain.

Here is the complete, production-ready package on Java Classes, including the exact security mechanisms and memory rules you need to pass an Object-Oriented Design interview.

## 1. The Anatomy of a Class

A standard Java class is made of three distinct parts. You must write them in this specific order to meet industry standards:

1. **State (Fields / Instance Variables):** The data the object holds.
2. **Constructors:** The specific instructions on *how* to build the object.
3. **Behavior (Methods):** What the object can do.

Let's build a proper `Employee` class:

```java
public class Employee {
    
    // 1. STATE (Fields)
    private String name;
    private double salary;

    // 2. CONSTRUCTOR (The Builder)
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    // 3. BEHAVIOR (Methods)
    public void giveRaise(double amount) {
        this.salary += amount;
    }
}
```

## 2. Constructors (The Object Builders)

A Constructor is a special block of code that runs **exactly once**—the very millisecond you use the `new` keyword to create an object. Its entire job is to set up the initial state of the object.

**The Strict Rules of Constructors:**

1. They must have the **exact same name** as the Class (case-sensitive).
2. They **never have a return type** (not even `void`).

### The Invisible Default Constructor

What happens if you write a class and forget to write a constructor?
Java actually steps in and silently writes an invisible, empty constructor for you behind the scenes.

```java
public class Car {
    String color;
    // Java secretly adds: public Car() {}
}
// So this still works:
Car myCar = new Car(); 
```

> [!CAUTION]
> **The Interview Trap:** The exact microsecond you write *any* custom constructor, Java deletes the invisible default one. If you write `public Car(String color)`, and then try to call `new Car()`, your code will crash because the empty builder no longer exists!

### Constructor Overloading

Just like methods, you can have multiple builders for the same object, depending on how much data the user has at the time of creation.

```java
public class User {
    String username;
    String status;

    // Builder 1: User provides everything
    public User(String username, String status) {
        this.username = username;
        this.status = status;
    }

    // Builder 2: User only provides a name, we set a default status
    public User(String username) {
        this.username = username;
        this.status = "Offline"; 
    }
}
```

## 3. The `this` Keyword (The Mirror)

Look closely at the constructors above. You see `this.username = username;`. Why do we do this?

This is called solving **Variable Shadowing**.
When you pass a parameter into a constructor called `username`, it has the exact same name as the instance variable at the top of the class. If you just write `username = username;`, Java gets confused. It thinks you are just assigning the parameter to itself, leaving the actual object's memory empty.

The `this` keyword is a mirror. It means **"This exact object I am currently building."**

* `this.username` -> The permanent memory slot inside the object.
* `username` -> The temporary data passed into the constructor.

## 4. Encapsulation (The Vault)

This is the **First Pillar of Object-Oriented Programming**.

If you leave your variables `public`, anyone can reach into your object and change them. They could write `employee.salary = -50000;`.

**Encapsulation** is the act of hiding your data and forcing people to use secure methods to change it.

1. Make all fields `private` (Only the Class itself can see them).
2. Provide `public` **Getters** (to read the data) and **Setters** (to change the data safely).

```java
public class BankAccount {
    // 1. Lock the vault
    private double balance;

    // 2. Provide a secure way to add money
    public void deposit(double amount) {
        if (amount > 0) { // Security check!
            this.balance += amount;
        } else {
            System.out.println("Invalid amount.");
        }
    }

    // 3. Provide a secure way to view the money
    public double getBalance() {
        return this.balance;
    }
}
```

## 5. The Ultimate Trap: `static` vs. Instance Members

Interviewers will relentlessly test your understanding of the `static` keyword.

Normally, every time you use `new`, Java creates a brand-new copy of all the variables for that specific object. But if you add the word `static` to a variable or method, **it no longer belongs to the Object. It belongs to the Class blueprint itself.**

* **Instance Variable:** Every object gets its own copy.
* **Static Variable:** All objects share the *exact same* single memory slot.

```java
public class Player {
    String name;               // Instance variable: Every player has their own name
    static int totalPlayers;   // Static variable: Shared by everyone!
}
```

If one player changes a `static` variable, it instantly changes for every single player in the entire game.
