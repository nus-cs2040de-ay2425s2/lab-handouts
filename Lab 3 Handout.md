# CS2040DE Data Structures and Algorithms

## Lab 3: Linked List, Stack, Queue, Deque

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

- [1] Understand more about linked list
- [2] Understand more about stack
- [3] Understand more about queue and deque
- [4] (Optional) have a better understanding of access modifiers and get other questions from previous labs answered
- [5] (Optional) Find more practice problems to do if they wish



## 1. Linked List

A linked list is a linear data structure where elements are connected through pointers. Each node consists of two parts:

1. A data field that stores the actual value

2. A pointer field that stores the reference to the next node

The last node's pointer field points to null, indicating the end of the list. The entry point of a linked list is called the head node, or simply "head".

This structure allows elements to be stored non-contiguously in memory, unlike arrays where elements must be stored in consecutive memory locations.

![链表1](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194529815.png)

**Types of Linked Lists:**

1. Singly Linked List (SLL):
   - Each node has only one pointer field
   - Can only point to the next node
   - Can only traverse forward
2. (Optional) Doubly Linked List (DLL):
   - Each node has two pointer fields
   - One points to the next node, one points to the previous node
   - Can traverse both forward and backward
   - Enables bidirectional traversal



**Doubly Linked List:**

![链表2](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806194559317.png)

Operations in Singly Linked List:

**Delete a node:**

![链表-删除节点](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806195114541-20230310121459257.png)

To remove node D, we simply go to node C, then need to make C's next pointer point to node E. 

You might ask: "Isn't node D still in memory? It's just not in the linked list anymore." Yes, that's correct. In C++, you should manually free this memory for node D. Other languages like Java and Python have their own garbage collection mechanisms, so manual memory deallocation isn't necessary.

**Add a node:**

![链表-添加节点](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806195134331-20230310121503147.png)

To insert node F between C and D:

1. Find node C (requires traversal from head) - O(N)
2. Once we're at the correct position, the actual insertion is O(1):
   - Set F's next pointer to D (F.next = C.next)
   - Set C's next pointer to F (C.next = F)

So while the actual pointer manipulation is O(1), the total operation is O(N) because we need to traverse to find node C first.



Here's a comprehensive table for Singly Linked List (SLL) and Doubly Linked List (DLL) operations:

| Operation | Location                                 | SLL (with tail)         | SLL (no tail)          | (Optional) DLL (with tail) |
| --------- | ---------------------------------------- | ----------------------- | ---------------------- | -------------------------- |
| Add       | Start                                    | O(1) - update head      | O(1) - update head     | O(1) - update head         |
| Add       | End                                      | O(1) - use tail         | O(n) - traverse to end | O(1) - use tail            |
| Add       | At index i (not start / end)             | O(n) - traverse         | O(n) - traverse        | O(n) - traverse            |
| Delete    | Start                                    | O(1) - update head      | O(1) - update head     | O(1) - update head         |
| Delete    | End                                      | O(n) - find second last | O(n) - traverse        | O(1) - use tail prev       |
| Delete    | At index i (not start / end)             | O(n) - traverse         | O(n) - traverse        | O(n) - traverse            |
| Search    | Start                                    | O(1) - use head         | O(1) - use head        | O(1) - use head            |
| Search    | End                                      | O(1) - use tail         | O(n) - traverse        | O(1) - use tail            |
| Search    | For value / at index i (not start / end) | O(n) - traverse         | O(n) - traverse        | O(n) - traverse            |



**Some comparison between array and linked list**

| Operation                         | Array (no resize) | Array (with resize) | Linked List            |
| --------------------------------- | ----------------- | ------------------- | ---------------------- |
| Add at end                        | O(1)              | O(n)                | O(1) with tail pointer |
| Add at start                      | O(n)              | O(n)                | O(1)                   |
| Add at index i (not start or end) | O(n)              | O(n)                | O(n)                   |
| Access index i (not start or end) | O(1)              | O(1)                | O(n)                   |
| Access at start / end             | O(1)              | O(1)                | O(1)                   |

| Characteristic     | Arrays                                                       | Linked Lists                                                 |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Memory             | - Fixed length when defined<br>- Contiguous allocation<br>- Requires new array creation for size changes | - Dynamic length<br>- Non-contiguous allocation<br>- Can grow/shrink without reallocation |
| Access Speed       | O(1) random access                                           | O(N) random access                                           |
| Primary Operations | Good for frequent access/search                              | Good for frequent insertions/deletions                       |
| Best Use Cases     | - Need fast random access<br>- Frequent searching            | - Frequent insertions/deletions<br>- Rare random access needs |



