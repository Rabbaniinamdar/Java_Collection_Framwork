# 📌 Java Collections Framework – Brief & Improved Notes

## 1️⃣ What is a Collection?

A **Collection** is an object that groups multiple elements (objects) into a single unit, such as numbers, strings, or custom objects.
The **Java Collections Framework (JCF)** is a unified architecture of **interfaces, classes, and algorithms** in the `java.util` package that enables **dynamic, efficient, and reusable data management**.

📌 Real-world analogy:
Just like a **coin collection** or **student list**, Java collections store and manage groups of data.

---

## 2️⃣ Problems Before Java 1.2

Before Java 1.2, developers used:

* `Vector`
* `Stack`
* `Hashtable`
* Arrays

### ❌ Limitations:

* No **common interface** → difficult to write generic code
* **Inconsistent APIs** → confusing to learn and use
* Poor **interoperability** between classes
* Limited flexibility and reusability

### ✅ Solution:

Java 1.2 introduced the **Collections Framework**, providing:

* Standardized interfaces
* Consistent method naming
* Better performance and flexibility

---

## 3️⃣ Core Collection Hierarchy (High Level)

```
Iterable
   ↓
Collection
 ├── List
 ├── Set
 └── Queue
      └── Deque

Map (separate hierarchy)
```

### 🔹 Key Points:

* **Iterable** → Enables enhanced for-loop (`for-each`)
* **Collection** → Root interface for most collections
* **Map** → Separate hierarchy (not a Collection)

---

## 4️⃣ Main Interfaces Explained

### 📋 List

* Maintains **insertion order**
* Allows **duplicate elements**
* Index-based access

**Implementations:**

* `ArrayList`
* `LinkedList`
* `Vector`

📌 Example: Student list, shopping cart

---

### 🎯 Set

* Stores **unique elements only**
* No duplicates allowed
* Order depends on implementation

**Implementations:**

* `HashSet` → Unordered
* `LinkedHashSet` → Insertion order
* `TreeSet` → Sorted order

📌 Example: Email IDs, roll numbers

---

### 🚶 Queue

* Follows **FIFO (First In, First Out)**
* Used when order of processing matters

**Implementations:**

* `PriorityQueue`
* `LinkedList`

📌 Example: Doctor appointment queue

---

### 🔄 Deque (Double Ended Queue)

* Insert/remove from **both ends**
* Can act as **Queue or Stack**

**Implementation:**

* `ArrayDeque`

---

### 🗺 Map (Key-Value Pair)

* Stores data as **key → value**
* Keys must be **unique**
* Not part of Collection hierarchy

**Implementations:**

* `HashMap`
* `LinkedHashMap`
* `TreeMap`
* `Hashtable` (legacy)

📌 Example: Roll number → Student details

---

## 5️⃣ Why Interfaces Matter in JCF

* Enable **loose coupling**
* Allow **generic programming**
* Support **polymorphism**

```java
List<Integer> list = new ArrayList<>();
Set<String> set = new HashSet<>();
Map<Integer, String> map = new HashMap<>();
```

# 📌 ArrayList in Java – Improved & Crisp Notes

## 1️⃣ What is ArrayList?

`ArrayList` is a **resizable array implementation** of the **List interface**.
It allows **dynamic storage**, meaning the size can grow or shrink automatically at runtime.

### ✅ When to Use ArrayList

* When **size is unknown upfront**
* When **fast access (index-based)** is required
* When **order matters** and **duplicates are allowed**

📌 Backed internally by an **array**, unlike `LinkedList`.

---

## 2️⃣ List Interface Basics

`List` extends `Collection` and represents an **ordered collection** with **duplicate elements allowed**.

### Core Features:

* Maintains **insertion order**
* Supports **index-based access**
* Allows **duplicate values**

### Common Methods:

```java
add(), remove(), get(), set(), size(),
isEmpty(), contains(), addAll()
```

📌 **Use List instead of Set** when:

* Order must be preserved
* Duplicate elements are required

---

## 3️⃣ Creating an ArrayList

### 🔹 Default Constructor

