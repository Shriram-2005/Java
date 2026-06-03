# Java If Statements

You now have variables (the nouns) and operators (the verbs). But right now, your code is just a mindless script that runs top-to-bottom. **Control Flow** is how you give your program a brain. It allows your code to make decisions, skip instructions, and react to different data.

## 1. The `if` / `else if` / `else` Ladder (The Bread and Butter)
This is the most common way to make decisions in any programming language. Java evaluates the condition inside the `()`—which must result in a `boolean` (`true` or `false`)—and executes the code inside the `{}`.

```java
int creditScore = 720;

if (creditScore >= 750) {
    System.out.println("Loan approved at 3% interest.");
} else if (creditScore >= 650) {
    System.out.println("Loan approved at 7% interest.");
} else {
    System.out.println("Loan denied.");
}
```

## 2. Nested If Statements
You can place `if` statements inside other `if` statements to check for further conditions. However, be careful not to nest them too deeply, or your code becomes hard to read (known as the "Arrow Anti-Pattern").

```java
boolean hasLicense = true;
int age = 18;

if (hasLicense) {
    if (age >= 18) {
        System.out.println("You can rent a car.");
    } else {
        System.out.println("You have a license, but you are too young to rent.");
    }
} else {
    System.out.println("You must have a license to drive.");
}
```

## 3. The Interview Trap: Missing Braces
In Java, curly braces `{}` are technically optional if your `if` statement only contains one single line of code. Interviewers use this to trick you into misreading code.

```java
boolean isVip = true;

if (isVip)
    System.out.println("Free Drink!");
    System.out.println("Free Dessert!"); // TRAP!
```
**Why people fail this:** Looking at the indentation, you'd think both print statements only happen if `isVip` is `true`. Wrong. Because there are no braces, Java only associates the *first* line with the `if`. The `"Free Dessert!"` line will print for every single person, VIP or not.

**The Golden Rule:** Always use curly braces in production code, even for one-liners.