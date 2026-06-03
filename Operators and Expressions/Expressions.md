# Java Expressions

If data types are the nouns of Java, and operators are the verbs, then **Expressions** are the complete sentences. 

An Expression is simply any combination of variables, operators, literals, and method calls that evaluates to a single value. 

For example:
- `5 + 3` is an expression that evaluates to `8`.
- `myBalance > 1000` is an expression that evaluates to `true` or `false`.
- `employee.calculateBonus()` is an expression that evaluates to whatever value that method returns.

## Expressions vs Statements
It is crucial to understand the difference between an Expression and a Statement in Java:
- **Expression:** Evaluates to a value. (e.g., `x + 5`)
- **Statement:** An expression that forms a complete unit of execution. It "does something." You turn an expression into a statement by terminating it with a semicolon `;`. (e.g., `total = x + 5;`)

## The Common Trap: Integer Division
In Java, if you divide an `int` by an `int`, the mathematical expression results in an `int`. Java truncates (chops off) the decimal; it does *not* round up.

```java
int a = 5;
int b = 2;
double result = a / b; 

// result is 2.0, NOT 2.5! 
```
**Why?**
Java calculates the expression `5 / 2` first. Because both are integers, it calculates the int value which is `2`. Only *after* the expression is fully evaluated does it assign it to the `double result` variable, casting `2` into `2.0`.

**The Fix:** 
Force the expression to evaluate as a decimal by making at least one number a decimal first:
```java
double result = (double) a / b; // Now the expression is 5.0 / 2, which equals 2.5
```

## Operator Precedence (Order of Operations)
When building complex expressions, Java follows strict rules on which operators run first (similar to PEMDAS in standard math).

1. `()`, `[]`, `.` (Parentheses, Arrays, Method calls)
2. `++`, `--`, `!` (Increments, Logical NOT)
3. `*`, `/`, `%` (Multiplication, Division, Modulo)
4. `+`, `-` (Addition, Subtraction)
5. `>`, `<`, `>=`, `<=` (Relational)
6. `==`, `!=` (Equality)
7. `&&` (Logical AND)
8. `||` (Logical OR)
9. `=`, `+=`, `-=`, etc. (Assignment)

**Pro-Tip:** Don't try to memorize the entire precedence table perfectly. Just use `(parentheses)`! It makes your expressions infinitely easier for other developers to read and forces the exact execution order you want.
