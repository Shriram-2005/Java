# Java Switch Statements

When you have a single variable and you want to check it against many specific, exact values, an `if/else` ladder gets incredibly messy. The `switch` statement was built to solve this.

## 1. The Legacy `switch` Statement (Java 8 and Older)
Legacy Java switch statements are heavily tested in interviews because of a massive architectural flaw: **Fall-Through**.

```java
String day = "Tuesday";

switch (day) {
    case "Monday":
        System.out.println("Start of the week.");
        break; // Crucial!
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
        System.out.println("Midweek grind.");
        break; // Stops the execution
    case "Friday":
        System.out.println("Weekend is near!");
        // TRAP: FORGOT TO PUT 'break;' HERE!
    default:
        System.out.println("It is the weekend!");
}
```

### The Fall-Through Trap
If you forget the `break;` keyword, Java doesn't just stop when it finds a match. It executes the matching case, and then mindlessly executes every single case below it, *ignoring the case labels entirely*, until it hits a `break` or the end of the `switch`.

In the code above, if `day` is `"Friday"`, it will print `"Weekend is near!"` AND `"It is the weekend!"`.

## 2. The Modern `switch` Expression (Java 14+)
Because the legacy `switch` was so bug-prone and ugly, Oracle completely redesigned it. If you write backend code in a modern enterprise, this is what you will use.

It introduces two massive upgrades:
1. **Arrow Syntax (`->`):** Replaces the colon `:`. It completely eliminates "fall-through". You do not need to write `break;` anymore.
2. **Expressions (Returning Values):** A switch can now instantly return a value directly into a variable.

```java
String day = "Tuesday";

// Modern Switch: Clean, safe, and returns a value directly
String schedule = switch (day) {
    case "Monday" -> "Team Meeting";
    case "Tuesday", "Wednesday", "Thursday" -> "Deep Work";
    case "Friday" -> "Deploy Code";
    default -> "Off grid";
}; // Notice the semicolon here, because the whole switch is an expression!

System.out.println(schedule);
```

### The `yield` Keyword (Advanced Modern Switch)
Sometimes, the logic inside a modern switch case is too complex for one line, so you need curly braces `{}`. But if you use `{}` in a modern switch expression, how do you return the value? You use the new `yield` keyword.

```java
int status = 404;

String message = switch (status) {
    case 200 -> "Success";
    case 404 -> {
        System.out.println("Logging error to database...");
        // yield is like 'return', but specifically for escaping a switch block
        yield "Not Found"; 
    }
    default -> "Unknown Status";
};
```

## Summary for Technical Rounds
- **Use `if / else`** when evaluating ranges (e.g., `score > 90`), complex conditions, or multiple variables.
- **Use `switch`** when checking one variable against specific, discrete values (like enums, strings, or integers).
- If you are asked to write a switch statement on a whiteboard, specifically ask the interviewer: *"Would you prefer I use legacy syntax with breaks, or modern Java 14+ switch expressions?"* (This instantly signals that you are an up-to-date, high-level candidate).