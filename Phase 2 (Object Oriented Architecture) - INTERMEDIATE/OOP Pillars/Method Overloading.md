# Method Overloading: Compile-Time Polymorphism

You just conquered Method Overriding (Runtime Polymorphism). Now we are looking at its sibling: **Method Overloading**, which represents **Compile-Time Polymorphism**.

If Overriding is a child deciding to do things differently than their parent, Overloading is a single person having multiple different skills depending on what tools you hand them.

Here is the complete, production-ready package on Method Overloading, including the exact tricks interviewers use to trip you up.

## 1. What is Method Overloading?

Method Overloading happens when you write **multiple methods in the exact same class** that share the **exact same name**, but take **different parameters**.

It exists purely for readability and convenience. The most famous example in Java is `System.out.println()`. You can pass it a String, an int, a boolean, or an Object, and it always works. Under the hood, Java actually has 10 different `println` methods written to handle each specific data type!

```java
public class Calculator {
    
    // Version 1: Adds two integers
    public int add(int a, int b) {
        return a + b;
    }

    // Version 2: Adds three integers (Different number of parameters)
    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // Version 3: Adds two decimals (Different data types)
    public double add(double a, double b) {
        return a + b;
    }
}
```

## 2. Compile-Time Binding (Early Binding)

With Overriding (Parent vs. Child), the JVM has to wait until the code is actually running to look at the Heap memory and figure out which method to call.

With Overloading, the **Compiler figures it out instantly** before you even run the code. When you write `calc.add(5.0, 2.0);`, the compiler looks at the decimals, finds Version 3, and hardwires the connection right then and there.

## 3. The Three Golden Rules of Overloading

To legally overload a method, you **must** change the **Method Signature**. The signature consists of the method name and the parameter list.

You can change the parameter list in three ways:

1. **Change the number of parameters:** `add(int a)` vs. `add(int a, int b)`
2. **Change the data types:** `add(int a)` vs. `add(double a)`
3. **Change the sequence of data types:** `process(String a, int b)` vs. `process(int a, String b)`

### The Ultimate Interview Trap: The Return Type Flaw

Interviewers will write this on a whiteboard and ask, *"Will this compile?"*

```java
public class Printer {
    
    public int print(String text) {
        System.out.println(text);
        return 1;
    }

    // TRAP: Trying to overload by ONLY changing the return type
    public boolean print(String text) {
        System.out.println(text);
        return true;
    }
}
```

> [!CAUTION]
> **Answer:** NO. It will instantly crash the compiler.
> **Why?** Because when you call `printer.print("Hello");` in your code, you don't *have* to save the result to a variable. If you don't save it, the compiler has absolutely no clue if you wanted the `int` version or the `boolean` version. Therefore, **changing only the return type is strictly forbidden.**

## 4. Type Promotion (The Edge Case)

What happens if you call a method with an `int`, but there is no method that exactly matches an `int`? Does it crash?

Not immediately. Java will attempt **Type Promotion** (Widening). It will take your `int` and automatically upgrade it to the next largest size (`long`, then `float`, then `double`) to see if it can find a match.

```java
public class MathEngine {
    
    public void multiply(double a, double b) {
        System.out.println("Double version called.");
    }
}

// In your main method:
MathEngine engine = new MathEngine();
engine.multiply(5, 10); 
```

Even though `5` and `10` are integers, the code compiles perfectly. The compiler promotes them to `5.0` and `10.0` and routes the call to the `double` method.

## 5. The Cheat Sheet: Overloading vs. Overriding

This comparison is arguably the most frequently asked basic OOP question in technical interviews. Memorize these distinctions:

| Feature | Method Overloading | Method Overriding |
| :--- | :--- | :--- |
| **Where does it happen?** | Inside the **same class**. | Between **Parent and Child** classes. |
| **Method Name** | Must be exactly the same. | Must be exactly the same. |
| **Parameters** | Must be **different**. | Must be **exactly the same**. |
| **Return Type** | Can change (but cannot be the *only* thing changed). | Must be the same (or a subclass / covariant). |
| **When is it resolved?** | **Compile-Time** (Early Binding). | **Runtime** (Dynamic Method Dispatch). |
| **Inheritance Required?** | No. | Yes (`extends`). |
