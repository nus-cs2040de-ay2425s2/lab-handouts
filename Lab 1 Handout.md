# CS2040DE Data Structures and Algorithms

## Lab 1: Introduction to Java (and Binary Search)

<div align="center">
<img src="./images/nus_logo.jpg" alt="NUS Logo" style="zoom:30%;" />
</div>


<div align="center">
<table style="border-collapse: collapse; margin: 0 auto;">
<tr><td align="center" style="font-size: 24px;"><strong>National University of Singapore</strong></td></tr>
<tr><td align="center" style="font-size: 24px;"><em>Academic Year 2024/25 Semester 2</em></td></tr>
<tr><td align="center" style="font-size: 16px; padding-top: 10px;">Hitesh (B03) and Zikun (B01 & B04)</td></tr>
<tr><td align="center" style="font-size: 14px;"><em>Materials adapted from: Xiaoyang, Hitesh, Yurui</em></td></tr>
</table>
</div>



## Learning Objectives
By the end of this lab, students should be able to:
- [1] Set up a complete Java development environment
- [2] Write and execute basic Java programs
- [3] Understand and implement binary search algorithm (correctly!) by understanding different interval approaches
- [3] (Optional) Deal with integer overflow when implementing binary search algorithm in Java
- [4] (Optional) Find more practice problems to do if they wish

## 1. Set up Prerequisites: Install IntelliJ IDEA and Java (JDK 21)

### **Install IntelliJ IDEA**

