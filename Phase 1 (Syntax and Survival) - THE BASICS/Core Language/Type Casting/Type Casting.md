# Java Type Casting

Type casting is the art of forcing a piece of data to change its shape. You have a square peg, and you need it to fit into a round hole.

In Java, casting is split into two entirely different worlds: **Primitive Casting** (resizing memory boxes) and **Object Casting** (changing how you view an object).

Here is the complete, production-ready breakdown of type casting.

---

## Part 1: Primitive Casting (Changing Memory Sizes)
This deals with the 8 basic data types. Imagine these data types as buckets of different sizes. A `byte` is a tiny teacup, an `int` is a bucket, and a `double` is a massive barrel.

### 1. Widening Casting (Implicit / Automatic)
This happens when you pour a smaller bucket into a larger bucket. It is 100% safe. No water spills. Because it’s safe, Java does it for you automatically—you don't even have to ask.

**The Order:** `byte -> short -> char -> int -> long -> float -> double`

```java
int myInt = 9;
double myDouble = myInt; // Automatic casting: int to double

System.out.println(myInt);      // Outputs 9
System.out.println(myDouble);   // Outputs 9.0
```

### 2. Narrowing Casting (Explicit / Manual)
This happens when you try to pour a massive barrel into a tiny teacup. Water will spill. Data will be lost. Because this is dangerous, Java refuses to do it unless you explicitly take responsibility by writing the target type in parentheses `()`.

**The Order:** `double -> float -> long -> int -> char -> short -> byte`

```java
double myDouble = 9.78d;
int myInt = (int) myDouble; // Manual casting: double to int

System.out.println(myDouble);   // Outputs 9.78
System.out.println(myInt);      // Outputs 9 (The .78 is chopped off, NOT rounded)
```

### 3. Type Promotion in Expressions (The Hidden Cast)
Whenever you do math in Java, the compiler automatically **promotes** smaller primitive types (`byte`, `short`, and `char`) to an `int` before evaluating the expression. 
```java
byte a = 10;
byte b = 20;
// byte c = a + b; // ERROR! a + b evaluates to an int. 
int c = a + b;     // Correct
byte d = (byte) (a + b); // Also correct if you manually cast it back
```
If one of the operands is a `long`, `float`, or `double`, the entire expression is promoted to that type.

---

## Part 2: Object Type Casting (The Interview Meat)
This is where things get serious. Object casting doesn't change the actual object in the Heap memory. It only changes the reference type (the "remote control") you are using to interact with it.

Imagine you have a `Vehicle` parent class, and a `Car` child class that extends `Vehicle`.

### 1. Upcasting (Implicit)
Treating a child object as if it were a parent object. This is always safe because a Car *is a* Vehicle.
```java
Car mySubaru = new Car();
Vehicle myVehicle = mySubaru; // Upcasting. Safe and automatic.
// myVehicle can only access methods defined in the Vehicle class, 
// even though the actual underlying object in memory is still a Car.
```

### 2. Downcasting (Explicit)
Treating a parent reference as a child object. This is dangerous! Not every `Vehicle` is a `Car` (it might be a Boat). If you force it, and you are wrong, your program will crash instantly at runtime with a `ClassCastException`.

To do this safely, you must always check the object's true identity using the `instanceof` keyword before downcasting.

```java
Vehicle mysteriousVehicle = getVehicleFromDatabase();

// 1. Old Way (Pre-Java 16)
if (mysteriousVehicle instanceof Car) {
    Car myCar = (Car) mysteriousVehicle; // Explicit downcast
    myCar.honkHorn();
}

// 2. Modern Way (Java 16+ Pattern Matching) - Highly recommended!
if (mysteriousVehicle instanceof Car myCar) {
    // Java automatically casts it and creates the 'myCar' variable for you!
    myCar.honkHorn(); 
}
```

---

## Part 3: The Common Beginner Mistake (Type Conversion vs Casting)
In technical rounds, candidates often confuse Casting with Parsing/Conversion.

If you have a `String` containing a number (e.g., `"123"`) and you want an `int`, you **cannot** cast it. A `String` (Object) and an `int` (Primitive) are entirely different species. You have to *parse* it.

```java
String ageText = "25";

// int age = (int) ageText; -> THIS WILL NOT COMPILE!

int age = Integer.parseInt(ageText); // Correct. This is Parsing, not Casting.
```

Similarly, to go from primitive back to String, you do not cast, you convert it using methods like `String.valueOf(123)`.

---

That is the complete picture of type casting, from primitive memory manipulation and expression promotions to modern object pattern matching!