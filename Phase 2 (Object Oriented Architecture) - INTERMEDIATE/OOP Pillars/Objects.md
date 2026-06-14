# Java Objects: The Physical Instance

You have built the blueprint (the Class). Now it is time to manufacture the product.

In Phase 1, we talked briefly about how Objects live in the Heap memory and are controlled by reference variables in the Stack. But in Phase 2, we need to look at the exact lifecycle of an Object, how they interact, and the invisible rules Java enforces on them.

## 1. The Cosmic Superclass (`java.lang.Object`)

This is a massive interview concept. In Java, **every single class you write secretly inherits from a master class called `Object`.** Even if you just write `public class Car {}`, Java secretly compiles it as `public class Car extends Object {}`.

Because of this, every object you ever create comes pre-packaged with a few critical methods. If you don't rewrite (override) them, Java uses the default ones, which usually causes headaches.

**The "Big Three" Object Methods:**

| Method | Default Behavior | Why you MUST override it |
| --- | --- | --- |
| **`toString()`** | Returns the memory address (e.g., `Car@7a81197d`). | If you want to print your object (`System.out.println(myCar)`), you must override this to return actual data like `"Car: Red Mustang"`. |
| **`equals(Object o)`** | Acts exactly like `==`. It only checks if the memory addresses match. | If you want to check if two different `Car` objects have the same VIN number, you must override this to compare the internal data. |
| **`hashCode()`** | Generates a random integer based on the memory address. | If you override `equals()`, you **must** override `hashCode()`. (We will see exactly why when we hit `HashMap` in Phase 3). |

## 2. The Remote Control Trap (Reference Assignment)

If you have two primitive variables (`int a = 5; int b = a;`), Java makes a physical copy of the number 5. If you change `b` to 10, `a` is still 5.

**Objects do not work this way.** Remember: the variable is just a remote control.

```java
Employee emp1 = new Employee("Alice", 50000);
Employee emp2 = emp1; // TRAP: This does NOT create a second employee!

emp2.giveRaise(10000); 

// emp1's salary is now 60000! 
```

When you wrote `emp2 = emp1`, you didn't copy the employee. You just took a second remote control and paired it to the exact same TV.

## 3. Anonymous Objects (The One-Time Use)

Sometimes, you need an object to do exactly one piece of work, and you will never need it again. Instead of wasting time assigning it to a variable, you can create an **Anonymous Object**.

It is an object created in the Heap without a remote control in the Stack.

```java
// Standard way (Takes up a variable name, stays in memory longer)
Calculator calc = new Calculator();
int result = calc.add(5, 10);

// Anonymous Object way (Creates it, uses it, and instantly abandons it)
int result2 = new Calculator().add(5, 10); 
```

## 4. The Object Lifecycle & Garbage Collection

In languages like C++, when you build an object in memory, it stays there forever until you manually write code to delete it. If you forget, your computer runs out of RAM and crashes (a Memory Leak).

Java handles this automatically using the **Garbage Collector (GC)**. But you need to know exactly *when* an object becomes eligible for the trash.

**The Rule:** An object is marked for death the exact microsecond that **zero active reference variables are pointing to it.**

```java
Employee emp = new Employee("Bob"); // Object 'Bob' is born
emp = new Employee("Charlie");      // Object 'Charlie' is born. 

// The remote control 'emp' dropped 'Bob' to point to 'Charlie'. 
// 'Bob' now has 0 remote controls pointing to him. 
// 'Bob' is abandoned in the Heap and will be deleted by the GC.
```

### The Interview Trap: `System.gc()`

An interviewer might ask: *"How do you force the Garbage Collector to run immediately?"*
**The Answer:** You can't.
You can write `System.gc();` in your code, but this is merely a *polite request* to the JVM. The JVM decides when it is the optimal time to clean up memory. You cannot force it.