- Go to the IntelliJ IDEA official website: [IntelliJ IDEA – the Leading Java and Kotlin IDE (jetbrains.com)](https://www.jetbrains.com/idea/)
- Click **Download**, and either download the **Community Edition (recommended, free)** or the **Ultimate Edition** (free for students, but requires registering for a license).

### **Install Java (JDK 21)**

Set up the correct JDK version as follows:

1. Click `File` > `Project Structure` then navigate to `Project` > `SDK`
2. Click **Download JDK** and download JDK 21
3. Click **Download**
4. Click **OK**

> [!NOTE]
>
> While we recommend IntelliJ IDEA for this course, you have flexibility in choosing your development environment. If you're already comfortable with Java programming, feel free to use any modern IDE or text editor of your preference. We've provided setup tutorials for several popular alternatives:
>
> 1. [How to set up Java in Visual Studio Code (Windows)](https://www.youtube.com/watch?v=BB0gZFpukJU) 
> 2. [How to set up Java in Visual Studio Code (MacOS)](https://www.youtube.com/watch?v=qTaZO_Ou7Zo) 
> 3. [How to set up Java in Eclipse (Linux)](https://www.youtube.com/watch?v=itepXP1Cros) 
>
> The only requirement is using a recent version of JDK. These tutorials will guide you through the complete setup process for your chosen platform :)

## 2. Hello World!

Let's start with some Java basics:

In Java, every application begins with a **class definition**. In our Hello World program, we can name the class **HelloWorld**. **Important**: The name of the class should match the file name in Java:

```java
class HelloWorld {
	// definition of the class
}
```

Every application in Java must contain a **main method**. The Java compiler starts executing the code from the main method. Here is the signature of the main method:

```java
public static void main(String[] args) {
	// definition of the main function
}
```

**Print**:

```java
// A typical line to output the string, Hello World!, to your console
System.out.println("Hello World!");
```

We also have some other options:
- **print()**: Prints a string inside the quotes without moving to a new line afterward.
- **println()**: Prints a string inside the quotes, then moves the cursor to the beginning of the next line.
- Optional: **printf()**: Provides string formatting (similar to printf in C/C++ programming).

**Print Concatenated Strings**:

```java
class HelloWorld  {
    public static void main(String[] args) {
    	
        int number = 2040;
    	
        System.out.println("This is " + "CS" + number + "DE!");
    }
}
// In Java, when you use the + operator with strings, 
// Java automatically converts other data types into strings for you. 
```

**Input:**

Typically, we get input from the user using a `Scanner` object:

```java
// Import Scanner class to read input
import java.util.Scanner;

public class HelloWorld {
    public static void main(String[] args) {
        // Create a Scanner object to read input
        Scanner input = new Scanner(System.in);
        
        // Example 1: Reading an integer
        System.out.print("Enter a number: ");
        int number = input.nextInt();
        System.out.println("You entered the number: " + number);
        
        // Example 2: Reading a decimal number (double)
        System.out.print("Enter a decimal number: ");
        double decimal = input.nextDouble();
        System.out.println("You entered the decimal: " + decimal);
        
        // Clear the input buffer (important after reading numbers)
        // nextInt() and nextDouble() only consumes the number
        //, leaving the \n in the buffer. 
        input.nextLine();
        
        // Example 3: Reading a line of text
        // nextLine() reads an entire line of text (everything until the user presses Enter)
        System.out.print("Enter a sentence: ");
        String line = input.nextLine();
        System.out.println("You entered the sentence: " + line);
        
        // Example 4: Reading a single word
        // next() reads just one word (until it hits any whitespace)
        System.out.print("Enter a single word: ");
        String word = input.next();
        System.out.println("You entered the word: " + word);
        
        // Example 5: Reading the first character from a word
        System.out.print("Enter a word (we'll take its first letter): ");
        String text = input.next();
        // charAt(pos) returns a single character from an existing String at the specified position
        char firstChar = text.charAt(0);
        System.out.println("The first character is: " + firstChar);
        
        // As a good practice, close the Scanner when done to prevent memory leaks from unclosed buffers
        input.close();
        
        System.out.println("\nProgram completed successfully!");
    }
}
```

Now you can try to write your first Java "Hello World!" program. You can also let the program read and print your own words.

## 3. Binary Search: Understanding Search Intervals

### Solution Template

```java
public class Solution {
    public static void main(String[] args) {
        Solution solution = new Solution();
        
        // Test case 1
        System.out.println("Test 1: " + solution.algo(params1));  // Expected: result1
        
        // Test case 2
        System.out.println("Test 2: " + solution.algo(params2));  // Expected: result2
        
        // Test case 3
        System.out.println("Test 3: " + solution.algo(params3));  // Expected: result3
    }

    public ReturnType algo(ParamType params) {
        // Write your solution here
        return null;
    }
}
```

### Introduction to Search Intervals

Binary search fundamentally narrows down a search space efficiently (and correctly!). The concept of intervals is crucial because it defines exactly which portion of the array we're currently examining. Two main interval approaches are commonly used: **closed intervals** `[left, right]` and **half-open intervals** `[left, right)`.

### Closed Interval Approach `[left, right]`

In the closed interval approach, both endpoints are included in our search space. This means both the `left` and `right` boundaries are valid positions to check for our target.

#### Implementation Details:

```java
public class Solution {
    public static void main(String[] args) {
        Solution solution = new Solution();
        
        // Test case 1
        int[] nums1 = {2, 3, 4};
        System.out.println("Test 1: " + solution.search(nums1, 4));  // Expected: 2
        
        // Test case 2
        int[] nums2 = {1, 2, 3, 4, 5};
        System.out.println("Test 2: " + solution.search(nums2, 1));  // Expected: 0
        
        // Test case 3
        int[] nums3 = {1};
        System.out.println("Test 3: " + solution.search(nums3, 2));  // Expected: -1
    }

    public int search(int[] nums, int target) {
        int left = 0;
        
        // right points to the last valid index
        int right = nums.length - 1;
        
        // <= because [left, right] includes both endpoints
        while (left <= right) {
            int middle = left + (right - left) / 2;
            
            if (nums[middle] < target) {
                left = middle + 1;
            } else if (nums[middle] > target) {
                // -1 because right is inclusive
                right = middle - 1;
            } else {
                return middle;
            }
        }
        return -1;
    }
}
```

#### Key Characteristics:

**1. Initial state**: `right = nums.length - 1`

**2. Loop condition**: `left <= right`

**3. Right boundary update**: `right = middle - 1`

The closed interval approach is intuitive because:

- It directly represents the indices we're searching.

- When `left == right`, we're examining a single valid element.

- The search terminates when `left > right`, meaning we've examined all possibilities.

### Half-Open Interval Approach [left, right)

The half-open interval includes the left boundary but excludes the right boundary. This aligns with many programming conventions (e.g., `for (int i = 0; i < n; i++)`).

#### Implementation Details:

```java
public class Solution {
    public static void main(String[] args) {
        Solution solution = new Solution();
        
        // Test case 1
        int[] nums1 = {2, 3, 4};
        System.out.println("Test 1: " + solution.search(nums1, 4));  // Expected: 2
        
        // Test case 2
        int[] nums2 = {1, 2, 3, 4, 5};
        System.out.println("Test 2: " + solution.search(nums2, 1));  // Expected: 0
        
        // Test case 3
        int[] nums3 = {1};
        System.out.println("Test 3: " + solution.search(nums3, 2));  // Expected: -1
    }

    public int search(int[] nums, int target) {
        int left = 0;
        // right points one past the last valid index
        int right = nums.length;    
        
        // < because right is exclusive
        while (left < right) {      
            int middle = left + (right - left) / 2;
            
            if (nums[middle] < target) {
                left = middle + 1;
            } else if (nums[middle] > target) {
                // no -1 because right is exclusive
                right = middle;     
            } else {
                return middle;
            }
        }
        return -1;
    }
}
```

#### Key Characteristics:

**1. Initial state**: `right = nums.length`

**2. Loop condition**: `left < right`

**3. Right boundary update**: `right = middle`

The half-open interval approach is elegant because:

- It matches common programming patterns.
- The interval `[left, right)` has a length of `right - left`.
- The search terminates when `left == right`, indicating an empty interval.

### Comparison of Approaches

Both approaches are mathematically equivalent but differ in their implementation details:

|                    | Closed `[left, right]` | Half-Open `[left, right)` |
| ------------------ | ---------------------- | ------------------------- |
| Initial right      | `nums.length - 1`      | `nums.length`             |
| Loop condition     | `left <= right`        | `left < right`            |
| Right update       | `middle - 1`           | `middle`                  |
| Terminal condition | `left > right`         | `left == right`           |

### Understanding Edge Cases

Let's examine how each approach handles an array with a single element `[5]`:

#### Closed Interval `[left, right]`:

1. Initial state: `left = 0, right = 0`

2. Valid because `left <= right`

3. `middle = 0`

4. We can inspect the single element.

#### Half-Open Interval `[left, right)`:

- Initial state: `left = 0, right = 1`
- Valid because `left < right`
- `middle = 0`
- We can inspect the single element.

Both approaches handle this edge case correctly, but their boundary conditions differ.

### Important Considerations

1. The choice between closed and half-open intervals affects three critical parts of the code:
   - Initial value of `right`
   - Loop condition
   - Right pointer update rule

2. **Consistency** is key—mixing conventions can lead to bugs or infinite loops.
3. The half-open interval often meshes better with other programming constructs, while the closed interval approach can be more intuitive for direct array index manipulation.

> [!NOTE]
>
> (Optional) Integer overflow in binary search is possible and there are also ways to address that.

## (Optional) Understanding the Middle Point Calculation

In binary search, calculating the middle point correctly and safely is crucial. The formula `mid = left + (right - left) / 2` is used instead of the more intuitive `mid = (left + right) / 2` to help prevent integer overflow.

### Mathematical Proof of Equivalence

Let's prove these formulas are equivalent:

1. Start with: `mid = left + (right - left) / 2`
2. Expand the parentheses: `mid = left + right/2 - left/2`
3. Combine like terms: `mid = (2×left)/2 + right/2 - left/2`
4. Simplify: `mid = (left + right)/2`

### Why Overflow Can Occur

Consider a 32-bit integer system where:

- Maximum value (2³¹ - 1) ≈ 2.1 billion
- left = 2³¹ - 1
- right = 2³¹ - 1

Using `(left + right) / 2`:

1. left + right = 2 × (2³¹ - 1)
2. This exceeds 2³¹ - 1
3. Integer overflow occurs before division

Using `left + (right - left) / 2`:

1. right - left = 0
2. 0 / 2 = 0
3. left + 0 = original left value
4. No overflow occurs

### Safe Implementation

```java
// Safe implementation
int middle = left + (right - left) / 2;

// Potentially dangerous implementation
int middle = (left + right) / 2;  // Risk of overflow
```

### Key Points

1. The safe formula `left + (right - left) / 2` prevents overflow by:
   - First calculating the distance between indices
   - Then dividing this smaller number
   - Finally adding to the left boundary

2. This approach works because:
   - right - left is always smaller than the sum left + right
   - Division by 2 further reduces the magnitude
   - Adding to left cannot overflow because the result is guaranteed to be between left and right

3. This is especially important when:
   - Working with large arrays
   - Implementing binary search on value ranges rather than indices
   - Writing robust, production-quality code

## 4. (Optional) Additional Questions From the Past Semester

### Basic coding problems

Q1: Hello World! - First Java Program. [Hello World! – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/hello)

Q2: Hipp Hipp - Simple for loop. [Hipp Hipp – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/hipphipp)

Q3: Mia - Conditional Statements, e.g., if-else. [Mia – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/mia)

Q4: Message - [Message – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/meddelande)

Q5: Autori - ASCII [Autori – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/autori)

Q6: Thanos - [Thanos – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/thanos)

Q7: Gangur - [Gangur – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/gangur)

### Course-related problems

Q8: Fyrirtækjanafn - Could use searching algorithms. [Fyrirtækjanafn – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/fyrirtaekjanafn)

Q9: Sort of Sorting - Could use sorting algorithms. [Sort of Sorting – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/sortofsorting)

Q10: A Rank Problem [A Rank Problem – Kattis, National University of Singapore](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/rankproblem)
