# Abstract Classes: The Half-Blueprint

You already know that an `interface` is a strict, empty contract. It tells a class *what* to do, but provides zero help on *how* to do it.

But what if you want a middle ground? What if you want to write a parent class that has some locked-in, shared logic, but still forces the children to figure out the rest on their own?

You use an **Abstract Class**. It is a "half-blueprint." It is too incomplete to be built into a physical object, but it is deeply structured enough to pass down physical variables and methods to its children.

Here is the complete, production-ready package on Abstract Classes, and the final piece of Phase 2.

## 1. The Anatomy of an Abstract Class

You create an abstract class using the `abstract` keyword. Once you do this, you activate two massive rules:

1. **No Instantiation:** Nobody is legally allowed to write `new ParentClass();`. It is permanently locked from becoming a direct object.
2. **The Mix-and-Match Ability:** It can contain a mix of **Concrete Methods** (methods with normal code bodies) and **Abstract Methods** (methods with no bodies, exactly like an interface).

```java
// 1. THE ABSTRACT PARENT
public abstract class Employee {
    // It can hold actual state/data! (Interfaces cannot do this)
    protected String name;
    protected int id;

    public Employee(String name, int id) {
        this.name = name;
        this.id = id;
    }

    // CONCRETE METHOD: Shared logic passed down to all children
    public void swipeBadge() {
        System.out.println(name + " swiped into the building.");
    }

    // ABSTRACT METHOD: A forced contract. Every child MUST write this themselves.
    public abstract void calculatePay(); 
}
```

## 2. The Concrete Child

If a child class `extends` an abstract parent, it inherits the data (`name`, `id`) and the concrete methods (`swipeBadge()`).

However, the compiler will instantly throw an error until the child writes the logic for the `abstract` method.

```java
// 2. THE CHILD
public class SalariedEmployee extends Employee {
    private double yearlySalary;

    public SalariedEmployee(String name, int id, double yearlySalary) {
        super(name, id); // Passing data up to the Abstract Parent's constructor
        this.yearlySalary = yearlySalary;
    }

    // We MUST override the abstract method, or this class refuses to compile
    @Override
    public void calculatePay() {
        System.out.println("Paying out: $" + (yearlySalary / 24));
    }
}
```

## 3. The Ultimate Interview Trap: Constructors

If an interviewer is testing your deep understanding of Java memory, they will ask you this exact question:
*"If you cannot instantiate an abstract class using the `new` keyword, why are you allowed to write Constructors inside them?"*

> [!TIP]
> **The Answer:** Because the child class needs them!
> When you write `new SalariedEmployee()`, the child object is being built in the Heap. But remember our Inheritance lesson? The parent's memory layer must be built *first*. The child uses `super()` to call the Abstract Class's constructor to set up the inherited variables (like `name` and `id`).
> 
> The Abstract Class's constructor *does* run, it just never runs on its own—it only runs as part of building a child object.

## 4. The Golden Rule: When to use WHICH?

You now know how to use Interfaces and Abstract Classes. But the true test of a Software Engineer is knowing *when* to choose one over the other.

**Use an Abstract Class when:**

1. **You want to share code and state.** If multiple classes need the exact same instance variables (like `name`, `health`, `speed`) and the exact same helper methods, put them in an abstract class so you don't repeat yourself.
2. **There is a strict "Identity" relationship.** A `Dog` fundamentally IS-AN `Animal`.

**Use an Interface when:**

1. **You only care about a specific capability.** You don't care about what the object *is*, you only care about what it *can do*.
2. **You need multiple inheritance.** A `SmartPhone` might need to be `Chargeable`, `Connectable`, and `CameraEnabled`. You can only extend ONE abstract class, but you can implement INFINITE interfaces.

> [!NOTE]
> **Pro-Tip for Modern Architecture:** In modern Java enterprise backends (like Spring Boot), you will use **Interfaces** 90% of the time, and Abstract Classes 10% of the time. Interfaces are lighter, more flexible, and decouple your code beautifully.

---

### 🏆 Phase 2 is Officially Complete!

You have conquered the grammar of Java (Phase 1) and the Object-Oriented Architecture (Phase 2). You can now design robust, secure, and scalable blueprints.

But architecture is useless if you don't know how to organize the people inside the building. In the real world, you aren't dealing with one `Employee` object; you are dealing with a database of 10,000 `Employee` objects that need to be sorted, searched, and updated in milliseconds. Arrays are too rigid to handle this.

Are you ready to enter **Phase 3: The Java Collections Framework**, where we will finally leave Arrays behind and master `ArrayList`, `HashSet`, and `HashMap`?
