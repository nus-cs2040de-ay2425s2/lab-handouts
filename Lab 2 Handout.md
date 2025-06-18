# CS2040DE Data Structures and Algorithms

## Lab 2: Introduction to OOP (and Recap of Lab 1 Content)

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

- [1] Understand and apply very basic concepts of Object-Oriented Programming
- [2] Demonstrate understanding of Java basics, including fundamental data types, access modifiers, the static keyword as well as differences between variables & functions and attributes & methods
- [3] Understand Week 3 content completely
- [4] (Optional) Find more practice problems to do if they wish



## 1. A Gentle Introduction to Object Oriented Programming

Please refer to slides [here](https://docs.google.com/presentation/d/1COooqRhPdcMTnTNcrpOh0gANVwkuvzKQzgkujTYBXGs/edit?usp=sharing).



**Now, do we understand what `Solution solution = new Solution();` means from Lab 1? :)**

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

This line consists of three parts:

1. `Solution` (first one) - declares the type of the variable
2. `solution` (lowercase) - is the name of the attribute we're creating
3. `new Solution()` - creates a new instance (object) of the Solution class

It's like saying:

- We need a `Solution` object to use our non-static `search` method
- Because `search` is non-static (no `static` keyword), it belongs to instances of the class
- We can't just call `search` directly like we can with `main` (which is static)



### Follow up questions #1: when we define multiple classes, can we define multiple classes in the same file?

Answer: In **Java**, it’s technically possible to define **multiple classes** in a single `.java` file. However, **only one** of those classes can be declared `public` (if any). 

Example: 

```java
// File: MyClasses.java
public class MyClasses {
    public static void main(String[] args) {
        Helper helper = new Helper();
        helper.sayHello();
    }
}

class Helper {
    public void sayHello() {
        System.out.println("Hello from Helper!");
    }
}
```

Here:

- `MyClasses` is `public`.
- `Helper` has **default (package-private)** visibility.
- Both are in the same file `MyClasses.java`.

#### Best Practice

The usual (and strongly recommended) best practice is indeed to **create one top-level public class per `.java` file**, with the file name matching the class name. This approach makes your code:

- **Easier to navigate**: You can always find a class by looking for its name in the file system.
- **Cleaner and more maintainable**: Classes are clearly separated, making it simpler to manage changes as your code grows.

While Java allows multiple classes (only one can be `public`) in a single file, it’s rarely used in larger or production code because it can become confusing.



## 2. Java Basics, Continued

In this section, we will learn about 4 topics:

1. Variables & Functions versus Attributes & Methods

2. Java Basic Data Types

3. Access Modifiers

4. The `static` Keyword



### 1. Variables & Functions versus Attributes & Methods (Are We Inside a Class?)

#### Python: Variables and Functions vs. Attributes and Methods

- Procedural Python

  - If you write code in a single `.py` file without using classes, you typically say you have **variables** and **functions** at the **module** (file) level.

  - Example

    ```python
    # This is Python code
    
    x = 10  # A module-level variable
    
    def greet(name):
        print(f"Hello, {name}!")
    ```

- **Object-Oriented Python**

  - Once you define a **class**, you typically say **“attributes”** (variables inside a class) and **“methods”** (functions inside a class).

  - Example:

    ```python
    # This is Python code
    
    class Student:
        def __init__(self, name):
            self.name = name  # An attribute
    
        def say_hello(self):
            # A method
            print(f"Hello, I'm {self.name}")
    ```

#### Java: Everything Must Be Inside a Class

In Java, you **cannot** define variables or functions **outside** a class. Therefore:

- **Variables** are called **“attributes”**.
- **Functions** are called **“methods.”** 

**Example**:

```java
// Student.java
public class Student {

    // An attribute
    private String name;

    // Constructor (special method) - same name as the class, no return type
    public Student(String name) {
        this.name = name; 
    }

    // A method
    public void sayHello() {
        System.out.println("Hello, I'm " + name);
    }
}

// Main.java
public class Main {
    public static void main(String[] args) {
        // In Java, we can't just write code on its own; it must be inside a method in a class.
        Student student = new Student("Alice");
        student.sayHello();
    }
}
```