#### Using Java's Built-In LinkedList:

Java provides the `LinkedList` class in the `java.util` package. Note that Java's LinkedList is implemented as a doubly-linked list with references to both head and tail.

```java
/*
class SinglyNode {
    int data;
    SinglyNode next;  // only one pointer
}

class DoublyNode {
    int data;
    DoublyNode next;     // pointer to next node
    DoublyNode previous; // pointer to previous node
}

Operation       Singly          Doubly
-----------------------------------------
addFirst()      O(1)            O(1)
addLast()       O(1)*           O(1)
removeFirst()   O(1)            O(1)
removeLast()    O(N)            O(1)
* O(1) for addLast() in singly list only if we maintain a tail pointer (common practice even in a singly linked list - it allows addLast() to be O(1) while only using a small amount of extra memory for the tail pointer.)
*/

import java.util.LinkedList;

public class LinkedListExample {
    public static void main(String[] args) {
        LinkedList<Integer> linkedList = new LinkedList<>();
        // int, Integer
        // char, Character
        // String
        
        // Add elements to the list
        linkedList.add(10);            // [10]
        linkedList.add(20);            // [10, 20]
        linkedList.addFirst(5);        // [5, 10, 20]
        linkedList.addLast(30);        // [5, 10, 20, 30]
        
        // Access elements
        System.out.println("First element: " + linkedList.getFirst());    // 5
        System.out.println("Last element: " + linkedList.getLast());      // 30
        
        // Remove elements
        System.out.println("Removed first: " + linkedList.removeFirst()); // removes 5
        System.out.println("Removed last: " + linkedList.removeLast());   // removes 30
        
        // Print remaining list
        System.out.println("Remaining elements: " + linkedList);          // [10, 20]
    }
}
```



#### (Optional) Manual Implementation of Singly Linked List:

The basic operations include:

- `add`: Adds a node at the end of the list.
- `remove`: Removes a node from the list.
- `get`: Gets the value at a particular position.
- `size`: Returns the size of the list.

```java
class LinkedList {
    private Node head;
    private int size;

    // Node class
    private class Node {
        int value;
        Node next;

        Node(int value) {
            this.value = value;
            this.next = null;
        }
    }

    // Constructor
    public LinkedList() {
        head = null;
        size = 0;
    }

    // Add a node at the end of the list
    public void add(int value) {
        Node newNode = new Node(value);
        if (head == null) {
            head = newNode;
        } else {
            Node current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        size++;
    }

    // Remove the node at the front of the list
    public int remove() {
        if (head == null) {
            System.out.println("List is empty");
            return -1;
        }
        int removedValue = head.value;
        head = head.next;
        size--;
        return removedValue;
    }

    // Get the size of the list
    public int size() {
        return size;
    }

    // Display the elements of the list
    public void display() {
        Node current = head;
        while (current != null) {
            System.out.print(current.value + " ");
            current = current.next;
        }
        System.out.println();
    }
}
```

Question: Is Python list a linked list?

Answer: No. Python's list is actually implemented as a dynamic array (also called resizable array), which means:

1. Memory Organization:
   - Python list: Elements are stored in contiguous memory blocks
   - Linked List: Elements can be scattered in memory, connected by pointers
2. Access Pattern:
   - Python list: Direct access by index - O(1)
   - Linked List: Must traverse from head - O(n)
3. Growing Behavior:
   - Python list: When full, creates new larger array and copies elements (resizes)
   - Linked List: Just adds new node and updates pointers (no resizing needed)

## 2. Stack

A stack is a LIFO (Last In First Out) data structure where elements are added and removed from the same end. 

#### Using Java's Built-In Stack:

Java provides the `Stack` class in the `java.util` package, which extends `Vector` and implements stack operations.

```java
import java.util.Stack;

public class StackExample {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();

        // Push elements onto the stack
        stack.push(10);
        stack.push(20);

        // Peek at the top element
        System.out.println("Top element is: " + stack.peek());

        // Pop the top element
        System.out.println("Popped element: " + stack.pop());

        // Check if the stack is empty
        System.out.println("Is stack empty? " + stack.isEmpty());
    }
}
```

The operations supported are:

- `push`: Adds an element to the top of the stack.
- `pop`: Removes the element at the top of the stack.
- `peek`: Returns the top element without removing it.
- `isEmpty`: Checks if the stack is empty.

