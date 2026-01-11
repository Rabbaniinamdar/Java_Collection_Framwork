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





