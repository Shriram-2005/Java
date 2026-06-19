# Recurrence Relations: The Mathematical Proof

In Phase 2, finding the Time Complexity of your code was easy: you just counted the `for` loops. If you saw a nested loop, you knew it was $O(n^2)$.

But recursion doesn't use loops. When a method calls itself, splits data in half, and calls itself again, you cannot just "eyeball" the Time Complexity. You have to translate the code into a mathematical equation.

This equation is called a **Recurrence Relation**.

If you master this, you bypass the hardest hurdle of the placement drive. Interviewers will hand you a block of complex recursive code and ask for the Big O. While other candidates try to guess, you will write out the mathematical proof.

Here is the complete, un-sugarcoated package on Recurrence Relations, the Recursion Tree, and the "Master Theorem" cheat code.

## 1. The Anatomy of a Recurrence Relation

A recurrence relation defines the total time $T(n)$ of an algorithm by breaking it down into two parts: the cost of the recursive calls + the cost of the work done *outside* the recursive calls.

Let's look at the `factorial(n)` method we just wrote:

```java
public int factorial(int n) {
    if (n <= 1) return 1;               // Base case: O(1)
    return n * factorial(n - 1);        // Recursive case: 1 call for (n-1) + O(1) multiplication
}
```

We write this mathematically as:

$$T(n) = T(n-1) + O(1)$$

* **$T(n-1)$**: The time it takes to process the recursive call with the input reduced by 1.
* **$O(1)$**: The time it takes to do the actual multiplication (`n * ...`).

Because we subtract 1 from $N$ every time, this will run $N$ times. Therefore, the total Time Complexity is **$O(n)$**.

## 2. Divide and Conquer (The Real Challenge)

In Phase 3 and Phase 6, we stop subtracting by 1 and start **dividing by 2**. This is the core of "Divide and Conquer" algorithms like Merge Sort and Binary Search.

Let's say you write an algorithm that splits an array in half, runs recursion on both halves, and then spends an $O(n)$ loop merging them back together.

The equation looks like this:

$$T(n) = 2T\left(\frac{n}{2}\right) + O(n)$$

* **$2$**: The number of recursive calls you make (branching left and right).
* **$T(n/2)$**: The size of the data for each call (cut in half).
* **$O(n)$**: The work done *outside* the recursion (looping through the array to merge it).

How do you solve this for Big O? You use the Cheat Code.

## 3. The Master Theorem (The Cheat Code)

In a placement OA, you do not have time to solve heavy algebraic proofs. Computer scientists created **The Master Theorem** to instantly solve any Divide and Conquer recurrence relation.

Memorize this template. Every Divide and Conquer algorithm fits into it:

$$T(n) = aT\left(\frac{n}{b}\right) + O(n^d)$$

* $a$: Number of recursive branches you spawn.
* $b$: The factor by which you divide the data.
* $d$: The power of $N$ for the work done outside recursion (e.g., if you do a standard loop, $d = 1$. If you just do a simple variable swap, $d = 0$).

To find the Big O, you simply compare $a$ to $b^d$. There are exactly three cases:

### Case 1: The Recursion is heavier ($a > b^d$)

The algorithm spends most of its time frantically splitting into smaller branches.

* **The Answer:** $O(n^{\log_b a})$

### Case 2: The Work is balanced ($a = b^d$)

The time spent splitting data and the time spent looping over data is perfectly equal.

* **The Answer:** $O(n^d \log n)$
* *Example (Merge Sort):* $T(n) = 2T(n/2) + O(n^1)$. Here, $a=2, b=2, d=1$. Since $2 = 2^1$, it falls perfectly into Case 2. Merge Sort is $O(n \log n)$.

### Case 3: The Outside loop is heavier ($a < b^d$)

The recursion is minimal, and the algorithm spends most of its time doing heavy loops at the current level.

* **The Answer:** $O(n^d)$

## 4. The Interview Trap: The Exponential Tree

The Master Theorem only works for "Divide and Conquer" (where the data is strictly divided by a fraction like $n/2$ or $n/3$).

What happens if you write a recursive function that makes *multiple* calls, but only subtracts data instead of dividing it? Enter the **Recursive Fibonacci Trap**.

```java
public int fibonacci(int n) {
    if (n <= 1) return n; 
    return fibonacci(n - 1) + fibonacci(n - 2); 
}
```

The Recurrence Relation is:

$$T(n) = T(n-1) + T(n-2) + O(1)$$

> [!CAUTION]
> **The Trap:** Junior developers see $n-1$ and think "Oh, it's $O(n)$."
> **The Reality:** Because every single method call spawns **two more** method calls, the Call Stack splits and multiplies exponentially.

This is exactly why recursive Fibonacci has a Time Complexity of **$O(2^n)$ Exponential Time**. If you type `fibonacci(50)` on your computer right now, your fan will spin up, and your program will likely freeze, because it is trying to execute over 1 quadrillion operations.
