# Polymorphism & Method Overriding: The Third Pillar

You have officially triggered the Third Pillar of Object-Oriented Architecture: **Polymorphism** (which literally translates to "Many Forms").

Inheritance gave you the ability to pass down behaviors from a Parent to a Child. But what happens when the Child doesn't like how the Parent does things?

If a generic `Vehicle` has a `startEngine()` method that makes a loud rumbling noise, an `ElectricCar` (which inherits from `Vehicle`) cannot use that method. It needs to start silently.

To solve this, the child must **Override** the parent's method. Here is the complete, production-ready package on Method Overriding, including the exact rules and the classic "Overriding vs. Overloading" interview traps.

## 1. The Basics and the `@Override` Annotation

When a child class writes a method with the **exact same name and parameters** as a method in its parent class, it successfully overrides it. When you call that method on the child object, Java completely ignores the parent's version and runs the child's version instead.

```java
public class Vehicle {
    public void startEngine() {
        System.out.println("VROOM! Gas engine started.");
    }
}

public class ElectricCar extends Vehicle {
    
    // The @Override annotation is highly recommended, though technically optional.
    @Override 
    public void startEngine() {
        System.out.println("...silent hum... Electric engine started.");
    }
}
```

**Why use `@Override` if it's optional?**
It is a safety net. If you accidentally misspell the method in the child class (e.g., `public void startEngin()`), Java won't override it. It will think you just invented a brand-new method. If you put `@Override` at the top, the compiler checks the parent class, realizes there is no `startEngin()` to override, and throws a compilation error instantly, saving you from a massive bug.

## 2. The `super` Keyword (Combining Behaviors)

Sometimes you don't want to completely destroy the parent's behavior; you just want to *add* to it. You can use `super.methodName()` to execute the parent's logic first, and then execute the child's unique logic.

```java
public class Employee {
    public void logWorkHours() {
        System.out.println("Logged 8 hours of standard work.");
    }
}

public class Manager extends Employee {
    
    @Override
    public void logWorkHours() {
        // 1. Do the normal employee stuff first
        super.logWorkHours(); 
        
        // 2. Add the manager-specific stuff
        System.out.println("Logged 2 hours of administrative meetings.");
    }
}
```

## 3. The Three Ironclad Rules of Overriding

Interviewers will write code that breaks these rules to see if you can spot the compilation errors.

### Rule #1: The Contract Must Match (Signatures)
The method name and the parameters must be 100% identical. If the parent takes an `int`, the child must take an `int`. If you change the parameters, you are not overriding; you are *Overloading* (which we will compare below).

### Rule #2: You Cannot Lock Down Access
You cannot make an overridden method *more private* than the parent's version.

* If the parent method is `protected`, the child method can be `protected` or `public`.
* **TRAP:** If the parent is `public`, and the child tries to make it `private`, the code will refuse to compile. You cannot hide a behavior that the parent promised to the world.

### Rule #3: Covariant Return Types (Advanced)
Usually, the return type must be identical. However, Java allows a "Covariant Return Type." This means the child can return a *subclass* of whatever the parent returned.

```java
public class Factory {
    // Parent returns a generic Vehicle
    public Vehicle manufacture() {
        return new Vehicle();
    }
}

public class CarFactory extends Factory {
    @Override
    // Child is allowed to return a Car, because a Car IS-A Vehicle!
    public Car manufacture() { 
        return new Car();
    }
}
```

## 4. Dynamic Method Dispatch (The Magic of Polymorphism)

This is the ultimate whiteboard question. How does Java actually know which method to run when variables get mixed up?

Remember: The **Reference Variable** (left side) dictates *what methods you are allowed to call*. The **Actual Object** in the Heap (right side) dictates *which version of the method actually runs*.

```java
// Reference is Vehicle, but the physical Object is an ElectricCar
Vehicle myRide = new ElectricCar(); 

// Which one runs? The loud Gas engine or the silent Electric engine?
myRide.startEngine(); 
```

**The Answer:** Java uses "Dynamic Method Dispatch." At runtime, Java follows the remote control (`myRide`) into the Heap memory, looks at the physical object (`ElectricCar`), and says, *"Ah, this is actually an ElectricCar. I will use the ElectricCar's overridden method."* It prints the silent hum.

## 5. The Interview Traps: What CANNOT be Overridden?

Interviewers will ask you to override things that physically cannot be overridden.

### Trap #1: Overriding vs. Overloading
They sound similar but are completely different mechanics.

* **Overloading:** Happens in the *same class*. Same method name, *different parameters*. Decided by the compiler at compile-time.
* **Overriding:** Happens between *Parent and Child classes*. Same method name, *same parameters*. Decided by the JVM at runtime.

### Trap #2: Can you override a `static` method?
**NO.** Remember, `static` means the method belongs to the Class Blueprint, not the Object. Overriding relies on physical objects in the Heap. If a child class writes a static method with the same name as a parent's static method, it is called **Method Hiding**, not overriding. Dynamic Method Dispatch will not work.

### Trap #3: Can you override `private` or `final` methods?
**NO.** 
* `private` methods are completely invisible to the child class, so the child can't override what it can't see.
* `final` methods are specifically locked by the parent. The word `final` literally means *"This is the final version of this method, no child is allowed to change it."*
