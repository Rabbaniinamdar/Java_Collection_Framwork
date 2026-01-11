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

---\

















Below are **clean, interview-ready prescribed notes** derived from your content, **expanded with clear explanations, best practices, and pitfalls**. You can use this directly for **revision, interviews, or documentation**.

---

# 📘 Java Comparator – Prescribed Notes (Detailed)

## 1️⃣ What is Comparator in Java?

**Comparator<T>** is a **functional interface** used to define **custom sorting logic** for objects.

* Used when:

  * Objects **do not have natural ordering**
  * You need **multiple sorting strategies**
* Belongs to: `java.util`

```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

---

## 2️⃣ Comparator vs Comparable (MOST ASKED)

| Comparable                 | Comparator                  |
| -------------------------- | --------------------------- |
| Defines **natural order**  | Defines **custom order**    |
| Implemented inside class   | Implemented externally      |
| `compareTo()`              | `compare()`                 |
| Only **one sorting logic** | **Multiple sorting logics** |
| Modifies class             | No class modification       |

👉 Example:

* `String`, `Integer` → Comparable
* `Student`, `Employee` → Comparator

---

## 3️⃣ How compare(o1, o2) Works

Return value determines ordering:

| Return value | Meaning                 |
| ------------ | ----------------------- |
| `< 0`        | o1 comes **before** o2  |
| `0`          | o1 and o2 are **equal** |
| `> 0`        | o1 comes **after** o2   |

### Example (Ascending Order)

```java
compare(3, 5) → 3 - 5 = -2 → 3 comes first
compare(5, 3) → 5 - 3 = 2  → 3 comes first
```

### Example (Descending Order)

```java
compare(3, 5) → 5 - 3 = 2 → 5 comes first
compare(5, 3) → 3 - 5 = -2 → 5 comes first
```

---

## 4️⃣ How Sorting is Applied

You pass a Comparator to sorting methods:

```java
Collections.sort(list, comparator);
list.sort(comparator); // Java 8+
```

👉 Sorting algorithm repeatedly calls `compare()` until list is ordered.

---

## 5️⃣ Implementation Approaches

---

### ✅ 1. Comparator Using Implementation Class

```java
class StringLengthComparator implements Comparator<String> {
    @Override
    public int compare(String s1, String s2) {
        return s1.length() - s2.length(); // ascending
    }
}
```

**Usage**

```java
words.sort(new StringLengthComparator());
```

📌 Best when:

* Logic is **complex**
* Reused in multiple places

---

### ✅ 2. Anonymous Class

```java
words.sort(new Comparator<String>() {
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
});
```

📌 Less common after Java 8

---

### ✅ 3. Lambda Expression (Java 8+)

```java
// Ascending
words.sort((s1, s2) -> s1.length() - s2.length());

// Descending
words.sort((s1, s2) -> s2.length() - s1.length());
```

📌 Lambda works because:

* Comparator is a **functional interface**
* Lambda provides implementation of `compare()`

---

## 6️⃣ Custom Object Example – Student

### Student Class

```java
class Student {
    private String name;
    private double gpa;

    // constructor, getters
}
```

### Sorting Requirement

1. GPA → **Descending**
2. Name → **Ascending** (tie-breaker)

---

### ❌ Bad Approach (Risky)

```java
(int)(s2.getGpa() - s1.getGpa());
```

⚠️ Problems:

* Precision loss
* Wrong ordering for close values

---

### ✅ Correct Lambda Approach

```java
students.sort((s1, s2) -> {
    int gpaCompare = Double.compare(s2.getGpa(), s1.getGpa());
    if (gpaCompare != 0) return gpaCompare;
    return s1.getName().compareTo(s2.getName());
});
```

---

## 7️⃣ Java 8 Comparator Utility Methods (VERY IMPORTANT)

### 🔹 Single Field Sorting

```java
Comparator.comparing(Student::getGpa)
```

### 🔹 Descending Order

```java
Comparator.comparing(Student::getGpa).reversed()
```

### 🔹 Multi-field Sorting (Chaining)

```java
students.sort(
    Comparator.comparing(Student::getGpa).reversed()
              .thenComparing(Student::getName)
);
```

📌 Explanation:

* `comparing()` → primary sort
* `thenComparing()` → tie-breaker
* `reversed()` → reverse previous comparator

---

## 8️⃣ Primitive-Specific Comparators (BEST PRACTICE)

Avoid subtraction for numbers 👇

| Type   | Method                         |
| ------ | ------------------------------ |
| int    | `Comparator.comparingInt()`    |
| double | `Comparator.comparingDouble()` |
| long   | `Comparator.comparingLong()`   |

```java
students.sort(
    Comparator.comparingDouble(Student::getGpa).reversed()
);
```

---

## 9️⃣ String Comparison

```java
// Ascending
s1.getName().compareTo(s2.getName());