#### (Optional) Manual Implementation of Stack:

```java
class Stack {
    private int[] elements;
    private int size;
    private int capacity;

    // Constructor to initialize the stack
    public Stack(int capacity) {
        this.capacity = capacity;
        elements = new int[capacity];
        size = 0;
    }

    // Push an element to the top of the stack
    public void push(int value) {
        if (size == capacity) {
            System.out.println("Stack overflow");
            return;
        }
        elements[size++] = value;
    }

    // Pop an element from the top of the stack
    public int pop() {
        if (size == 0) {
            System.out.println("Stack underflow");
            return -1;
        }
        return elements[--size];
    }

    // Peek at the top element without removing it
    public int peek() {
        if (size == 0) {
            System.out.println("Stack is empty");
            return -1;
        }
        return elements[size - 1];
    }

    // Check if the stack is empty
    public boolean isEmpty() {
        return size == 0;
    }
}

```

## 3. Queue

A queue is a FIFO (First In First Out) data structure where elements are added at the back and removed from the front. 

#### Using Java's Built-In Queue:

Java provides several implementations of the `Queue` interface, such as `LinkedList` and `ArrayDeque`.

```java
import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {
    public static void main(String[] args) {
        Queue<Integer> queue = new LinkedList<>();

        // Enqueue elements
        queue.add(10);
        queue.add(20);

        // Peek at the front element
        System.out.println("Front element is: " + queue.peek());

        // Dequeue the front element
        System.out.println("Dequeued element: " + queue.poll());

        // Check if the queue is empty
        System.out.println("Is queue empty? " + queue.isEmpty());
    }
}
```

**Deque:**

A Deque (Double-ended queue) is very versatile because it can function as either a stack or a queue depending on which methods you use:

As a Stack (LIFO - Last In First Out):

- push() - adds to front
- pop() - removes from front
- peek() - looks at front without removing

As a Queue (FIFO - First In First Out):

- offer() or addLast() - adds to back
- poll() or removeFirst() - removes from front
- peek() or getFirst() - looks at front without removing

```java
Deque<Type> nameOfDeque = new LinkedList<>();
```



**Optional:**

The reason we used `LinkedList` in the `Queue` example instead of directly using `Queue` is because `Queue` is an **interface** in Java, not a concrete class. Let me explain this in more detail.

#### Why `Queue` is an Interface:

- The **`Queue`** interface defines the contract for the operations that any queue implementation should provide, such as `add()`, `poll()`, `peek()`, and so on.
- **`Queue`** does not have any specific implementation on its own. It simply defines the methods that any concrete class implementing `Queue` must have.

#### `LinkedList` as a Concrete Implementation of `Queue`:

- **`LinkedList`** is a class that implements the `Queue` interface (as well as the `List` interface). This means that `LinkedList` provides the actual implementations of the methods defined in the `Queue` interface.
- We use `LinkedList` because it provides an implementation of the `Queue` operations, such as adding and removing elements in a queue-like manner (FIFO: First In First Out).

#### Other Implementations of `Queue`:

In addition to `LinkedList`, Java provides other implementations of the `Queue` interface. Some common ones are:

- **`PriorityQueue`**: A priority-based queue that orders elements based on their natural order or a comparator.
- **`ArrayDeque`**: A double-ended queue that can be used both as a queue (FIFO) or a stack (LIFO).

Here's an example using **`ArrayDeque`** as the concrete implementation of the **`Queue`** interface.

#### Why Use `ArrayDeque`?

- **`ArrayDeque`** is a resizable array-based implementation of the **`Deque`** (double-ended queue) interface.
- It can be used both as a **queue** (FIFO: First In First Out) and as a **stack** (LIFO: Last In First Out).
- **`ArrayDeque`** is generally faster than `LinkedList` for queue operations since it avoids the overhead of node allocation and pointer manipulation that comes with linked lists.

#### Queue Example Using `ArrayDeque`:

