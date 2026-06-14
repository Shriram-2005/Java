# Java Common String Methods

Day-to-day, 90% of your job as a backend engineer will involve taking raw text from a database, an API, or a user, and slicing it, checking it, or reformatting it.

Because `String` is an Object, it comes packed with dozens of built-in methods. Here is the complete, production-ready package of the most common String methods you need for technical rounds, grouped by exactly what they do.

## 1. Inspection (Looking at the data)

These methods don't change the String; they just tell you about it.

| Method | What it does | Example (`String s = "Java";`) |
| :--- | :--- | :--- |
| **`length()`** | Returns the number of characters. *(Remember the parentheses!)* | `s.length(); // Returns 4` |
| **`charAt(int index)`** | Returns the specific character at the given index (0-based). | `s.charAt(2); // Returns 'v'` |
| **`isEmpty()`** | Returns `true` if the length is 0 (`""`). | `s.isEmpty(); // Returns false` |
| **`isBlank()`** *(Java 11+)* | Returns `true` if it's empty OR only contains white spaces (`"   "`). | `"   ".isBlank(); // Returns true` |

## 2. Comparison (Checking against others)

We already discussed `.equals()`, but there are a few other critical comparison tools.

| Method | What it does | Example (`String s = "Java";`) |
| :--- | :--- | :--- |
| **`equals(String other)`** | Strictly compares the exact text (case-sensitive). | `s.equals("java"); // false` |
| **`equalsIgnoreCase(...)`** | Compares the text, ignoring uppercase/lowercase. | `s.equalsIgnoreCase("java"); // true` |
| **`compareTo(String other)`** | Compares alphabetically (lexicographically). Used for sorting. Returns `0` if equal, negative if `s` comes first, positive if `other` comes first. | `"Apple".compareTo("Banana"); // Returns negative number` |

## 3. Searching (Finding things inside)

These are heavily used in Data Structures and Algorithms (DSA) when parsing text.

| Method | What it does | Example (`String s = "Developer";`) |
| :--- | :--- | :--- |
| **`contains(CharSequence)`** | Returns `true` if the specific text exists inside. | `s.contains("velop"); // true` |
| **`indexOf(String str)`** | Returns the exact index where a word/letter *first* starts. Returns `-1` if not found. | `s.indexOf("e"); // Returns 1` |
| **`lastIndexOf(...)`** | Returns the index where a word/letter *last* appears. | `s.lastIndexOf("e"); // Returns 7` |
| **`startsWith(String prefix)`** | Returns `true` if the string begins with this text. | `s.startsWith("Dev"); // true` |
| **`endsWith(String suffix)`** | Returns `true` if the string ends with this text. | `s.endsWith("per"); // true` |

## 4. Manipulation (Creating NEW Strings)

> [!WARNING]
> **CRITICAL REMINDER:** Strings are immutable! None of these methods change the original string. They *all* return a brand-new string. If you don't save the result into a variable, the work is instantly lost.

| Method | What it does | Example (`String s = "  Hello World  ";`) |
| :--- | :--- | :--- |
| **`trim()`** | Removes leading and trailing spaces (not spaces in the middle). | `s.trim(); // "Hello World"` |
| **`toLowerCase()`** | Converts all characters to lowercase. | `s.toLowerCase(); // "  hello world  "` |
| **`toUpperCase()`** | Converts all characters to uppercase. | `s.toUpperCase(); // "  HELLO WORLD  "` |
| **`replace(old, new)`** | Swaps all occurrences of a character or word with a new one. | `s.replace("World", "Java"); // "  Hello Java  "` |

## 5. The Heavy Hitters (Slicing and Splitting)

These are the two most complex methods that interviewers will expect you to use flawlessly on a whiteboard.

### 1. `substring(int beginIndex, int endIndex)`
This extracts a specific chunk of text.

> [!CAUTION]
> **The Trap:** The `beginIndex` is **inclusive**, but the `endIndex` is **exclusive**.

```java
String text = "Hamburger";
// Start at index 4 ('u'), stop BEFORE index 8 ('e')
String sub = text.substring(4, 8); // Returns "urge"

// If you only provide the start index, it goes to the very end
String sub2 = text.substring(3); // Returns "burger"
```

### 2. `split(String regex)`
This takes a single String and violently chops it up into a `String[]` array based on a delimiter (like a comma or a space).

```java
String csvLine = "Alice,Bob,Charlie";
String[] names = csvLine.split(","); 
// names array is now: ["Alice", "Bob", "Charlie"]
```

## The Ultimate String Trap: `""` vs `null`

In a technical interview, you might be asked to write a method that checks if a String is valid before processing it. Candidates fail this constantly.

* **`""` (Empty String):** The memory box exists in the Heap, it just doesn't have any letters inside it. Calling `"".length()` returns `0`.
* **`null`:** There is no memory box at all. You have a remote control, but it points to nothing.

> [!CAUTION]
> **The Trap:** If you call a method on a `null` string, your program will instantly crash with a `NullPointerException`.

```java
// How to safely check a string in production code:
if (text != null && !text.isEmpty()) {
    // It is safe to use!
}
```

*(Notice we check `!= null` FIRST. Because of **Short-Circuit Evaluation** from our Operators lesson, if it is null, Java never checks `.isEmpty()`, saving us from a crash!)*