// Descending
s2.getName().compareTo(s1.getName());
```

---

## 🔟 Stable Sorting in Java

* Java uses **Timsort**
* Sorting is **stable**
* Equal elements retain original order

📌 Important in interviews:

> “Java’s sorting algorithm is stable, so elements considered equal maintain insertion order.”

---

## 1️⃣1️⃣ Comparator Quick Rules Table

| Requirement       | Code                     |
| ----------------- | ------------------------ |
| Ascending int     | `o1 - o2`                |
| Descending int    | `o2 - o1`                |
| Ascending double  | `Double.compare(o1, o2)` |
| Descending double | `Double.compare(o2, o1)` |
| Ascending String  | `s1.compareTo(s2)`       |
| Descending String | `s2.compareTo(s1)`       |

---

## 1️⃣2️⃣ Interview Tips ⭐

✔ Always use **custom objects** (Employee / Student)
✔ Prefer **Comparator.comparing()** over manual logic
✔ Avoid subtraction for floating-point comparison
✔ Mention **stable sort (Timsort)**
✔ Explain **why Comparator over Comparable**

---

## 1️⃣3️⃣ One-Line Interview Answer

> “Comparator is a functional interface used to define custom sorting logic externally. It allows multiple sorting strategies without modifying the class and supports lambda expressions and method chaining in Java 8+.”

---

Below are **prescribed, interview-ready notes on Java Comparable**, rewritten from your content with **clear structure, deep explanation, best practices, and interview insights**. You can use this for **revision, interviews, or documentation**.

---

# 📘 Java Comparable Interface – Prescribed Notes (Detailed)

---

## 1️⃣ What is Comparable in Java?

**Comparable<T>** is an interface used to define the **natural (default) ordering** of objects of a class.

* Implemented **inside the class**
* Provides **one fixed sorting logic**
* Used automatically by Java sorting APIs

```java
public interface Comparable<T> {
    int compareTo(T other);
}
```

📌 Examples of natural ordering:

* `Integer` → numeric ascending
* `String` → lexicographical (dictionary) order
* `Student` → GPA / roll number / ID (as defined by developer)

---

## 2️⃣ Why Comparable is Needed

Java needs to know **how to compare objects** while sorting.

* For **primitives & wrappers** → already implemented
* For **custom classes** → Java has no idea how to compare → must implement Comparable

Without Comparable:

```java
students.sort(null); // ❌ ClassCastException
```

With Comparable:

```java
students.sort(null); // ✅ Works using compareTo()
```

---

## 3️⃣ Comparable vs Comparator (VERY IMPORTANT)

| Feature          | Comparable             | Comparator          |
| ---------------- | ---------------------- | ------------------- |
| Defined          | Inside the class       | Outside the class   |
| Method           | `compareTo()`          | `compare()`         |
| Sorting logic    | Single (natural order) | Multiple / flexible |
| Objects compared | `this` vs `other`      | `o1` vs `o2`        |
| Usage            | Default sorting        | Custom sorting      |
| Java 8 Lambdas   | ❌ Not needed           | ✅ Supported         |

👉 **Interview rule**:

* **Comparable** → “What is the default order?”
* **Comparator** → “How else can I sort?”

---

## 4️⃣ compareTo() Method – Core Logic

```java
int compareTo(T other)
```

### Return value meaning:

| Return | Meaning                            |
| ------ | ---------------------------------- |
| `< 0`  | this object comes **before** other |
| `0`    | both objects are **equal**         |
| `> 0`  | this object comes **after** other  |

---

## 5️⃣ Implementing Comparable – Student Example

### Student Class (Natural Order = GPA Descending)

```java
public class Student implements Comparable<Student> {
    private String name;
    private double gpa;

