# Java 1D Arrays

An array is a contiguous (side-by-side) block of memory that holds a fixed number of values of the **exact same type**. 

In Java, arrays are Objects. Even if you make an array of primitive `int`s, the array *itself* lives in the Heap memory, and your variable is just a remote control (reference) pointing to it.

## 1. The Three Ways to Create an Array

```java
// Way 1: The Explicit Way (When you don't know the data yet)
// Creates an array of exactly 5 spots. All spots default to 0.
int[] scores = new int[5]; 
scores[0] = 95; // Assigning data later

// Way 2: The Inline Way (When you know the data immediately)
String[] names = {"Alice", "Bob", "Charlie"};

// Way 3: The Anonymous Way (Useful for returning arrays from methods quickly)
return new double[]{1.5, 2.5, 3.5};
```

## 2. The Default Value Rule

If you create an array using `new int[5]`, Java doesn't leave the memory blank. It actively fills it with defaults to prevent crashes:

* **Number arrays** (`int`, `double`, etc.) default to `0` or `0.0`.
* **`boolean` arrays** default to `false`.
* **Object arrays** (`String`, `Employee`, etc.) default to `null`.

## 3. The `Arrays` Utility Class (Your Best Friend)

Never write manual code to sort or print an array if you don't have to. Java provides a built-in utility class called `java.util.Arrays` that does the heavy lifting.

```java
import java.util.Arrays;

int[] numbers = {5, 2, 8, 1, 9};

// 1. Sorting (Uses a highly optimized Dual-Pivot Quicksort)
Arrays.sort(numbers); 

// 2. Searching (Array MUST be sorted first!)
int index = Arrays.binarySearch(numbers, 8); 

// 3. Printing a 1D Array
// If you just do System.out.println(numbers), it prints garbage memory like "[I@7a81197d"
System.out.println(Arrays.toString(numbers)); // Prints: [1, 2, 5, 8, 9]
```

## 4. The Interview Traps (Where Candidates Fail)

### Trap #1: Array Size is Permanent

Once you create an array using `new int[5]`, you **cannot** change its size. Ever.
If you need space for a 6th item, you must create a brand-new `new int[6]` array, copy all 5 items over manually, and then add the 6th. *(This exact painful process is what `ArrayList` automates for you under the hood).*

### Trap #2: `.length` vs `.length()`

This is the most common compilation error candidates make on whiteboards.

* For **Arrays**, it is a property: `myArray.length` (No parentheses).
* For **Strings**, it is a method: `myString.length()` (Requires parentheses).