Here:

- `name` is an **attribute** of `Student`.
- `Student(...)` is a **constructor**, a special **method**.
- `sayHello()` is a **method**.
- `main(String[] args)` is a static **method** required as an entry point.



### 2. Java Basic Data Types

In Python, you could just do `x = 42` or `x = "Hello"` without specifying the type. In Java, you must explicitly declare a variable’s type when you create it. If you try to assign a type that doesn't match the variable's declared type, you will get a compile-time error.

#### Primitive Data Types

Java has **eight** primitive data types:

1. **`byte`** (8-bit integer, -128 to 127)
2. **`short`** (16-bit integer, -32,768 to 32,767)
3. **`int`** (32-bit integer, ~ -2 billion to 2 billion)
4. **`long`** (64-bit integer, very large range; often suffixed with an `L`)
5. **`float`** (32-bit floating point, often suffixed with an `f`)
6. **`double`** (64-bit floating point, default for decimal numbers)
7. **`char`** (16-bit Unicode character)
8. **`boolean`** (can only be `true` or `false`)

```java
// A bit about naming convention here, in Java, we use PascalCase (a.k.a. UpperCamelCase) for class names
// and camelCase for method names and attribute names
public class DataTypesExample {
    public static void main(String[] args) {
        // Integers
        byte smallNumber = 100;
        int myNumber = 42;
        long bigNumber = 123456789L; // Using L clarifies this is a long (64-bit)

        // Floating point
        float piApprox = 3.14f; // 3.14f is a float literal
        double morePrecisePi = 3.1415926535;

        // Boolean
        boolean isJavaFun = true;

        // Character
        char letterA = 'A';

        // Printing
        System.out.println("Byte value: " + smallNumber);
        System.out.println("Int value: " + myNumber);
        System.out.println("Long value: " + bigNumber);
        System.out.println("Float value: " + piApprox);
        System.out.println("Double value: " + morePrecisePi);
        System.out.println("Boolean value: " + isJavaFun);
        System.out.println("Char value: " + letterA);
    }
}
```

FAQ #1: 

To store the number 42, should I use `byte`, `short`, or `int` in Java?

Answer: 

The short answer is just use `int`. A longer answer is below.

When deciding between these integer types, we should consider their memory ranges and common programming practices. A `byte` can store numbers from -128 to 127, using 8 bits of memory. A `short` expands this to -32,768 to 32,767, using 16 bits. An `int` gives us the largest range of about -2.1 billion to 2.1 billion, using 32 bits.

While 42 fits comfortably within all three ranges, the standard practice in Java is to use `int`. This is because Java treats number literals as `int` by default, and Java is optimized for working with `int` values. Even though `byte` and `short` might seem more memory-efficient, the JVM often converts them to `int` internally for calculations anyway.

You would typically only choose `byte` or `short` when working with very large arrays where memory conservation is crucial, or when interfacing with APIs that specifically require these types. For a single value like 42, using `int` follows Java conventions and won't impact your program's performance.



FAQ #2:

Why is `String` not a primitive data type?

Answer:

**Primitives in Java** (e.g., `int`, `boolean`, `char`, etc.) represent the most basic, low-level data values. **`String`** in Java is a **class**.



### 3. Access Modifiers

Access modifiers in Java determine the **visibility** (or accessibility) of classes, methods, and attributes. The main access modifiers are:

1. **`public`**: Accessible from **anywhere** (any class in any package).
2. **`private`**: Accessible **only within** the same class.
3. (Optional) **`protected`**: Accessible within the **same package** or **subclasses** in different packages.
4. (Optional) *(default/package-private)*: When no keyword is provided, accessible **only within the same package**.