    @Override
    public int compareTo(Student other) {
        return Double.compare(other.getGpa(), this.getGpa());
    }
}
```

### Why `Double.compare()`?

❌ Bad approach:

```java
(other.getGpa() - this.getGpa()); // precision + overflow risk
```

✅ Correct approach:

```java
Double.compare(a, b);
```

**Benefits**:

* Handles precision
* Handles NaN
* No overflow
* Interview-approved

---

## 6️⃣ How Sorting Works Internally

```java
student1.compareTo(student2)
```

* Java calls `compareTo()` repeatedly
* Sorting algorithm (Timsort) rearranges elements based on return values
* Sorting is **stable**

---

## 7️⃣ Sorting Using Comparable

```java
students.sort(null);
Collections.sort(students);
```

📌 `null` means:

> “Use natural ordering (compareTo)”

---

## 8️⃣ Example Output (Stable Sort)

### Input

* Alice (3.5)
* Bob (3.7)
* Charlie (3.5)

### Output (Descending GPA)

```
Bob(3.7)
Alice(3.5)
Charlie(3.5)
```

📌 Alice remains before Charlie → **stable sort**

---

## 9️⃣ compareTo() Rules with Example

| Comparison          | Result   | Meaning          |
| ------------------- | -------- | ---------------- |
| this=3.7, other=3.5 | negative | this comes first |
| this=3.5, other=3.5 | zero     | order unchanged  |
| this=3.5, other=3.7 | positive | this comes after |

---

## 🔟 Multi-Field Sorting with Comparable

Comparable allows **only one method**, but logic can include **tie-breakers**.

```java
@Override
public int compareTo(Student other) {
    int gpaCompare = Double.compare(other.getGpa(), this.getGpa());
    if (gpaCompare != 0) {
        return gpaCompare;
    }
    return this.getName().compareTo(other.getName()); // tie-breaker
}
```

📌 Primary → GPA (desc)
📌 Secondary → Name (asc)

---

## 1️⃣1️⃣ Comparable with Primitives & Wrappers

```java
List<Integer> nums = Arrays.asList(99, 1, 5);
nums.sort(null); // Works
```

Why?

* `Integer` implements `Comparable<Integer>`

---

## 1️⃣2️⃣ Comparable in Sorted Collections

Used automatically by:

| Collection      | Purpose                |
| --------------- | ---------------------- |
| `TreeSet`       | Sorted unique elements |
| `TreeMap`       | Sorted keys            |
| `PriorityQueue` | Priority ordering      |

📌 If `compareTo()` is wrong → data structure behaves incorrectly.

---

## 1️⃣3️⃣ equals() and compareTo() Contract

⚠️ **VERY IMPORTANT**

If:

```java
compareTo() == 0
```

Then:

```java
equals() should return true
```

Otherwise:

* `TreeSet` may drop elements
* `TreeMap` may overwrite keys

👉 Best practice:

* Override `equals()` and `hashCode()` consistently

---

## 1️⃣4️⃣ When NOT to Use Comparable

❌ When:

* Multiple sorting orders required
* Sorting logic changes frequently
* You don’t own the class

✅ Use **Comparator** instead

---

## 1️⃣5️⃣ Interview One-Line Answer ⭐

> “Comparable is used to define the natural ordering of objects inside the class using compareTo(). It supports only one default sorting logic and is automatically used by Java sorting and sorted collections.”

---

## 1️⃣6️⃣ Comparable vs Comparator – Interview Closing Line

> “Comparable defines *what is the natural order*, while Comparator defines *how else we can sort*.”

---
Below are **prescribed, interview-ready notes** for **SortedMap, NavigableMap, and TreeMap**, rewritten cleanly from your content with **deep explanations, examples, and interview insights**—same style as your Comparable/Comparator notes.

---

# 📘 Java SortedMap & NavigableMap – Prescribed Notes (Detailed)

---

## 1️⃣ What is SortedMap in Java?

**SortedMap** is a sub-interface of `Map` that **maintains its keys in sorted order**.

* Sorting is done:

  * By **natural ordering** of keys (`Comparable`)
  * OR by a **custom Comparator**
* Values are **not sorted**, only keys are

```java
Map → SortedMap → NavigableMap
```

📌 **Important**: Sorting happens **automatically** when keys are inserted.

---

## 2️⃣ Primary Implementation – TreeMap

```java
SortedMap<K, V> map = new TreeMap<>();
```

### Why TreeMap?

* Backed by **Red-Black Tree**
* Self-balancing Binary Search Tree
* Guarantees **O(log n)** time for:

  * put()
  * get()
  * remove()
  * containsKey()

---

## 3️⃣ Red-Black Tree (Interview Explanation)

A **Red-Black Tree**:

* Is a **self-balancing BST**
* Prevents tree from becoming skewed
* Ensures height ≈ `log n`

👉 This avoids worst-case `O(n)` behavior seen in unbalanced trees.

---

## 4️⃣ Basic SortedMap Example

```java
SortedMap<String, Integer> map = new TreeMap<>();

map.put("Vivek", 91);
map.put("Shubham", 99);
map.put("Mohit", 78);
map.put("Vipul", 77);

System.out.println(map);
```

### Output (Natural String Order)

```
{Mohit=78, Shubham=99, Vipul=77, Vivek=91}
```

📌 Keys are sorted **lexicographically**.

---

## 5️⃣ SortedMap with Integer Keys

```java
SortedMap<Integer, String> map = new TreeMap<>();

map.put(91, "Vivek");
map.put(99, "Shubham");
map.put(78, "Mohit");
map.put(77, "Vipul");
```

### Output

```
{77=Vipul, 78=Mohit, 91=Vivek, 99=Shubham}
```

📌 Integer keys follow **natural ascending order**.

---

## 6️⃣ Essential SortedMap Methods

Assume keys = `{77, 78, 91, 99}`

| Method          | Description   | Result    |
| --------------- | ------------- | --------- |
| `firstKey()`    | Smallest key  | 77        |
| `lastKey()`     | Largest key   | 99        |
| `headMap(91)`   | `< 91`        | `{77,78}` |
| `tailMap(91)`   | `≥ 91`        | `{91,99}` |
| `subMap(78,91)` | `≥78 and <91` | `{78}`    |

📌 These methods return **views**, not copies.

---

## 7️⃣ Custom Sorting in SortedMap (Comparator)

### Descending Order Example

```java
SortedMap<Integer, String> map =
    new TreeMap<>((a, b) -> b - a);
