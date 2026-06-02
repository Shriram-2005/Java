# Java Variables

Think of your computer's memory as a massive warehouse full of empty boxes. A variable is simply putting a label on one of those boxes and putting something inside it. 

In Java, because it is a **Statically Typed** language, you must declare exactly what kind of box it is before you use it. The computer needs to know exactly how much space to reserve.

```java
int myAge = 25; 
```
* `int`: The Data Type (the size of the box).
* `myAge`: The Variable Name (the label on the box).
* `25`: The Value (what goes inside the box).

## 1. Naming Conventions (The Rules of the Label)
When naming your variables in Java, you must follow specific rules and conventions:
- **camelCase:** Variables should start with a lowercase letter, and every subsequent word should be capitalized (e.g., `employeeSalary`, `isUserLoggedIn`).
- **Allowed Characters:** Variable names can only contain letters, numbers, underscores (`_`), and dollar signs (`$`). They **cannot** start with a number.
- **Meaningful Names:** Avoid names like `x` or `temp` (unless in a short loop). Use descriptive names like `customerName`.

## 2. Variable Scope (Where does it live?)
A variable only exists within the block of code (defined by `{ }`) where it was created. This is known as its scope. There are three levels of scope in Java:

1. **Local Variables:** Created inside a method. They live on the Stack and are destroyed the second the method finishes. You must initialize a local variable before you can use it.
2. **Instance Variables:** Created inside a Class, but outside any method. Every time you create a new Object (e.g., `Employee e1 = new Employee()`), that specific object gets its own copy of these variables in the Heap memory. They have default values (e.g., `0` for int, `false` for boolean, `null` for objects).
3. **Static Variables (Class Variables):** Created with the `static` keyword. This means the variable belongs to the Class itself, not any individual object. If you have a `static int companyTotalEmployees`, all Employee objects share that exact same variable in memory.

## 3. The Ultimate Trick Question: Pass-by-Value
If an interviewer asks you: *"Is Java Pass-by-Value or Pass-by-Reference?"*

The only correct answer is: **Java is strictly Pass-by-Value.** 

However, it behaves differently for Primitives and Objects, which often confuses developers:
- **For Primitives:** Java passes a copy of the *actual value*. If you pass `x = 5` into a method and the method changes it to `10`, the original `x` outside the method is still `5`.
- **For Objects:** Java passes a copy of the *reference* (the address or the "remote control"). If you pass an array into a method, and the method changes the first item in the array, the original array *will* be changed. You didn't pass the actual array; you passed a copy of the memory address pointing to the array in the Heap. So both the caller and the method are using "remote controls" that point to the exact same TV.