```java
public class AccessModifierExample {
    
    public int publicVariable = 10;     // Can be accessed from anywhere
    private int privateVariable = 20;   // Can be accessed only in this class
    protected int protectedVariable = 30; // Accessible in the same package or subclasses

    // Default (package-private)
    int defaultVariable = 40; // If no modifier is specified, accessible within this package only

    public static void main(String[] args) {
        AccessModifierExample example = new AccessModifierExample();
        
        // Accessing variables within the same class
        System.out.println("Public: " + example.publicVariable);
        System.out.println("Private: " + example.privateVariable);
        System.out.println("Protected: " + example.protectedVariable);
        System.out.println("Default: " + example.defaultVariable);
    }
}
```



### 4. The `static` Keyword

When you see `public static void main(String[] args)`, the keyword **`static`** means that the method **belongs to the class itself** rather than an instance of the class.

- **`static`** methods and attributes can be called without creating an instance of the class.
- **Non-static** methods and attributes (often called instance methods and instance attributes) require an instance (an object) before they can be used.

```java
public class StudentGradebook {

    // Static attribute (belongs to the class)
    public String schoolName = "XYZ High School";

    // Non-static (instance) attributes
    private String studentName;
    private int studentGrade;

    // Constructor (special method with the same name as the class)
    // This method is called when creating a new instance (object) of the class.
    public StudentGradebook(String studentName, int studentGrade) {
        this.studentName = studentName;
        this.studentGrade = studentGrade;
    }

    // Non-static method to display details
    public void displayStudentInfo() {
        System.out.println("School: " + schoolName);
        System.out.println("Student: " + studentName);
        System.out.println("Grade: " + studentGrade);
    }

    // Static method - belongs to the class, not any single student object
    public static void printSchoolName() {
        System.out.println("Welcome to " + schoolName);
    }

    // The main method - also static (entry point for the JVM)
    public static void main(String[] args) {
        // We can call this static method directly:
        printSchoolName();

        // We need to create an object (instance) to use non-static parts
        StudentGradebook student1 = new StudentGradebook("Alice", 90);
        StudentGradebook student2 = new StudentGradebook("Bob", 85);

        // Display each student's info
        student1.displayStudentInfo();
        System.out.println("---------------------");
        student2.displayStudentInfo();
    }
}
```

For attributes:

1. Some things belong to the school as a whole:
   - The school name "XYZ High School" is the same for everyone
   - That's why `schoolName` is static - it belongs to the school (class) itself
2. Other things belong to individual students:
   - Each student has their own name and grade
   - That's why `studentName` and `studentGrade` are non-static - they belong to each student object

For methods:

1. Static methods (`printSchoolName`):
   - Can be called without creating any student objects
   - Can only access static attributes
   - Like a school announcement system that works even when no students are present
2. Non-static methods (`displayStudentInfo`):
   - Need a specific student object to work
   - Can access both static and non-static attributes
   - Like asking a specific student to tell you their information

The main method:

- Must be static because when Java starts running your program, no objects exist yet
- It's like the school opening ceremony - it needs to happen before any students arrive



## 3. Recap of Week 3 Content

This is for students from Thursday and Friday lab sessions. We will go back to Lab 1 handout to address any confusions, if any.



## 4. (Optional) Additional Questions From the Past Semester

Q1: [Sort of Sorting](https://nus.kattis.com/courses/CS2040DE/CS2040DE_S1AY2425/assignments/veckw8/problems/sortofsorting)

Q2: [Sideways Sorting](https://open.kattis.com/problems/sidewayssorting)

Q3: [Rice judge](https://open.kattis.com/problems/risdomare) 

Q4: [Ferskasta Jarmið](https://open.kattis.com/problems/ferskastajarmid)

Q5: [Height Ordering](https://open.kattis.com/problems/height)

Q6: [Bílskúrar](https://open.kattis.com/problems/bilskurar)

Q7: [Bread Sorting](https://open.kattis.com/problems/bread)

Q8: [Music Your Way](https://open.kattis.com/problems/musicyourway)

Q9: [Illuminated City](https://open.kattis.com/problems/stadiljus)

Q10: [Judging Troubles](https://open.kattis.com/problems/judging)