```

### Output

```
{99=Shubham, 91=Vivek, 78=Mohit, 77=Vipul}
```

📌 Sorting logic is applied **only to keys**.

---

## 8️⃣ NavigableMap – Advanced SortedMap

**NavigableMap** extends SortedMap and adds **navigation & range operations**.

```java
NavigableMap<Integer, String> map = new TreeMap<>();
```

### Key Additions:

* Closest key search
* Forward & backward navigation
* Descending views

---

## 9️⃣ NavigableMap Navigation Methods

Assume keys = `{1, 3, 5}`

```java
nav.floorKey(4);    // 3
nav.ceilingKey(2); // 3
nav.lowerKey(3);   // 1
nav.higherKey(3);  // 5
```

### Meaning Table

| Method        | Meaning        |
| ------------- | -------------- |
| floorKey(k)   | ≤ k (greatest) |
| ceilingKey(k) | ≥ k (smallest) |
| lowerKey(k)   | < k            |
| higherKey(k)  | > k            |

---

## 🔟 Entry-Level Navigation

```java
nav.higherEntry(1);
```

Returns:

```java
Map.Entry<Integer, String>
```

📌 Useful when **both key and value** are needed.

---

## 1️⃣1️⃣ Descending Views

```java
NavigableMap<Integer, String> desc = map.descendingMap();
```

📌 Creates a **reverse-sorted view**, not a new map.

---

## 1️⃣2️⃣ Time Complexity Comparison

| Operation      | HashMap  | TreeMap  |
| -------------- | -------- | -------- |
| put/get/remove | O(1) avg | O(log n) |
| containsKey    | O(1) avg | O(log n) |
| Iteration      | Random   | Sorted   |
| Ordering       | ❌ No     | ✅ Yes    |

👉 Use **TreeMap** when sorted data is required.

---

## 1️⃣3️⃣ Reference Type Flexibility (IMPORTANT)

```java
Map<String,Integer> m = new TreeMap<>();
SortedMap<String,Integer> sm = new TreeMap<>();
NavigableMap<String,Integer> nm = new TreeMap<>();
```

| Reference    | Access                    |
| ------------ | ------------------------- |
| Map          | Basic map ops             |
| SortedMap    | + range & boundary ops    |
| NavigableMap | + navigation & descending |

📌 Object decides behavior, reference decides **access**.

---

## 1️⃣4️⃣ Key Rules & Constraints

✔ Keys must be **Comparable** or Comparator must be provided
✔ Duplicate keys → **value replaced**
✔ Null keys → ❌ (TreeMap throws NPE)
✔ Values → can be null

---

## 1️⃣5️⃣ Real-World Use Cases

| Scenario            | Why TreeMap        |
| ------------------- | ------------------ |
| Leaderboard         | Sorted scores      |
| Phone directory     | Alphabetical order |
| Configuration files | Ordered properties |
| Range queries       | headMap / subMap   |

---

## 1️⃣6️⃣ Interview One-Line Answers ⭐

### SortedMap

> SortedMap maintains keys in sorted order using natural ordering or a Comparator.

### NavigableMap

> NavigableMap extends SortedMap by providing navigation methods like floorKey and ceilingKey.

### TreeMap

> TreeMap is a Red-Black Tree based implementation of NavigableMap with O(log n) performance.

---

## 1️⃣7️⃣ SortedMap vs PriorityQueue (Quick Clarification)

| Feature        | TreeMap           | PriorityQueue   |
| -------------- | ----------------- | --------------- |
| Data structure | Map               | Queue           |
| Sorting        | Entire map        | Only head       |
| Access         | Random            | Head only       |
| Use case       | Ordered key-value | Min/Max element |

---
Below are **prescribed, interview-ready notes on `Hashtable`**, written in the **same structured + deep-explanation style** as your previous topics (Comparable, SortedMap, TreeMap). This is exactly how interviewers expect the answer.

---

# 📘 Java Hashtable – Prescribed Notes (Detailed)

---

## 1️⃣ What is Hashtable in Java?

**Hashtable** is a **legacy key–value data structure** introduced in **JDK 1.0** that provides **thread-safe storage** by synchronizing **every method**.

* Part of **java.util**
* Implements **Map interface**
* Predecessor of **HashMap**

```java
public class Hashtable<K, V>
    extends Dictionary<K, V>
    implements Map<K, V>
```

📌 Today, Hashtable is kept **only for backward compatibility**.

---

## 2️⃣ Why Hashtable Was Introduced?

Before Java had:

* `synchronized` blocks
* `ConcurrentHashMap`

Hashtable was created to:

* Provide **built-in thread safety**
* Avoid inconsistent data in multithreaded programs

---

## 3️⃣ Core Characteristics of Hashtable

✔ Thread-safe
✔ Synchronized at method level
❌ No null key
❌ No null value
❌ Poor performance
❌ Legacy (avoid in new code)

---

## 4️⃣ Hashtable vs HashMap (MOST ASKED)

| Feature         | HashMap   | Hashtable         |
| --------------- | --------- | ----------------- |
| Thread-safe     | ❌ No      | ✅ Yes             |
| Synchronization | None      | Full method-level |
| Null keys       | 1 allowed | ❌ Not allowed     |
| Null values     | Allowed   | ❌ Not allowed     |
| Performance     | Fast      | Slow              |
| Introduced      | JDK 1.2   | JDK 1.0           |
| Usage           | Preferred | Legacy            |

---

## 5️⃣ Why Hashtable Disallows Null?

```java
table.put(null, 1);  // NullPointerException
table.put("A", null); // NullPointerException
```

### Reason:

* Hashtable internally uses `hashCode()` and `equals()`
* Null handling wasn’t safely designed in early Java

📌 HashMap was redesigned later to allow nulls.

---

## 6️⃣ Basic Hashtable Usage

```java
Hashtable<String, Integer> table = new Hashtable<>();

table.put("apple", 1);
table.put("banana", 3);
table.put("cherry", 2);