```java
import java.util.Queue;
import java.util.ArrayDeque;

public class QueueWithArrayDeque {
    public static void main(String[] args) {
        // Declare a Queue, but instantiate it as an ArrayDeque
        Queue<Integer> queue = new ArrayDeque<>();

        // Enqueue elements
        queue.add(10);
        queue.add(20);
        queue.add(30);

        // Peek at the front element
        System.out.println("Front element is: " + queue.peek());  // Output: 10

        // Dequeue the front element
        System.out.println("Dequeued element: " + queue.poll());  // Output: 10

        // Peek again after one removal
        System.out.println("Front element is: " + queue.peek());  // Output: 20

        // Dequeue the front element
        System.out.println("Dequeued element: " + queue.poll());  // Output: 20

        // Check if the queue is empty
        System.out.println("Is queue empty? " + queue.isEmpty());  // Output: false

        // Dequeue remaining elements
        System.out.println("Dequeued element: " + queue.poll());  // Output: 30

        // Now check if the queue is empty
        System.out.println("Is queue empty? " + queue.isEmpty());  // Output: true
    }
}
```

#### Key Differences Between `ArrayDeque` and `LinkedList`:

- **Performance**: `ArrayDeque` usually has better performance for enqueuing and dequeuing than `LinkedList` because it avoids the overhead associated with node pointers in a linked list.
- **Memory Usage**: `ArrayDeque` uses a contiguous array, which generally has better memory locality, whereas `LinkedList` uses more memory due to the overhead of storing references to the next and previous nodes.
- **Use Case**: If you only need a basic queue or stack functionality and are concerned with performance, `ArrayDeque` is often the better choice. However, if you need a list-like structure with frequent insertions and deletions at arbitrary positions, `LinkedList` may be more appropriate.

### Optional: Java `ListIterator`

#### Overview:

The **`ListIterator`** is a powerful interface in Java that allows you to traverse and modify elements in a **`List`**. Unlike a regular iterator, `ListIterator` provides more control, allowing:

- Bidirectional traversal (both forward and backward).
- Element modification during iteration.
- Ability to insert or remove elements at the current iterator position.

`ListIterator` is available for lists like `ArrayList`, `LinkedList`, and other classes that implement the `List` interface.

#### **Basic Usage of `ListIterator`**

To obtain a `ListIterator`, you need to call the `listIterator()` method from a `List` instance. Here’s an example of using `ListIterator` with a `LinkedList`.

```java
import java.util.LinkedList;
import java.util.ListIterator;

public class ListIteratorExample {
    public static void main(String[] args) {
        // Create a LinkedList of strings
        LinkedList<String> list = new LinkedList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");

        // Get a ListIterator
        ListIterator<String> it = list.listIterator();

        // Forward traversal using ListIterator
        System.out.println("Forward Traversal:");
        while (it.hasNext()) {
            System.out.println(it.next());
        }

        // Backward traversal using ListIterator
        System.out.println("\nBackward Traversal:");
        while (it.hasPrevious()) {
            System.out.println(it.previous());
        }
    }
}

// Output:

// Forward Traversal:
// Apple
// Banana
// Cherry

// Backward Traversal:
// Cherry
// Banana
// Apple

```

#### **Key Methods in `ListIterator`**

Here are some of the most important methods provided by `ListIterator`:

- **`hasNext()`**: Returns `true` if there are more elements when moving forward.
- **`next()`**: Returns the next element in the list.
- **`hasPrevious()`**: Returns `true` if there are elements when moving backward.
- **`previous()`**: Returns the previous element in the list.
- **`nextIndex()`**: Returns the index of the next element in the list.
- **`previousIndex()`**: Returns the index of the previous element in the list.
- **`remove()`**: Removes the last element returned by `next()` or `previous()`.
- **`set(E e)`**: Replaces the last element returned by `next()` or `previous()` with the specified element.
- **`add(E e)`**: Inserts the specified element into the list at the iterator's current position.

#### **Modifying the List During Iteration**

`ListIterator` allows you to modify the list while iterating over it. You can remove, replace, or add elements using `remove()`, `set()`, and `add()` respectively.

##### Example: Modifying List with `ListIterator`

```java
import java.util.LinkedList;
import java.util.ListIterator;

public class ModifyListWithIterator {
    public static void main(String[] args) {
        // Create a LinkedList of integers
        LinkedList<Integer> list = new LinkedList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);

        // Get a ListIterator
        ListIterator<Integer> it = list.listIterator();

        // Modify the list during iteration
        while (it.hasNext()) {
            int current = it.next();
            if (current == 2) {
                it.remove(); // Remove the element '2'
            }
            if (current == 3) {
                it.add(5); // Add element '5' after '3'
            }
        }

        // Print the modified list
        System.out.println(list); // Output: [1, 3, 5, 4]
    }
}

// Output:
// [1, 3, 5, 4]

// In this example:
// We remove the element 2 using it.remove().
// After encountering 3, we add 5 immediately after it using it.add(5).
```

