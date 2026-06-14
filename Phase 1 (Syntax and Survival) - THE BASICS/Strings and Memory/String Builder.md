# Java StringBuilder: The Mutable String

Since standard `String` objects are strictly immutable, looping and concatenating strings using `+` creates dead objects in the Heap and triggers memory spikes.

To solve this, Java provides the **Mutable** (changeable) `StringBuilder` class. It allows you to open the memory box and change the actual text inside without creating a new object.

## 1. The Architecture (How it works under the hood)

Under the hood, `StringBuilder` is not a magical text box. It is simply an **Array of Characters** (`char[]`). But unlike standard Java arrays that have a fixed size, this class is built to dynamically resize itself automatically.

This brings us to the most common technical question: **Length vs. Capacity**.

* **Length:** How many actual characters are currently typed inside.
* **Capacity:** How many empty boxes the array has currently reserved in memory.

When you create a blank `StringBuilder`, Java gives it a default capacity of **16 empty slots**.

```java
StringBuilder sb = new StringBuilder();
System.out.println(sb.length());   // 0 (No text inside)
System.out.println(sb.capacity()); // 16 (Room for 16 characters)

sb.append("Hello");
System.out.println(sb.length());   // 5
System.out.println(sb.capacity()); // 16 (Still hasn't run out of room)
```

### The Expansion Formula Trap

What happens when you exceed the capacity? Java has to pause, create a brand-new, larger array, copy the old letters over, and delete the small array.
Interviewers will ask: *"By how much does the capacity increase?"*

**The Formula:** `(Old Capacity * 2) + 2`

If you are at 16, and you add a 17th character, the new capacity becomes `(16 * 2) + 2 = 34`.

## 2. The "Big Four" Methods

Because `StringBuilder` is mutable, it has powerful built-in methods that a standard `String` does not have. You will use these heavily in Data Structures and Algorithms (DSA) interviews.

```java
StringBuilder sb = new StringBuilder("Java");

// 1. append(): Adds to the end (The most common operation)
sb.append(" is hard"); // "Java is hard"

// 2. insert(): Pushes text into a specific index without overwriting
sb.insert(4, " 17"); // "Java 17 is hard"

// 3. delete(): Removes a range of characters (start is inclusive, end is exclusive)
sb.delete(5, 7); // "Java  is hard"

// 4. reverse(): The ultimate DSA cheat code! 
sb.reverse(); // "drah si  avaJ"
```

> **Pro-Tip:** If an interviewer asks you to write a method to check if a String is a Palindrome (reads the same backward and forward), do not write a complex `for` loop with two pointers unless they force you to.
> Write this one-liner: `return str.equals(new StringBuilder(str).reverse().toString());`

## 3. The `.equals()` Interview Trap

This is the exact trap that causes candidates to fail their core Java screening.

You already know that for a standard `String`, the `.equals()` method compares the actual text inside the box.
**`StringBuilder` does NOT override the `.equals()` method.**

This means if you try to use `.equals()` on two builders, it defaults to checking their memory addresses (just like the `==` operator).

```java
StringBuilder b1 = new StringBuilder("Apple");
StringBuilder b2 = new StringBuilder("Apple");

// TRAP! This prints FALSE!
System.out.println(b1.equals(b2)); 

// The Fix: You MUST convert them to standard Strings first to compare the text
System.out.println(b1.toString().equals(b2.toString())); // TRUE
```