System.out.println(table.get("banana"));   // 3
System.out.println(table.containsKey("cherry")); // true
```

📌 Order is **not guaranteed** (same as HashMap).

---

## 7️⃣ How Hashtable Achieves Thread Safety

```java
public synchronized V put(K key, V value)
```

* Every method is synchronized
* Entire Hashtable is locked
* Only **one thread** can access it at a time

📌 This is called **coarse-grained locking**

---

## 8️⃣ Thread Safety Example (Interview Favorite)

### ❌ HashMap (Not Thread-Safe)

```java
Map<String, Integer> map = new HashMap<>();
```

Two threads writing → **lost updates**, inconsistent size.

---

### ✅ Hashtable (Thread-Safe)

```java
Hashtable<String, Integer> table = new Hashtable<>();
```

Two threads writing → **correct final size**

📌 Correctness ✔
📌 Performance ❌

---

## 9️⃣ Why Hashtable Is Slow (Very Important)

### Problems with Hashtable

❌ Locks entire structure
❌ Reads block writes
❌ Writes block reads
❌ Threads wait unnecessarily

```text
Thread 1 → reading
Thread 2 → reading (blocked ❌)
Thread 3 → writing (blocked ❌)
```

➡️ **No parallelism**

---

## 🔟 Collision Handling in Hashtable

* Uses **LinkedList** for collisions
* ❌ No Tree (unlike HashMap JDK 8+)

📌 Leads to slower performance under high collision scenarios.

---

## 1️⃣1️⃣ Iteration Behavior

* Enumeration (legacy)
* Fail-fast behavior on modification

```java
Enumeration<String> keys = table.keys();
```

📌 Not modern, avoid usage.

---

## 1️⃣2️⃣ Hashtable vs ConcurrentHashMap (CRITICAL)

| Feature           | Hashtable       | ConcurrentHashMap    |
| ----------------- | --------------- | -------------------- |
| Locking           | Whole map       | Bucket/segment level |
| Read operations   | Synchronized    | Lock-free            |
| Write concurrency | ❌ Single thread | ✅ Multiple threads   |
| Performance       | Poor            | Excellent            |
| Null support      | ❌ No            | ❌ No                 |
| Java version      | JDK 1.0         | JDK 1.5+             |

---

## 1️⃣3️⃣ Why ConcurrentHashMap Is Better

✔ Fine-grained locking
✔ High throughput
✔ Scales well
✔ Modern APIs (`computeIfAbsent`)
✔ Production-ready

📌 **Industry standard replacement** for Hashtable.

---

## 1️⃣4️⃣ When Should You Use Hashtable?

✔ Very rare legacy systems
✔ Old APIs expecting Hashtable

❌ New applications
❌ Performance-critical systems

👉 Use:

* **HashMap** → single-threaded
* **ConcurrentHashMap** → multi-threaded

---

## 1️⃣5️⃣ Interview One-Line Answers ⭐

### What is Hashtable?

> Hashtable is a legacy synchronized Map implementation that provides thread safety by locking the entire data structure.

### Why is Hashtable slow?

> Because every method is synchronized, allowing only one thread at a time.

### Why is Hashtable deprecated?

> Due to poor scalability and availability of ConcurrentHashMap.

---

# 📘 Java ConcurrentHashMap 

## 1️⃣ What is ConcurrentHashMap?

**ConcurrentHashMap** is a **thread-safe and high-performance** implementation of `Map`, designed for **concurrent read/write access**.

* Introduced in **JDK 1.5**
* Part of `java.util.concurrent`
* Implements **ConcurrentMap → Map**
* **No null keys or values**

```java
public class ConcurrentHashMap<K,V>
    implements ConcurrentMap<K,V>
```

📌 It is the **modern replacement for Hashtable**.

---

## 2️⃣ Why ConcurrentHashMap Is Needed

### Problems with older Maps

| Map       | Problem                                      |
| --------- | -------------------------------------------- |
| HashMap   | Not thread-safe (data corruption)            |
| Hashtable | Thread-safe but **very slow** (full locking) |

👉 **ConcurrentHashMap solves both**:

* Thread safety ✔
* High performance ✔

---

## 3️⃣ Key Characteristics

✔ Thread-safe
✔ High concurrency
✔ Lock-free reads
✔ Fine-grained locking
✔ No null keys/values
✔ Scales well under load

---

## 4️⃣ Internal Evolution (JDK 7 vs JDK 8+)

| Version    | Mechanism          | Description                       |
| ---------- | ------------------ | --------------------------------- |
| **JDK 7**  | Segments (16)      | Each segment had its own lock     |
| **JDK 8+** | CAS + synchronized | No segments, bucket-level locking |

---

## 5️⃣ JDK 7 – Segment-Based Locking

### How it works

* Map divided into **16 segments**
* Each segment = independent HashMap + lock
* Only the affected segment is locked

```text
Segment 0 | Segment 1 | Segment 2 | ... | Segment 15
```

### Advantages

✔ Up to **16 concurrent writers**
✔ Reads mostly lock-free
✔ Better than Hashtable

📌 **Limitation**: Fixed concurrency level.

---

## 6️⃣ JDK 8+ – CAS (Compare-And-Swap) Approach

### Key Idea

> **Avoid locks whenever possible**

Uses:

* **CAS (lock-free atomic operations)**
* `synchronized` only for:

  * Resizing
  * High collision buckets

---

### CAS Explained (Interview Favorite)

```text
Thread reads value = 42
CAS: if (value == 42) → update to 50
```

* If value unchanged → update succeeds
* If changed by another thread → retry

✔ No blocking
✔ No thread waiting

---

## 7️⃣ Locking in JDK 8+

| Operation              | Locking                  |
| ---------------------- | ------------------------ |
| Read                   | ❌ No lock                |
| Write (low contention) | ❌ CAS                    |
| Write (collision)      | 🔒 Bucket-level          |
| Resize                 | 🔒 Partial (incremental) |

📌 **Never locks entire map**

---

## 8️⃣ Performance Comparison (VERY IMPORTANT)

| Scenario        | HashMap | Hashtable  | ConcurrentHashMap |
| --------------- | ------- | ---------- | ----------------- |
| Single thread   | Fastest | Slow       | Slightly slower   |
| Multiple reads  | Unsafe  | Blocked    | Fastest           |
| Multiple writes | Corrupt | Serialized | Parallel          |
| High contention | ❌       | ❌          | ✅ Best            |

---

## 9️⃣ Time Complexity

Same as HashMap:

| Operation | Complexity |
| --------- | ---------- |
| get       | O(1) avg   |
| put       | O(1) avg   |
| remove    | O(1) avg   |

📌 Maintains performance **even under concurrency**.

---

## 🔟 Basic Usage

```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();

