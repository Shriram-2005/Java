# Java While Loops

Loops allow your program to do the heavy lifting: processing thousands of database rows, searching through massive arrays, or keeping a server running continuously.

The `while` and `do-while` loops are used when you **do not know exactly how many times** the loop needs to run. You just want it to keep going until a specific condition becomes false.

## 1. The `while` Loop (Pre-Test)

Java checks the condition *before* doing any work. If the condition is false from the very beginning, the loop will run **zero** times.

```java
int attempts = 0;
// Useful for reading files or listening for server requests
while (attempts < 3) {
    System.out.println("Connecting to database...");
    attempts++;
}
```

## 2. The `do-while` Loop (Post-Test)

Java does the work first, and *then* checks the condition. This guarantees that the code inside the loop will execute **at least once**, no matter what.

```java
int attempts = 5; // Notice this is already over the limit

do {
    System.out.println("Connecting to database..."); // This will print exactly once!
    attempts++;
} while (attempts < 3);
```

## 3. The Interview Trap

It is a classic whiteboard trap to give a candidate an initial variable that already fails the condition, just to see if they spot that the `do-while` still executes its body at least once.

*   **`while` loop:** Think, "Should I even start?"
*   **`do-while` loop:** Think, "Do it once, then decide if I should do it again."
