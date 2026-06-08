# Java 2D Arrays

Most candidates think of a 2D array as a grid or a spreadsheet (Rows and Columns). **This is a dangerous oversimplification in Java.**

In Java, a 2D array is literally an **"Array of Arrays"**.
The main array doesn't hold data; it holds *remote controls* (references) that point to other arrays in memory.

## 1. The Standard Grid

When you know you want a perfect rectangular grid:

```java
// Creates a 3x3 grid (3 rows, 3 columns)
int[][] grid = new int[3][3];

grid[0][0] = 1; // Top-left corner
grid[2][2] = 9; // Bottom-right corner

// Inline creation
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

## 2. The "Jagged Array" (The Ultimate Interview Concept)

Because a 2D array is just an array pointing to other arrays, **the rows do not have to be the same length.** This is called a Jagged (or Ragged) array. Interviewers use this to see if you actually understand Java memory.

```java
// Create a 2D array with 3 rows, but DON'T specify the columns yet
int[][] jagged = new int[3][];

jagged[0] = new int[2]; // Row 0 has 2 columns
jagged[1] = new int[5]; // Row 1 has 5 columns
jagged[2] = new int[1]; // Row 2 has 1 column
```

## 3. Printing a 2D Array

If you use `Arrays.toString()` on a 2D array, it will print memory addresses for the inner arrays. You must use `Arrays.deepToString()` instead.

```java
import java.util.Arrays;

int[][] grid = {{1,2}, {3,4}};
System.out.println(Arrays.deepToString(grid)); // Prints: [[1, 2], [3, 4]]
```

## 4. The Interview Trap: Looping a 2D Array

If you are iterating through a 2D array, you must use nested loops, and you must use the correct lengths for the rows and columns, especially if it's a jagged array.

```java
int[][] matrix = { 
    {1, 2}, 
    {3, 4, 5} 
};

for (int row = 0; row < matrix.length; row++) {
    // TRAP: The inner loop must go up to matrix[row].length, NOT matrix[0].length!
    for (int col = 0; col < matrix[row].length; col++) {
        System.out.print(matrix[row][col] + " ");
    }
    System.out.println(); // New line after each row
}
```
If you mistakenly use `matrix[0].length` for the inner loop, it assumes every row is the exact same length as the first row. In a jagged array, this will crash with an `ArrayIndexOutOfBoundsException`.