map.put("A", 1);
map.put("B", 2);
```

❌ Not allowed:

```java
map.put(null, 1);  // NullPointerException
```

---

## 1️⃣1️⃣ Atomic Methods (VERY IMPORTANT)

ConcurrentHashMap provides **atomic operations**:

```java
map.putIfAbsent("key", 1);
map.computeIfAbsent("key", k -> 0);
map.replace("key", 1, 2);
```

📌 These prevent race conditions without explicit synchronization.

---

## 1️⃣2️⃣ Reference Flexibility

```java
Map<String,Integer> m = new ConcurrentHashMap<>();
ConcurrentMap<String,Integer> cm = new ConcurrentHashMap<>();
```

| Reference     | Access            |
| ------------- | ----------------- |
| Map           | Basic operations  |
| ConcurrentMap | Atomic operations |

---

## 1️⃣3️⃣ Multithreading Example (Conceptual)

| Map Type          | Final Size    |
| ----------------- | ------------- |
| HashMap           | ❌ ~1800       |
| Hashtable         | ✅ 2000 (slow) |
| ConcurrentHashMap | ✅ 2000 (fast) |

---

## 1️⃣4️⃣ ConcurrentHashMap vs Hashtable

| Feature           | Hashtable | ConcurrentHashMap |
| ----------------- | --------- | ----------------- |
| Locking           | Whole map | Bucket/segment    |
| Read blocking     | Yes       | No                |
| Write scalability | Poor      | Excellent         |
| Performance       | Low       | High              |
| Modern APIs       | ❌         | ✅                 |

---

## 1️⃣5️⃣ When to Use ConcurrentHashMap

✔ Caches
✔ Counters
✔ Session management
✔ High-traffic web apps (Spring Boot)
✔ Multi-threaded services

❌ Avoid in single-threaded apps → use HashMap

Below is a **clear, deep, and interview-ready explanation** of **Java `Iterable` and `Iterator`**, written step-by-step so you understand **what**, **why**, **how**, and **when**—not just definitions.

---

# 📘 Java Iterable & Iterator — Detailed Explanation

---

## 1️⃣ Why Do We Need Iterable & Iterator?

Before Java 5:

* Loops were index-based (`for`, `while`)
* Worked only for arrays or index-based collections

Problem:

* Different collections store data differently

  * ArrayList → index-based
  * HashSet → no index
  * TreeSet → sorted structure

👉 Java needed **one common way** to traverse *any* collection.

✅ **Solution**: `Iterable` + `Iterator`

---

## 2️⃣ What is Iterable?

### 📌 Definition

**Iterable** is an interface that **marks a class as iterable**, meaning:

> “Objects of this class can be traversed one by one.”

```java
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

### 🔑 Key Points

* It is a **contract**
* It does **not** perform iteration itself
* It only promises:
  👉 *“I can give you an Iterator”*

---

## 3️⃣ Why Iterable Is Important

If a class implements `Iterable`, it:

* Can be used in **enhanced for-loop (for-each)**
* Works uniformly with all collections

```java
for (int n : list) {
    System.out.println(n);
}
```

📌 This is possible **only because** `list` implements `Iterable`.

---

## 4️⃣ What is Iterator?

### 📌 Definition

**Iterator** is the **actual object that performs traversal** over elements.

```java
public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove();
}
```

### 🔑 Key Points

* Returned by `iterator()`
* Works like a **cursor**
* Moves step-by-step through elements

---

## 5️⃣ Iterable vs Iterator (Core Difference)

| Aspect         | Iterable          | Iterator              |
| -------------- | ----------------- | --------------------- |
| Purpose        | Enables iteration | Performs iteration    |
| Responsibility | Provide iterator  | Traverse elements     |
| Key method     | `iterator()`      | `hasNext()`, `next()` |
| Used directly  | Rare              | Frequently            |

🧠 **Easy memory trick**:

> Iterable = permission
> Iterator = execution

---

## 6️⃣ How for-each Loop Works Internally (VERY IMPORTANT)

### Code you write:

```java
for (int n : list) {
    System.out.println(n);
}
```

### What compiler converts it into:

```java
Iterator<Integer> it = list.iterator();
while (it.hasNext()) {
    int n = it.next();
    System.out.println(n);
}
```

📌 **for-each is just syntactic sugar** over Iterator.

---

## 7️⃣ Iterator Cursor Explained (Visual)

Imagine this list:

