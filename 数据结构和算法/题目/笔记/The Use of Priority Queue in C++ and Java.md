Imagine a list of elements from left to right, if the judge statement is `true` when substitute the **left operator** for the **element on the left side** and right so, the list needn't change and the rightmost element is the highest one. For default of C++, it's like just add `<` between every two elements.
Default priority queue in C++ is a **max heap**
And for Java, the orientation is opposite (and the default is also opposite)

### The Core Concept: What is a Priority Queue?

A Priority Queue is an abstract data type that operates like a regular queue, but with a critical difference: each element has a "priority."

*   Instead of "first-in, first-out," a priority queue dequeues elements in order of their priority.
*   The element with the **highest priority** is always removed first, regardless of when it was added.

Think of an emergency room: patients are treated based on the severity of their condition (priority), not just their arrival time.

Internally, both C++ and Java implement their priority queues using a **heap**, which is a tree-based data structure that allows for very efficient (O(log n)) insertion and removal of the highest-priority element.

---

### Priority Queue in C++: `std::priority_queue`

The C++ implementation is part of the `<queue>` header. Its most important characteristic is its default behavior.

**Default Behavior: Max-Heap**
By default, `std::priority_queue` is a **max-heap**. This means the largest element is considered the highest priority and will always be at the `top()`.

**Basic Usage**

```cpp
#include <iostream>
#include <queue>
#include <vector>

int main() {
    // By default, a max-heap
    std::priority_queue<int> pq;

    pq.push(30);
    pq.push(100);
    pq.push(20);
    pq.push(50);

    std::cout << "Top element (max): " << pq.top() << std::endl; // 100

    std::cout << "Popping elements: ";
    while (!pq.empty()) {
        std::cout << pq.top() << " "; // Access the top element
        pq.pop();                      // Remove the top element
    }
    std::cout << std::endl;
    // Output: Popping elements: 100 50 30 20
    return 0;
}
```

**Key Methods**
*   `push()`: Adds an element.
*   `top()`: Returns a reference to the highest-priority (largest) element without removing it.
*   `pop()`: Removes the highest-priority element (returns `void`).
*   `empty()`: Checks if the queue is empty.
*   `size()`: Returns the number of elements.

**Customization (How to make a Min-Heap or use custom objects)**

This is where C++ can seem a bit verbose. You need to provide three template arguments:
`std::priority_queue<Type, Container, Comparator>`

1.  **Min-Heap (for primitive types):** The easiest way is to use the `std::greater` comparator.
    ```cpp
    // A min-heap
    std::priority_queue<int, std::vector<int>, std::greater<int>> min_pq;
    min_pq.push(30);
    min_pq.push(100);
    min_pq.push(20);
    // min_pq.top() will be 20
    ```

2.  **Custom Objects:** You can either overload the `<` operator in your class (for max-heap behavior) or provide a custom comparator struct.

    ```cpp
    struct Task {
        int priority;
        std::string name;

        // Overload '<' for default max-heap behavior
        bool operator<(const Task& other) const {
            return priority < other.priority;
        }
    };

    std::priority_queue<Task> task_pq;
    task_pq.push({10, "Low priority task"});
    task_pq.push({100, "High priority task"});
    // task_pq.top().name will be "High priority task"
    ```

---

### Priority Queue in Java: `java.util.PriorityQueue`

The Java implementation is part of the `java.util` package. Its default behavior is the **exact opposite** of C++.

**Default Behavior: Min-Heap**
By default, Java's `PriorityQueue` is a **min-heap**. This means the smallest element is considered the highest priority and will always be at the `head` of the queue.

**Basic Usage**

```java
import java.util.PriorityQueue;
import java.util.Collections;

class Main {
    public static void main(String[] args) {
        // By default, a min-heap
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        pq.add(30);
        pq.add(100);
        pq.add(20);
        pq.add(50);

        System.out.println("Peek at top element (min): " + pq.peek()); // 20

        System.out.print("Polling elements: ");
        while (!pq.isEmpty()) {
            System.out.print(pq.poll() + " "); // poll() retrieves and removes the head
        }
        System.out.println();
        // Output: Polling elements: 20 30 50 100
    }
}
```

**Key Methods**
*   `add()` or `offer()`: Adds an element.
*   `peek()`: Returns the highest-priority (smallest) element without removing it.
*   `poll()`: Removes *and returns* the highest-priority element.
*   `isEmpty()`: Checks if the queue is empty.
*   `size()`: Returns the number of elements.

**Customization (How to make a Max-Heap or use custom objects)**

Java's customization is often considered more intuitive. You pass a `Comparator` to the constructor.

1.  **Max-Heap (for primitive types):** The easiest way is using `Collections.reverseOrder()`.
    ```java
    // A max-heap
    PriorityQueue<Integer> max_pq = new PriorityQueue<>(Collections.reverseOrder());
    max_pq.add(30);
    max_pq.add(100);
    max_pq.add(20);
    // max_pq.peek() will be 100
    ```

2.  **Custom Objects:** You can either have the class implement the `Comparable` interface (for natural ordering) or pass a `Comparator` to the constructor (more flexible).

    ```java
    // Option 1: Implement Comparable for natural (min-heap) ordering
    class Task implements Comparable<Task> {
        int priority;
        String name;
        
        public Task(int priority, String name) {
            this.priority = priority;
            this.name = name;
        }

        @Override
        public int compareTo(Task other) {
            // Natural order is ascending by priority
            return Integer.compare(this.priority, other.priority);
        }
    }

    PriorityQueue<Task> task_pq = new PriorityQueue<>();
    task_pq.add(new Task(10, "Low priority task"));
    task_pq.add(new Task(100, "High priority task"));
    // task_pq.peek().name will be "Low priority task"
    
    // Option 2: Use a lambda for ad-hoc max-heap ordering
    PriorityQueue<Task> max_task_pq = new PriorityQueue<>((a, b) -> b.priority - a.priority);
    ```

---

### Side-by-Side Comparison

| Feature                  | C++ (`std::priority_queue`)                          | Java (`java.util.PriorityQueue`)                     |
| :----------------------- | :--------------------------------------------------- | :--------------------------------------------------- |
| **Default Behavior**     | **Max-Heap** (largest is highest priority)           | **Min-Heap** (smallest is highest priority)          |
| **Header / Package**     | `#include <queue>`                                   | `import java.util.PriorityQueue;`                    |
| **Declaration**          | `std::priority_queue<int> pq;`                       | `PriorityQueue<Integer> pq = new PriorityQueue<>();` |
| **Adding Element**       | `pq.push(val);`                                      | `pq.add(val);` or `pq.offer(val);`                   |
| **Viewing Top Element**  | `pq.top()`                                           | `pq.peek()`                                          |
| **Removing Top Element** | `pq.pop()` (returns `void`)                          | `pq.poll()` (returns the element)                    |
| **Creating Min-Heap**    | `std::priority_queue<T, V, std::greater<T>>`         | This is the default behavior.                        |
| **Creating Max-Heap**    | This is the default behavior.                        | `new PriorityQueue<>(Collections.reverseOrder())`    |
| **Custom Object Order**  | Overload `operator<` or provide a comparator struct. | Implement `Comparable` or provide a `Comparator`.    |