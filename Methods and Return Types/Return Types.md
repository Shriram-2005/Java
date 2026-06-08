# Method Return Types

A method's return type dictates exactly what kind of data the method will give back to the code that called it. It's a strict contract.

## 1. Returning Data

If a method specifies a return type (like `int`, `String`, `boolean`, or any custom object), it **must** use the `return` keyword to provide a value of that exact type.

```java
public String getGreeting(String name) {
    return "Hello, " + name; // Must return a String
}

public boolean isAdult(int age) {
    return age >= 18; // Must return a boolean
}
```

## 2. The `void` Return Type (The Command)

What if a machine doesn't produce a product? What if its only job is to clean the floor, print a receipt, or save data to a database?

If a method does work but gives nothing back, you use the keyword `void`.

```java
// This method takes a String, prints it, and returns absolutely nothing.
public void printWelcomeMessage(String name) {
    System.out.println("Welcome to the system, " + name + "!");
    // Notice there is NO 'return' statement returning a value here.
}
```

**Critical Distinction:** You cannot save the result of a `void` method into a variable. It produces no data. Attempting to do so is a massive beginner compilation error.

```java
// ERROR: printWelcomeMessage returns void, you cannot assign it to a variable!
// String msg = printWelcomeMessage("Alice"); 
```

## 3. The Interview Traps (Where Code Fails to Compile)

When you write methods on a whiteboard, interviewers will look for these two exact compilation errors regarding return types and statements.

### Trap #1: Missing Return in a Branch

If your return type is anything other than `void`, Java demands a **100% guarantee** that a value is returned, no matter what path the logic takes.

```java
// WILL NOT COMPILE!
public String checkScore(int score) {
    if (score > 50) {
        return "Pass";
    }
    // TRAP: What happens if the score is 40? 
    // The method ends without returning a String. Java panics and refuses to compile.
}
```

**The Fix:** You must provide a default return at the bottom, or an `else` block that also returns a value.

```java
// FIXED
public String checkScore(int score) {
    if (score > 50) {
        return "Pass";
    } else {
        return "Fail";
    }
    // Alternatively: Just return "Fail"; at the end of the method.
}
```

### Trap #2: Unreachable Code

The `return` keyword is an absolute kill switch. The exact microsecond Java hits a `return` statement, the method is destroyed, and execution jumps back to whoever called it.

```java
// WILL NOT COMPILE!
public int calculateTax(int income) {
    return income / 5;
    
    // TRAP: This line is physically impossible to reach because the method 
    // already returned and exited on the line above. 
    // Java considers this a fatal error.
    System.out.println("Tax calculated successfully!"); 
}
```

**The Fix:** Any code that must run should be placed *before* the `return` statement.