```
[10, 20, 30]
```

Initial position:

```
| 10  20  30
^
cursor (before first element)
```

* `hasNext()` → checks availability
* `next()` → returns element and moves cursor

---

## 8️⃣ Iterator Methods Explained Clearly

### 🔹 hasNext()

```java
boolean hasNext();
```

* Returns `true` if elements remain
* Does **not move cursor**

---

### 🔹 next()

```java
E next();
```

* Returns current element
* Advances cursor
* Throws `NoSuchElementException` if no element

---

### 🔹 remove()

```java
void remove();
```

* Removes **last element returned by next()**
* Can be called **only once per next()**
* Safe way to modify collection during iteration

---

## 9️⃣ Why remove() Exists in Iterator

### ❌ Wrong Way (Common Mistake)

```java
for (int n : list) {
    if (n % 2 == 0) {
        list.remove(n); // ❌
    }
}
```

❗ This causes **ConcurrentModificationException**

---

## 🔟 What is ConcurrentModificationException?

* Iterator keeps an internal modification count (`modCount`)
* If collection changes unexpectedly → mismatch detected
* Iterator fails immediately (fail-fast)

---

## 1️⃣1️⃣ Correct Way to Remove Elements (SAFE)

```java
Iterator<Integer> it = list.iterator();

while (it.hasNext()) {
    int n = it.next();
    if (n % 2 == 0) {
        it.remove(); // ✅ safe
    }
}
```

✔ No exception
✔ Iterator stays in sync

---

## 1️⃣2️⃣ Why iterator.remove() Is Safe

| Operation           | Safe? | Reason           |
| ------------------- | ----- | ---------------- |
| `list.remove()`     | ❌ No  | Iterator unaware |
| `iterator.remove()` | ✅ Yes | Updates modCount |

---

## 1️⃣3️⃣ ListIterator (Advanced Iterator)

`ListIterator` is a **special iterator for List**.

### Extra Capabilities

* Traverse **forward & backward**
* Modify elements
* Add elements during iteration

```java
ListIterator<Integer> lit = list.listIterator();
```

### Example

```java
while (lit.hasNext()) {
    if (lit.next() % 2 == 0) {
        lit.set(0); // replace element
    }
}
```

📌 Not possible with normal Iterator.

---

## 1️⃣4️⃣ Iterator Hierarchy

```
Iterable
   ↓
Collection
   ↓
List / Set / Queue
   ↓
Iterator
```

✔ All collections implement Iterable
✔ All provide Iterator

---

## 1️⃣5️⃣ Custom Iterable Class (INTERVIEW GOLD ⭐)

```java
class MyCollection implements Iterable<Integer> {
    private int[] data = {1, 2, 3};

    @Override
    public Iterator<Integer> iterator() {
        return new Iterator<>() {
            int index = 0;

            public boolean hasNext() {
                return index < data.length;
            }

            public Integer next() {
                return data[index++];
            }
        };
    }
}
```

Usage:

```java
for (int x : new MyCollection()) {
    System.out.println(x);
}
```

📌 Shows **deep understanding**.

---

## 1️⃣6️⃣ Fail-Fast vs Fail-Safe (Brief)

| Type      | Behavior                             |
| --------- | ------------------------------------ |
| Fail-Fast | Throws exception on modification     |
| Fail-Safe | Works on snapshot / weak consistency |

Examples:

* Fail-Fast → ArrayList, HashMap
* Fail-Safe → ConcurrentHashMap

---

## 1️⃣7️⃣ Real-World Analogy 🌍

* **Iterable** → “You are allowed to walk through the building”
* **Iterator** → “Here is the map & path to walk”

---

Below is a **clear, deep, interview-ready explanation of Java `Set`**, written in the same **step-by-step teaching style** you liked for Iterable/Iterator.
I’ll explain **what it is, how it works internally, why duplicates are not allowed, each implementation, thread-safety, and interview traps**.

---

# 📘 Java Set Interface — Detailed Explanation

---

## 1️⃣ What is Set in Java?

**Set** is a collection that **stores unique elements only**.

* No duplicates allowed
* Order depends on implementation
* Part of **Java Collection Framework**
* Extends `Collection`, not `List`

```java
public interface Set<E> extends Collection<E>
```

📌 If you try to add a duplicate → it is **ignored**.

---

## 2️⃣ Why Set Does NOT Allow Duplicates

Internally, **Set is backed by a Map**.

| Set           | Backing Map   |
| ------------- | ------------- |
| HashSet       | HashMap       |
| LinkedHashSet | LinkedHashMap |
| TreeSet       | TreeMap       |

### Internal Concept (IMPORTANT)

```java
HashSet<E> {
    private transient HashMap<E, Object> map;
    private static final Object PRESENT = new Object();
}
```

When you do:

```java
set.add("Java");
```

Internally:

```java
map.put("Java", PRESENT);
```

📌 Elements of Set are treated as **keys**
📌 Keys in Map are **always unique** → hence Set uniqueness

---

## 3️⃣ How Set Decides Duplicates (VERY IMPORTANT)

### For HashSet / LinkedHashSet

Uses:

* `hashCode()`
* `equals()`

Duplicate condition:

```text
Same hashCode AND equals() returns true
```

### For TreeSet

Uses:

* `compareTo()` OR
* `Comparator.compare()`

Duplicate condition:

```text
compare() == 0
```

