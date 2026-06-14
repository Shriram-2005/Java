# Checked vs. Unchecked Exceptions: The Security Hierarchy

You are hitting the absolute core of Java’s safety mechanics. Before you start passing data around in databases or web APIs, you have to understand how Java categorizes danger.

In Java, not all errors are created equal. The Java Compiler acts as a strict security guard, but it treats different types of errors with entirely different rules. This division is known as **Checked vs. Unchecked Exceptions**.

Here is the complete, production-ready package on the Exception Hierarchy, how to use the `throws` keyword, and the exact interview traps that catch developers off guard.

## 1. The Family Tree (The Hierarchy)

To understand the rules, you first have to understand the family tree. Every single error in Java inherits from a master class called `Throwable`.

* **`Throwable`** (The Cosmic Parent)
  * **`Error`:** Catastrophic system failures (e.g., `OutOfMemoryError`, `StackOverflowError`). You **never** try to catch these. Your app is already dead.
  * **`Exception`:** Recoverable issues. This is where we live.
    * **`RuntimeException`:** A special sub-branch of Exceptions.

How Java treats your code depends entirely on which branch of this tree your exception comes from.

## 2. Checked Exceptions (The "Red Tape")

A **Checked Exception** is any class that inherits from `Exception` but *does not* inherit from `RuntimeException`.

**The Rule:** The Java Compiler strictly checks these **at compile-time**. If you write code that *might* throw a Checked Exception, the compiler puts a red underline beneath your code and absolutely refuses to let you click "Run" until you deal with it.

**Why?** Because these are "External Risks." These are things outside of your control, like the hard drive being full, the network going down, or a database being locked. Java forces you to prepare for the worst.

**Classic Examples:**

* `IOException` (File reading/writing fails)
* `SQLException` (Database query fails)
* `FileNotFoundException`

```java
public void readDocument() {
    // COMPILER ERROR! 
    // Java knows 'FileReader' can throw a FileNotFoundException.
    // It refuses to compile until you wrap this in a try-catch, or use 'throws'.
    FileReader reader = new FileReader("secret.txt"); 
}
```

## 3. Unchecked Exceptions (The "Bug Hunters")

An **Unchecked Exception** is any class that inherits from `RuntimeException`.

**The Rule:** The Java Compiler **ignores these at compile-time**. It trusts that you know what you are doing. If one of these occurs, it explodes at **runtime** while the user is actually using the app.

**Why?** Because these represent **Programming Bugs**. These are logic errors made by the developer. You shouldn't be writing `try-catch` blocks for these; you should be fixing your garbage code!

**Classic Examples:**

* `NullPointerException` (Trying to use a method on a `null` object)
* `ArrayIndexOutOfBoundsException` (Asking for item #10 in an array of 5)
* `ArithmeticException` (Dividing by zero)

```java
public void doMath() {
    // The compiler allows this perfectly! No red underline.
    // But when you run it... KABOOM. Runtime crash.
    int result = 10 / 0; 
}
```

## 4. The `throws` Keyword (Passing the Buck)

What if you are writing a method that connects to a database, and you *know* it might throw a Checked `SQLException`, but you don't want to write a `try-catch` right there? Maybe you want the person *calling* your method to deal with it.

You use the **`throws`** keyword in the method signature. This essentially puts a warning label on your method: *"Warning: If you use me, I might blow up. You handle it."*

```java
// We declare 'throws IOException' to warn whoever calls this method.
public void loadGame() throws IOException {
    // We don't need a try-catch here anymore! We passed the buck.
    FileReader file = new FileReader("save_data.txt");
    System.out.println("Loading...");
}

public void startApp() {
    // Now, the compiler forces startApp() to deal with the problem!
    try {
        loadGame();
    } catch (IOException e) {
        System.out.println("Save file corrupted.");
    }
}
```

## 5. Creating Custom Exceptions

In professional enterprise applications, you rarely rely purely on Java's built-in exceptions. If you are building a banking app, you want exceptions that make sense to your business logic.

**How to make a Custom Checked Exception:**
Inherit from `Exception`. You force other developers to handle it with a try-catch.

```java
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

**How to make a Custom Unchecked Exception:**
Inherit from `RuntimeException`. It will crash the app instantly if thrown.

```java
public class InvalidPasswordException extends RuntimeException {
    public InvalidPasswordException(String message) {
        super(message);
    }
}
```

> [!TIP]
> **How to physically trigger it (`throw` vs `throws`):**
> * `throws` (with an 's'): Used in the method signature to pass the buck.
> * `throw` (no 's'): Used inside the method to physically create and launch the explosive.

```java
public void withdraw(double amount) throws InsufficientFundsException {
    if (amount > 1000) {
        // We literally build the bomb and detonate it right here
        throw new InsufficientFundsException("You don't have enough money!");
    }
}
```

## 6. The Ultimate Interview Trap: Overriding with Exceptions

If an interviewer wants to see if you truly understand Object-Oriented Architecture *combined* with Exceptions, they will ask you about Method Overriding rules.

> [!CAUTION]
> **The Rule:** If a Child class overrides a Parent method, the Child **cannot** throw a *new* or *broader* Checked Exception than the Parent.

```java
public class Parent {
    public void process() throws IOException {
        System.out.println("Processing...");
    }
}

public class Child extends Parent {
    // TRAP! WILL NOT COMPILE! 
    // Exception is the parent of IOException, so it is "broader". 
    // You cannot suddenly add more red tape to an overridden method!
    @Override
    public void process() throws Exception { 
        System.out.println("Child Processing...");
    }
}
```

> [!NOTE]
> *Note: This rule ONLY applies to Checked Exceptions. A Child class is allowed to throw as many new Unchecked (Runtime) exceptions as it wants without violating the contract.*
