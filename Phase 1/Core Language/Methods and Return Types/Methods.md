# Java Methods

Think of a method as a factory machine. You put raw materials in (Parameters), the machine does specific work (Method Body), and it hands you a finished product (Return Type).

## 1. The Anatomy of a Method

Here is the exact blueprint of a Java method:

```java
// 1      2     3     4                     
public   int   add  (int a, int b) {
    // 5
    int sum = a + b;
    // 6
    return sum; 
}
```

1. **Access Modifier (`public`)**: Who is allowed to use this machine? (We will cover this deeply in Phase 2).
2. **Return Type (`int`)**: The strict contract of what this machine will spit out. If it says `int`, it must give back an `int`. It cannot give back a `String` or a `double`.
3. **Method Name (`add`)**: Always written in camelCase (starts lowercase, new words capitalized). It should usually be an action verb representing what the method does.
4. **Parameters (`(int a, int b)`)**: The raw materials the machine requires to run. If you don't provide exactly two integers, the machine refuses to turn on.
5. **Method Body (`{ ... }`)**: The actual logic and instructions the method performs.
6. **Return Statement (`return sum;`)**: The exact moment the machine hands the finished product back to whoever called it and shuts down.

## 2. Method Overloading (The Interview Favorite)

**Interview Question:** "Can you have two methods with the exact same name in the same class?"

**Answer:** Yes. This is called Method Overloading. 

Java is smart enough to tell them apart as long as their **Parameters (Inputs)** are different. Java looks at the data you are trying to pass in and automatically routes it to the correct version of the method.

```java
// Version 1: Adds two integers
public int add(int a, int b) {
    return a + b;
}

// Version 2: Adds three integers
public int add(int a, int b, int c) {
    return a + b + c;
}

// Version 3: Adds two decimals
public double add(double a, double b) {
    return a + b;
}
```

**The Overloading Rule:** You can change the parameters (different amount, or different data types). You **cannot** overload a method just by changing the Return Type. Java doesn't know which return type you want until after the method finishes, which is too late to determine which method to call.

## 3. Best Practices for Methods

*   **Do One Thing:** A method should have a single, clear responsibility. If your method is `calculateTaxAndPrintReceiptAndSaveToDatabase`, it's doing too much. Break it down!
*   **Keep it Short:** Ideally, a method should be short enough to read on a single screen without scrolling.
*   **Descriptive Names:** Use names that clearly explain what the method does. `getUserData()` is much better than `getData()`.
