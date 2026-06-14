# Inheritance: The Second Pillar of OOP

Welcome to the Second Pillar of Object-Oriented Architecture: **Inheritance**.

If Encapsulation is about protecting your data, Inheritance is about **eliminating redundant code**.

Imagine you are building a racing game. You need a `Car`, a `Truck`, and a `Motorcycle`. They all have a `speed` variable, an `engine` variable, and a `brake()` method. If you write three separate classes from scratch, you will copy and paste the exact same code three times. If you later find a bug in the `brake()` method, you have to fix it in three different files. This is a maintenance nightmare.

Inheritance solves this by allowing you to create a **Parent Class** (Superclass) that holds the shared logic, and **Child Classes** (Subclasses) that inherit everything the parent has, while adding their own unique features.

Here is the complete, production-ready package on Inheritance and the `extends` keyword.

## 1. The `extends` Keyword (The "IS-A" Relationship)

To make one class inherit from another, you use the `extends` keyword. This establishes a strict **"IS-A" relationship**. A Car *is a* Vehicle.

```java
// 1. THE PARENT (Superclass)
public class Vehicle {
    public int speed;
    
    public void startEngine() {
        System.out.println("Engine is rumbling...");
    }
}

// 2. THE CHILD (Subclass)
// The Car automatically gets 'speed' and 'startEngine()' without writing them!
public class Car extends Vehicle {
    public int numberOfDoors = 4; // Unique to Car
    
    public void playRadio() {
        System.out.println("Playing FM Radio.");
    }
}
```

If you instantiate the child:

```java
Car mySubaru = new Car();
mySubaru.startEngine(); // Works perfectly! Inherited from Vehicle.
mySubaru.playRadio();   // Works perfectly! Unique to Car.
```

## 2. The `super` Keyword (Talking to the Parent)

Just like `this` refers to the current object, `super` refers specifically to the **Parent's portion** of the object.

The most critical use of `super` is in **Constructors**. When you build a `Car`, the `Vehicle` part of the object must be built *first*. You cannot build the roof of a house before you pour the foundation.

> [!IMPORTANT]
> **The Rule:** If the parent class has a parameterized constructor, the child class **must** call it using `super()` on the absolute first line of its own constructor.

```java
public class Vehicle {
    String brand;

    // Parent Constructor requires a brand
    public Vehicle(String brand) {
        this.brand = brand;
    }
}

public class Car extends Vehicle {
    int topSpeed;

    public Car(String brand, int topSpeed) {
        // 1. MUST BE FIRST LINE: Pass the brand up to the Parent builder
        super(brand); 
        
        // 2. Build the Child-specific data
        this.topSpeed = topSpeed; 
    }
}
```

## 3. Method Overriding (The `@Override` Annotation)

What if the Parent provides a method, but the Child wants to do it differently?
For example, a generic `Vehicle` might honk like *"Beep Beep"*, but a `SemiTruck` needs to honk like *"HOOOONNNNK!"*.

You can **Override** the parent's method by writing a method in the child class with the exact same name and parameters.

```java
public class Vehicle {
    public void honk() {
        System.out.println("Beep Beep");
    }
}

public class SemiTruck extends Vehicle {
    
    // The @Override annotation tells the compiler to double-check that 
    // you are actually replacing a parent method, preventing spelling errors.
    @Override 
    public void honk() {
        System.out.println("HOOOONNNNK!");
    }
}
```

*Note: You can still call the parent's original version from inside the child class by using `super.honk();`.*

## 4. The `protected` Access Modifier

In the Encapsulation lesson, we learned that `private` completely locks down a variable. But what if you want to lock down a variable from the outside world, but **allow your child classes to access it**?

You use the `protected` modifier.

* `public`: Anyone can touch it.
* `private`: Only the exact class itself can touch it (Child classes are locked out).
* **`protected`**: Only the class itself, **its children**, and classes in the same package can touch it.

```java
public class Vehicle {
    private String engineSecret; // Car CANNOT see this
    protected String fuelType;   // Car CAN see this!
}
```

## 5. The Interview Traps

Interviewers love testing the absolute boundaries of the `extends` keyword. You must know these rules:

### Trap #1: The Diamond Problem (No Multiple Inheritance)

Can a class extend two parents? `public class FlyingCar extends Car, Airplane {}`

**NO.** Java strictly forbids multiple inheritance of classes.
**Why?** Imagine both `Car` and `Airplane` have a `startEngine()` method. If `FlyingCar` doesn't override it, and you call `myFlyingCar.startEngine()`, Java wouldn't know which parent's version to use. This ambiguity causes fatal crashes. (Java solves this later using Interfaces, which we will cover).

### Trap #2: Private Variables ARE Inherited, but NOT Accessible

If an interviewer asks, *"Are private parent variables inherited by the child?"* The answer is **Yes, but they are invisible.**
When the object is built in the Heap, the private data physically exists inside the child object. However, the child code cannot type `this.privateParentVar`. The child *must* use the parent's `public` getters and setters to interact with it.

### Trap #3: The `final` Keyword

If you want to create a blueprint that is absolutely perfect and you refuse to let anyone ever inherit from it or alter its behavior, you put the `final` keyword on the class.

```java
// No class is legally allowed to write `extends String`
public final class String { ... } 
```