```java
ArrayList<Integer> list = new ArrayList<>();
```

* Initial capacity = **10**

---

### 🔹 With Initial Capacity (Performance Optimization)

```java
ArrayList<Integer> list = new ArrayList<>(1000);
```

✅ Reduces **resizing overhead** when large data is expected

---

### 🔹 From Existing Array / Collection

```java
List<String> fixed = Arrays.asList("Mon", "Tue");
```

⚠️ Fixed-size list → **no add/remove**, only replace

```java
List<String> modifiable = new ArrayList<>(Arrays.asList("Mon", "Tue"));
```

✅ Fully modifiable list

---

### 🔹 Java 9+ Immutable List

```java
List<Integer> list = List.of(1, 2, 3);
```

❌ Cannot add/remove/update elements

---

## 4️⃣ Common Operations with Examples

### ➕ Add Elements

```java
list.add(50);          // end
list.add(1, 99);       // at index
list.addAll(Arrays.asList(4,5,6));
```

---

### 🔍 Access Elements

```java
list.get(0);   // index-based (0-based)
```

---

### ❌ Remove Elements

```java
list.remove(1);                    // by index
list.remove(Integer.valueOf(50));  // by object (first occurrence)
```

---

### 🔄 Replace Element

```java
list.set(1, 99);
```

---

### ✅ Check & Size

```java
list.contains(50);
list.size();
```

---

### 🔁 Iteration

```java
for (int x : list) { }
```

or

```java
for (int i = 0; i < list.size(); i++) { }
```

---

### 📤 Convert to Array

```java
Integer[] arr = list.toArray(new Integer[0]);
```

📌 Zero-length array is best practice

---

### 🔃 Sorting

```java
Collections.sort(list);
Collections.sort(list, null); // natural order
```

---

## 5️⃣ Internal Working of ArrayList (Very Important)

### 🔹 Internal Structure

* Uses an internal **Object[] array** called `elementData`
* Default capacity = **10**

### 🔹 Resizing Mechanism

* When capacity exceeds:

```
New Capacity = Old Capacity × 1.5
```

Example:

```
10 → 15 → 22 → ...
```

### 🔹 Add Operation

* If space available → O(1)
* If resize needed → new array created + copy → **O(n)**

### 🔹 Remove Operation

* Elements shift left → **O(n)**

### 🔹 Memory Optimization

* No automatic shrinking
* Manual trimming:

```java
list.trimToSize();
```

---

## 6️⃣ Time Complexity (Interview Favorite)

| Operation      | Time Complexity |
| -------------- | --------------- |
| get(index)     | O(1)            |
| add(end)       | O(1) amortized  |
| add(middle)    | O(n)            |
| remove(middle) | O(n)            |
| contains       | O(n)            |
| iteration      | O(n)            |

---

## 7️⃣ Capacity vs Size (Key Concept)

| Term         | Meaning                            |
| ------------ | ---------------------------------- |
| **Size**     | Number of elements (`list.size()`) |
| **Capacity** | Internal array length (default 10) |

📌 Capacity is **not directly accessible**
📌 Always specify capacity if list is large

---

## 8️⃣ ArrayList Limitations

❌ Slower insert/delete in middle
❌ Not thread-safe
❌ Memory overhead due to resizing

👉 Use `LinkedList` for frequent insertions/deletions
👉 Use `Vector` or `Collections.synchronizedList()` for thread safety

---

## 🎯 Interview Tip (One-Line Summary)

> **ArrayList** is best when **read operations are frequent**, order matters, and fast index access is required.


# 📌 Java Comparator – Improved & Interview-Ready Notes

## 1️⃣ What is Comparator?

`Comparator` is used to define **custom sorting logic** for objects when:

* Natural ordering is **not available**
* Multiple sorting strategies are required
* You don’t want to modify the class (`Comparable` not suitable)

📌 Commonly used with **Collections**, **Streams**, and **custom domain objects**.

---

## 2️⃣ Comparator Interface Basics

`Comparator<T>` is a **functional interface** with a single abstract method:

