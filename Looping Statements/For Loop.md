# Java Traditional For Loop

You use the traditional `for` loop when you know **exactly how many times** you want the loop to run. It condenses the initialization, the condition, and the update into a single clean line.

## 1. Syntax and Usage

**Syntax:** `for (Initialization; Condition; Update)`

```java
// Prints 0, 1, 2, 3, 4
for (int i = 0; i < 5; i++) {
    System.out.println("Processing item " + i);
}
```

> **Pro-Tip:** The variable `i` only exists *inside* the loop. Once the loop finishes, `i` is destroyed (Variable Scope!).

## 2. Loop Control (`break` and `continue`)

Sometimes you need to hijack the flow of a loop from the inside.

* **`break`:** Instantly destroys the loop. Execution jumps completely out of it. (Used heavily in search algorithms when you find what you are looking for).
* **`continue`:** Instantly skips the *rest of the current iteration* and jumps straight back to the top to start the next iteration. (Used to skip bad or irrelevant data).

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue; // Skips 3, goes straight to 4
    }
    if (i == 5) {
        break; // Kills the loop completely before printing 5
    }
    System.out.println(i);
}
// Final Output: 1, 2, 4
```

## 3. The Interview Trap: The Off-By-One Error

When iterating through an array, candidates often write `i <= array.length`.

Because arrays are **Zero-Indexed** (counting starts at 0), the last item is at `array.length - 1`. If an array has 5 items, the indexes are `0, 1, 2, 3, 4`. Trying to access index `5` will throw an `ArrayIndexOutOfBoundsException` and instantly fail your code.

**The Fix:** Always use `< array.length`, not `<=`.

```java
int[] numbers = {10, 20, 30};

// CORRECT
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}

// CRASH! 
// for (int i = 0; i <= numbers.length; i++) { ... }
```