#### (Optional) Manual Implementation of Queue:

The operations supported are:

- `enqueue`: Adds an element to the back of the queue.
- `dequeue`: Removes an element from the front of the queue.
- `peek`: Returns the front element without removing it.
- `isEmpty`: Checks if the queue is empty.

```java
class Queue {
    private int[] elements;
    private int size;
    private int front;
    private int rear;
    private int capacity;

    // Constructor to initialize the queue
    public Queue(int capacity) {
        this.capacity = capacity;
        elements = new int[capacity];
        size = 0;
        front = 0;
        rear = -1;
    }

    // Enqueue an element at the back of the queue
    public void enqueue(int value) {
        if (size == capacity) {
            System.out.println("Queue overflow");
            return;
        }
        rear = (rear + 1) % capacity;
        elements[rear] = value;
        size++;
    }

    // Dequeue an element from the front of the queue
    public int dequeue() {
        if (size == 0) {
            System.out.println("Queue underflow");
            return -1;
        }
        int dequeuedValue = elements[front];
        front = (front + 1) % capacity;
        size--;
        return dequeuedValue;
    }

    // Peek at the front element without removing it
    public int peek() {
        if (size == 0) {
            System.out.println("Queue is empty");
            return -1;
        }
        return elements[front];
    }

    // Check if the queue is empty
    public boolean isEmpty() {
        return size == 0;
    }
}
```

#### 

### 3. Further Questions from Lab 1 and Lab 2

### Access Modifiers, continued

```JAVA
// This code shows how to access private variables in Java using getters

class accessvariable{
    public int publicVariable = 10;     // Can be accessed from anywhere
    private int privateVariable = 20;   // Can be accessed only in this class
    protected int protectedVariable = 30; // Accessible in the same package or subclasses    
    
    public int getterfunction(){
        return privateVariable;
    }

}

public class Accessmodifierworking {
    public static void main(String[] args) {
        accessvariable example = new accessvariable();
        
        // Accessing variables within the same class
        System.out.println("Public: " + example.publicVariable);
        System.out.println("Private: " + example.getterfunction()); // using getter to access private attribute
        System.out.println("Protected: " + example.protectedVariable);
       
    }
}
```



```JAVA
// Example to depict the private access modifier
class accessvariable{
    public int publicVariable = 10;     // Can be accessed from anywhere
    private int privateVariable = 20;   // Can be accessed only in this class
    protected int protectedVariable = 30; // Accessible in the same package or subclasses       
}

public class AccessModifierExample {
    public static void main(String[] args) {
        accessvariable example = new accessvariable();
        
        // Accessing variables within the same class
        System.out.println("Public: " + example.publicVariable);
        System.out.println("Private: " + example.privateVariable); // This line will throw an error as private attributes of a class are not directly accessible
        System.out.println("Protected: " + example.protectedVariable);
       
    }
}
```



### Java doesn't support default parameters in its language design at all

In Java, if you want to achieve similar functionality to default parameters, one of the easiest way is to use:

1. Method overloading:

```java
// In Python, you can use default method param like __init__(personality="formal")
public class Car {
    private String personality;
    
    // Default constructor
    public Car() {
        this("formal");  // Calls the other constructor with default value
    }
    
    // Constructor with parameter
    public Car(String personality) {
        this.personality = personality;
    }
}
```





# 4. (Optional) Additional Questions From the Past Semester

Q1: [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

Q2: [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/description/) 

Q3: Backspace - [Backspace – Kattis, Kattis](https://open.kattis.com/problems/backspace) 

Q4: Even Up Solitaire - https://open.kattis.com/problems/evenup

Q5: Bungee Builder - TIME LIMIT! If the language is changed to C++, then the same algorithm meets the time limit. - https://open.kattis.com/problems/bungeebuilder

Q6: Number Colosseum - You may also encounter the TIME LIMIT problem - https://open.kattis.com/problems/numbercolosseum

Q7: Circuit Math - https://open.kattis.com/problems/circuitmath

Q8: Lyklagangriti - https://open.kattis.com/problems/lyklagangriti

Q9: Sim - based on Backspace. You may also encounter the TIME LIMIT problem - https://open.kattis.com/problems/sim

Q10: Ferry Loading III - https://open.kattis.com/problems/ferryloading3

Q11: Ferry Loading IV - https://open.kattis.com/problems/ferryloading4

Q12: Stock Prices - Priority Queue - https://open.kattis.com/problems/stockprices