```java
int compare(T o1, T o2);
```

### 🔹 Return Value Meaning

| Return Value | Meaning                                   |
| ------------ | ----------------------------------------- |
| `< 0`        | `o1` comes before `o2`                    |
| `0`          | Both are equal (relative order preserved) |
| `> 0`        | `o1` comes after `o2`                     |

⚠️ Avoid direct subtraction for large numbers → risk of **integer overflow**.

---

## 3️⃣ Comparator vs Comparable (Quick Context)

| Comparable           | Comparator              |
| -------------------- | ----------------------- |
| Inside class         | Outside class           |
| Single sorting logic | Multiple sorting logics |
| `compareTo()`        | `compare()`             |
| Modifies class       | No class change         |

📌 **Prefer Comparator** for flexibility (interview favorite).

---

## 4️⃣ Using Comparator with `Collections.sort()`

### 🔹 Natural Ordering

```java
Collections.sort(list);      // uses Comparable
Collections.sort(list, null);
```

❌ Fails if object does not implement `Comparable`

---

### 🔹 Custom Ordering

```java
Collections.sort(list, comparator);
list.sort(comparator); // Java 8+
```

---

## 5️⃣ Comparator Implementations

### ✅ A) Separate Comparator Class

```java
class StringLengthComparator implements Comparator<String> {
    @Override
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
}
```

📌 Result:

```
"Date" → "Apple" → "Banana"
```

✔ Safer than `s1.length() - s2.length()`

---

### ✅ B) Lambda Expression (Java 8+)

```java
list.sort((a, b) -> Integer.compare(a.length(), b.length())); // Asc
list.sort((a, b) -> Integer.compare(b.length(), a.length())); // Desc
```

📌 Replaces anonymous class → **short & readable**

---

## 6️⃣ Real-World Example: Student GPA Sorting

### 🔹 Student Class

```java
class Student {
    private String name;
    private double gpa;
    // getters
}
```

### 🔹 Custom Logic (GPA ↓, Name ↑)

```java
students.sort((s1, s2) -> {
    int gpaCompare = Double.compare(s2.getGpa(), s1.getGpa());
    if (gpaCompare != 0) return gpaCompare;
    return s1.getName().compareTo(s2.getName());
});
```

📌 Output:

```
Akshit(3.9)
Bob(3.7)
Alice(3.5)
Charlie(3.5)
```

---

## 7️⃣ Best Practice: Comparator Utility Methods (Java 8+)

### 🔹 Single Field

```java
Comparator<Student> byGpa =
    Comparator.comparing(Student::getGpa).reversed();
```

---

### 🔹 Multiple Fields (Chained Comparator)

```java
Comparator<Student> byGpaThenName =
    Comparator.comparing(Student::getGpa)
              .reversed()
              .thenComparing(Student::getName);
```

✔ Clean
✔ Readable
✔ Production-ready
✔ Interview-preferred

---

## 8️⃣ Comparator Rules Cheat Sheet

| Requirement       | Correct Approach        |
| ----------------- | ----------------------- |
| Ascending number  | `Integer.compare(a, b)` |
| Descending number | `Integer.compare(b, a)` |
| Ascending string  | `a.compareTo(b)`        |
| Descending string | `b.compareTo(a)`        |
| Double values     | `Double.compare(a, b)`  |
| Multi-field       | `thenComparing()`       |

---

## 9️⃣ Stable Sorting (Important Concept)

Java’s `Collections.sort()` and `List.sort()` use **TimSort**, which is **stable**.

📌 Meaning:

* If `compare()` returns `0`
* Original order is **preserved**

✔ Critical for **multi-level sorting**
✔ Frequently asked in interviews

---

## 🔟 Common Interview Pitfalls

❌ Using subtraction for comparison
❌ Forgetting secondary sorting
❌ Mixing Comparable & Comparator incorrectly
❌ Ignoring null handling (use `Comparator.nullsFirst()`)

---

## 🎯 One-Line Interview Summary

> **Comparator** allows flexible, external, and multiple custom sorting strategies without modifying the original class.

---


