# Java Operators

Operators are the verbs of your programming language. They command the software to manipulate data. Here is the complete, production-ready package on Java operators, including the exact traps interviewers use to trip candidates up.

## 1. The Daily Drivers (Common Operators)
You will use these in almost every single file you write.

| Category | Operators | What you need to know |
| :--- | :--- | :--- |
| **Arithmetic** | `+`, `-`, `*`, `/`, `%` | The `%` (modulo) operator gives you the remainder of division. It is heavily used in standard algorithms to find even/odd numbers (`x % 2 == 0`). |
| **Assignment** | `=`, `+=`, `-=`, `*=`, `/=` | `x += 5` is just a faster, cleaner way to write `x = x + 5`. |
| **Relational** | `==`, `!=`, `>`, `<`, `>=`, `<=` | These *always* result in a boolean (`true` or `false`). **Warning:** Never use `==` to compare the actual text of `String` class items in Java. Use `.equals()`. |
| **Logical** | `&&` (AND), `\|\|` (OR), `!` (NOT) | Used to chain or invert multiple conditions together in `if` statements. |

---

## 2. Advanced Operators

### The Ternary Operator (`? :`)
This is a miniature, one-line `if/else` statement. It is incredibly useful for writing clean, concise code.
**Syntax:** `condition ? valueIfTrue : valueIfFalse`

```java
int score = 85;
// Instead of a bulky 4-line if/else block:
String status = (score >= 50) ? "Pass" : "Fail"; 
```

### Bitwise Operators (`&`, `|`, `^`, `~`, `<<`, `>>`)
These manipulate the raw binary bits (1s and 0s) of your data. Unless you are writing high-performance low-level systems, cryptography, or solving specific LeetCode hard problems, you won't touch these often.

*Note:* Notice that `&` and `|` are single characters. If you use a single `&` instead of `&&` in an `if` statement, Java will perform a *Bitwise* check instead of a *Logical* check.

---

## 3. The Interview Traps (Where Candidates Fail)
Interviewers love testing your deep understanding of how Java executes operators under the hood. You must master these two traps.

### Trap #1: Short-Circuit Evaluation
When using logical `&&` (AND) and `||` (OR), Java is lazy—in a good way. It stops evaluating an expression the exact millisecond it knows the final answer.
- **With `&&`:** If the left side is `false`, Java knows the whole statement *must* be false. It entirely ignores the right side.
- **With `||`:** If the left side is `true`, Java knows the whole statement *must* be true. It ignores the right side.

```java
String name = null;
// This is SAFE. Because 'name != null' evaluates to false, Java never executes 'name.length()'
// If it didn't short-circuit, 'name.length()' would throw a NullPointerException and crash!
if (name != null && name.length() > 5) {
    System.out.println("Valid name");
}
```
*Note: If you use the single `&` bitwise operator instead, Java will **NOT** short-circuit. It will check both sides no matter what, and the above code would crash.*

### Trap #2: Pre-increment vs. Post-increment
You can increase a variable by 1 using `++x` (Pre) or `x++` (Post). In a vacuum, they do the exact same thing. But when you put them *inside* an expression, they behave completely differently:
- **Pre-increment (`++x`):** Change the value first, *then* use it in the expression.
- **Post-increment (`x++`):** Use the current value in the expression first, *then* change it.

```java
int x = 5;
int y = x++; // y gets 5, then x becomes 6
// Result: x = 6, y = 5

int a = 5;
int b = ++a; // a becomes 6, then b gets 6
// Result: a = 6, b = 6
```