⚠️ **Interview trap**
If `compareTo()` returns `0`, element is treated as duplicate even if `equals()` is false.

---

## 4️⃣ Basic Set Example

```java
Set<Integer> set = new HashSet<>();
set.add(12);
set.add(1);
set.add(67);
set.add(1); // duplicate

System.out.println(set);
```

### Output

```
[1, 12, 67]
```

📌 Duplicate `1` ignored

---

## 5️⃣ Set vs List (Interview MUST)

| Feature      | List         | Set           |
| ------------ | ------------ | ------------- |
| Duplicates   | Allowed      | ❌ Not allowed |
| Index access | Yes          | ❌ No          |
| Ordering     | Preserved    | Depends       |
| Nulls        | Allowed      | Depends       |
| Use case     | Ordered data | Unique data   |

---

## 6️⃣ HashSet (Most Used Set)

### Characteristics

* Unordered
* Fastest
* Backed by `HashMap`
* Allows **one null**

```java
Set<Integer> hashSet = new HashSet<>();
```

### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| add       | O(1) avg   |
| remove    | O(1) avg   |
| contains  | O(1) avg   |

📌 **Best choice when order doesn’t matter**

---

## 7️⃣ LinkedHashSet (Insertion Order)

### Characteristics

* Maintains insertion order
* Slightly slower than HashSet
* Backed by `LinkedHashMap`

```java
Set<Integer> linkedSet = new LinkedHashSet<>();
linkedSet.add(12);
linkedSet.add(1);
linkedSet.add(67);
```

### Output

```
[12, 1, 67]
```

📌 Use when **order matters + no duplicates**

---

## 8️⃣ TreeSet (Sorted Set)

### Characteristics

* Sorted order
* Backed by `TreeMap`
* Uses **Red-Black Tree**
* Does NOT allow null

```java
Set<Integer> treeSet = new TreeSet<>();
treeSet.add(12);
treeSet.add(1);
treeSet.add(67);
```

### Output

```
[1, 12, 67]
```

### Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| add       | O(log n)   |
| remove    | O(log n)   |
| contains  | O(log n)   |

📌 Use when **sorted unique data** is required

---

## 9️⃣ Set Implementations Comparison (Quick Memory)

| Set           | Order     | Speed   | Structure     |
| ------------- | --------- | ------- | ------------- |
| HashSet       | ❌ No      | Fastest | HashMap       |
| LinkedHashSet | Insertion | Fast    | LinkedHashMap |
| TreeSet       | Sorted    | Slower  | TreeMap       |

---

## 🔟 Thread-Safe Set Options

### ❌ HashSet is NOT thread-safe

```java
Set<Integer> set = new HashSet<>();
```

---

### ⚠️ Synchronized Set (Old way)

```java
Set<Integer> syncSet =
    Collections.synchronizedSet(new HashSet<>());
```

* Full locking
* Poor performance

---

### ✅ CopyOnWriteArraySet (Read-heavy)

```java
CopyOnWriteArraySet<Integer> cowSet =
    new CopyOnWriteArraySet<>();
```

✔ Thread-safe
✔ No ConcurrentModificationException
❌ Writes are expensive (copies array)

📌 Best for **many reads, few writes**

---

### ✅ ConcurrentSkipListSet (Sorted + Concurrent)

```java
ConcurrentSkipListSet<Integer> skipSet =
    new ConcurrentSkipListSet<>();
```

✔ Thread-safe
✔ Sorted
✔ High concurrency

📌 Best replacement for TreeSet in multi-threading

---

## 1️⃣1️⃣ CopyOnWriteArraySet vs ConcurrentSkipListSet

| Feature     | CopyOnWriteArraySet | ConcurrentSkipListSet |
| ----------- | ------------------- | --------------------- |
| Thread-safe | Yes                 | Yes                   |
| Sorted      | ❌ No                | ✅ Yes                 |
| Read-heavy  | Best                | OK                    |
| Write-heavy | ❌ Bad               | ✅ Good                |
| Iteration   | Snapshot            | Weakly consistent     |

---

## 1️⃣2️⃣ NavigableSet (TreeSet Power)

```java
NavigableSet<Integer> nav = new TreeSet<>();
nav.add(10);
nav.add(20);
nav.add(30);
```

```java
nav.lower(20);    // 10
nav.floor(20);    // 20
nav.ceiling(25);  // 30
nav.higher(20);   // 30
```

📌 Very common interview topic

---

## 1️⃣3️⃣ Immutable Sets (Java 9+)

```java
Set<Integer> set = Set.of(1, 2, 3);
set.add(4); // ❌ UnsupportedOperationException
```

✔ Thread-safe
✔ Memory efficient
✔ No modification allowed

---

## 1️⃣4️⃣ Real-World Use Cases

| Scenario                | Set Type                   |
| ----------------------- | -------------------------- |
| Unique user IDs         | HashSet                    |
| Ordered logs            | LinkedHashSet              |
| Leaderboard             | TreeSet                    |
| Cache keys (concurrent) | ConcurrentHashMap.keySet() |
| Read-heavy config       | CopyOnWriteArraySet        |

---

## 1️⃣5️⃣ Common Interview Traps ⚠️

❌ Forgetting equals/hashCode in HashSet
❌ compareTo() returning 0 unintentionally in TreeSet
❌ Assuming order in HashSet
❌ Using TreeSet in multithreaded app

---

