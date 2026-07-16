# Java Collections Framework — Notes
---

## 📑 Table of Contents

1. [Introduction to Java Collections Framework](#1-introduction-to-java-collections-framework)
2. [Collections vs Arrays](#2-collections-vs-arrays)
3. [Core Interfaces Overview](#3-core-interfaces-overview)
4. [Collection Hierarchy Diagram](#4-collection-hierarchy-diagram)
5. [Iterable, Iterator, ListIterator & Fail-Fast vs Fail-Safe](#5-iterable-iterator-listiterator--fail-fast-vs-fail-safe)
6. [List Interface](#6-list-interface)
   - 6.1 ArrayList — Deep Dive
   - 6.2 LinkedList — Deep Dive
   - 6.3 Vector — Deep Dive
   - 6.4 CopyOnWriteArrayList — Deep Dive
7. [Set Interface](#7-set-interface)
   - 7.1 HashSet
   - 7.2 LinkedHashSet
   - 7.3 TreeSet / NavigableSet
   - 7.4 CopyOnWriteArraySet
   - 7.5 ConcurrentSkipListSet
   - 7.6 EnumSet
8. [Queue & Deque Interface](#8-queue--deque-interface)
   - 8.1 Queue
   - 8.2 Deque / ArrayDeque
   - 8.3 PriorityQueue — Deep Dive
   - 8.4 BlockingQueue Family
9. [Map Interface (Separate Hierarchy)](#9-map-interface-separate-hierarchy)
   - 9.1 HashMap — Deep Dive
   - 9.2 LinkedHashMap — Deep Dive (incl. LRU Cache)
   - 9.3 SortedMap / NavigableMap / TreeMap
   - 9.4 Hashtable
   - 9.5 WeakHashMap
   - 9.6 IdentityHashMap
   - 9.7 EnumMap
   - 9.8 ConcurrentHashMap — Deep Dive
10. [Comparable vs Comparator](#10-comparable-vs-comparator)
11. [Collections Utility Class](#11-collections-utility-class)
12. [Arrays vs Collections](#12-arrays-vs-collections)
13. [Immutable Collections (Java 9+)](#13-immutable-collections-java-9)
14. [Streams Synergy with Collections](#14-streams-synergy-with-collections)
15. [Thread-Safety Cheat Sheet](#15-thread-safety-cheat-sheet)
16. [Real-World Collection Selection Guide](#16-real-world-collection-selection-guide)

---

## 1. Introduction to Java Collections Framework

The **Java Collections Framework (JCF)** is a unified architecture provided by Oracle Corporation under the `java.util` package that helps developers store, manipulate, and process groups of objects efficiently. Instead of manually managing arrays or creating custom data structures, JCF provides ready-made classes like `ArrayList`, `HashSet`, and `HashMap` that solve common problems such as storing dynamic data, ensuring uniqueness, maintaining order, or enabling fast lookups.

At its core, JCF is not just about storing data — it defines **standard interfaces, implementations, and algorithms**. Interfaces like `List`, `Set`, and `Queue` define *what operations are possible*, while classes like `ArrayList` or `HashSet` define *how those operations are implemented internally*. This separation is powerful because you can write flexible code that depends on interfaces rather than specific implementations.

For example, if you write your code using the `List` interface, you can later switch from `ArrayList` to `LinkedList` without changing your business logic. This makes your code more maintainable and scalable.

> **Real-world relevance:** In a Spring Boot service layer, you almost always declare return types and fields as `List<T>`, `Set<T>`, or `Map<K,V>` — never as `ArrayList<T>` or `HashMap<K,V>`. This lets you swap the concrete implementation later (e.g., moving from `HashMap` to `ConcurrentHashMap` when a bean becomes shared across threads) without touching calling code — a direct application of *programming to an interface*.

---

## 2. Collections vs Arrays

Arrays are the most basic way to store multiple values, but they come with limitations. Collections were introduced to overcome these and provide a more flexible approach.

The biggest limitation of arrays is that they have a **fixed size**. Once an array is created, you cannot change its length. In real-world applications like user lists or transaction logs, the amount of data is rarely fixed. Collections solve this by being **dynamic**, meaning they grow and shrink automatically as elements are added or removed.

Another major difference is that arrays can store both primitives and objects, but collections work with objects only. However, Java provides wrapper classes like `Integer` and `Double` to bridge this gap (this is where **autoboxing/unboxing** cost — an important interview and performance topic — comes from: every primitive inserted into a collection is boxed into its wrapper object, which has memory and GC implications in hot paths). Collections also provide **built-in methods** such as sorting, searching, filtering, and iteration, which would otherwise require manual implementation when using arrays.

Collections also improve **code readability and safety**. With features like generics (`List<String>`), you can enforce type safety at compile time, reducing runtime errors.

In real-world applications, collections are almost always preferred because they reduce development time and provide optimized internal implementations.

> **Real-world relevance:** Arrays are still preferred in performance-critical, fixed-size, primitive-heavy code — e.g., a numeric computation module, a matrix library, or JNI interop — precisely because they avoid boxing overhead. Collections are preferred everywhere else: DTOs, repository return types, service-layer aggregation, caching layers.

| Aspect | Array | Collection |
|---|---|---|
| Size | Fixed | Dynamic |
| Stores | Primitives + Objects | Objects only (autoboxed) |
| Type safety | Via compiler | Via Generics |
| Built-in algorithms | None (must write manually or use `Arrays` utility) | Rich (`Collections` utility, Streams) |
| Multi-dimensional | Natively supported | Simulated via nested collections |

---

## 3. Core Interfaces Overview

### 🔹 Collection Interface

The `Collection` interface is the **root interface** of the collection hierarchy. It defines basic operations such as adding, removing, and iterating over elements. It does not specify how elements are stored or ordered — this is left to its child interfaces.

Think of `Collection` as a **general contract** for all data structures that hold multiple elements.

### 🔹 List Interface

The `List` interface represents an **ordered collection** where elements are stored in sequence, and each element has an index. Lists allow duplicate elements and preserve insertion order.

For example, if you are building a feature like a **playlist or user activity log**, where order matters and duplicates are allowed, a `List` is the right choice.

Two popular implementations are:

* `ArrayList`: Fast for reading, slower for insertions in the middle
* `LinkedList`: Faster for insertions/deletions, slower for random access

### 🔹 Set Interface

The `Set` interface represents a collection that **does not allow duplicate elements**. If you try to add a duplicate, it will simply be ignored.

This is useful in scenarios like storing **unique usernames, email IDs, or IDs**, where duplicates are not acceptable.

Common implementations include:

* `HashSet`: No order, very fast
* `LinkedHashSet`: Maintains insertion order
* `TreeSet`: Sorted order

### 🔹 Queue Interface

The `Queue` interface is designed for **processing elements in a specific order**, typically FIFO (First-In-First-Out). It is widely used in scenarios like **task scheduling, messaging systems, or request handling**.

For example, in a print queue, the first document added is the first one printed.

Important implementations include:

* `LinkedList` (can act as a queue)
* `PriorityQueue` (elements processed based on priority instead of order)
* `ArrayDeque` (modern, faster double-ended queue)

### 🔹 Map Interface (Separate Hierarchy)

The `Map` interface is **not part of the `Collection` hierarchy**, but it is still considered part of the JCF. It stores data in **key-value pairs**, where each key is unique.

This is extremely useful when you need **fast lookups**, such as retrieving user details by user ID.

Common implementations include:

* `HashMap`: Fast, no ordering
* `LinkedHashMap`: Maintains insertion order
* `TreeMap`: Sorted by keys

> **Real-world relevance:** In a typical microservice, `List` backs paginated query results, `Set` backs deduplicated role/permission checks, `Queue`/`Deque` backs task schedulers or undo-stacks, and `Map` backs caches, indexes, and configuration lookups. Recognizing which contract a use case needs (ordering? uniqueness? key-based lookup? FIFO/LIFO?) is the first design decision, before even thinking about which concrete class to pick.

---

## 4. Collection Hierarchy Diagram

Understanding the hierarchy is one of the most frequently asked interview topics.

```
Iterable
└── Collection
    ├── List
    │   ├── ArrayList
    │   ├── LinkedList (also implements Deque)
    │   ├── Vector
    │   │   └── Stack
    │   └── CopyOnWriteArrayList
    │
    ├── Set
    │   ├── HashSet
    │   │   └── LinkedHashSet
    │   ├── CopyOnWriteArraySet
    │   └── SortedSet
    │       └── NavigableSet
    │           ├── TreeSet
    │           └── ConcurrentSkipListSet
    │
    └── Queue
        ├── PriorityQueue
        ├── BlockingQueue (interface)
        │   ├── ArrayBlockingQueue
        │   ├── LinkedBlockingQueue
        │   ├── PriorityBlockingQueue
        │   └── SynchronousQueue
        └── Deque
            ├── ArrayDeque
            └── LinkedList


Map (separate hierarchy, does NOT extend Collection)
└── Map
    ├── HashMap
    │   └── LinkedHashMap
    ├── IdentityHashMap
    ├── WeakHashMap
    ├── EnumMap
    ├── Hashtable
    │   └── Properties
    ├── SortedMap
    │   └── NavigableMap
    │       └── TreeMap
    └── ConcurrentMap (interface)
        ├── ConcurrentHashMap
        └── ConcurrentNavigableMap
            └── ConcurrentSkipListMap
```

At the top, you have the `Iterable` interface, which allows objects to be used in a for-each loop. The `Collection` interface extends `Iterable` and acts as the root for `List`, `Set`, and `Queue`.

The `Map` interface is separate and does not extend `Collection` because it deals with key-value pairs instead of individual elements.

Below these interfaces, you have concrete classes like `ArrayList`, `HashSet`, and `HashMap`, which provide actual implementations.

This layered design helps Java achieve:

* **Abstraction** (interfaces define behavior)
* **Flexibility** (multiple implementations)
* **Reusability** (common algorithms like sorting)

### Why Different Collections Exist (Most Important Concept)

This is the **core idea interviewers expect you to understand deeply**.

Different collections exist because **different problems require different data structures**. There is no single structure that can efficiently handle all use cases.

If you use a `List`, you get ordering and indexing, but you allow duplicates. If you use a `Set`, you ensure uniqueness but lose indexing. If you use a `Queue`, you process elements in a controlled order. If you use a `Map`, you optimize for fast key-based retrieval.

Each data structure is optimized for a specific operation:

* `ArrayList` → fast reads
* `LinkedList` → fast insert/delete at ends
* `HashSet` → uniqueness with fast operations
* `HashMap` → constant-time lookup
* `PriorityQueue` → priority-based processing

In real-world systems, choosing the wrong collection can lead to **performance issues, memory overhead, or incorrect behavior**.

For example, using a `List` to check for duplicates repeatedly can be slow (O(n)), whereas a `Set` does it efficiently (O(1)).

So the real reason multiple collections exist is:
👉 **To give developers the right tool for the right job, based on performance, ordering, and uniqueness requirements.**

---

## 5. Iterable, Iterator, ListIterator & Fail-Fast vs Fail-Safe

### 5.1 Why Iterable Exists (The for-each Loop Mystery)

When you write a for-each loop in Java like:

```java
for (String item : list) {
    System.out.println(item);
}
```

it looks simple, almost magical. But Java does not magically know how to move from one element to the next. For this to work, the object on the right side of `:` must implement a special interface called **Iterable**.

The purpose of `Iterable` is very small but extremely powerful. It provides a **contract** that says:
👉 *"This object knows how to give you an Iterator."*

Any class that implements `Iterable` automatically becomes compatible with the for-each loop. That is why all standard collections like `ArrayList`, `LinkedList`, `HashSet`, and others work seamlessly with for-each.

### 5.2 What Iterable Actually Contains

The `Iterable` interface contains only **one method**:

```java
Iterator<T> iterator();
```

That's it.

This method does not do iteration itself. Instead, it returns an **Iterator object**, which is responsible for traversing the elements. So `Iterable` is like a **factory** that hands out iterators.

This design allows Java to separate responsibilities cleanly:

* `Iterable` → *Can I be iterated?*
* `Iterator` → *How do I move through elements safely?*

### 5.3 What an Iterator Really Is (The Cursor Concept)

An `Iterator` is an object that acts like a **cursor** pointing to elements in a collection, one at a time. It knows:

* Whether there is another element
* How to move to the next element
* How to safely remove the current element

The `Iterator` interface exposes three main methods:

```java
boolean hasNext();
E next();
void remove();
```

Internally, the iterator maintains state about *where you are* in the collection. This is why multiple iterators can exist on the same collection at the same time, each walking independently.

### 5.4 How the for-each Loop Works Internally (Desugaring)

The most important concept to understand is that **for-each is just syntactic sugar**. The Java compiler converts it into an explicit iterator loop.

This code:

```java
for (Integer num : numbers) {
    System.out.println(num);
}
```

is internally translated by the compiler into something like this:

```java
Iterator<Integer> it = numbers.iterator();
while (it.hasNext()) {
    Integer num = it.next();
    System.out.println(num);
}
```

This explains everything:

* Why `Iterable` is required
* Why `iterator()` must exist
* Why `hasNext()` and `next()` are used

So whenever you use a for-each loop, **you are already using an Iterator**, whether you realize it or not.

### 5.5 Iterable vs Iterator (Clear Mental Separation)

A very common confusion is mixing up `Iterable` and `Iterator`. The easiest way to remember the difference is this:

* **Iterable** belongs to the collection
* **Iterator** belongs to the traversal

A collection like `ArrayList` implements `Iterable` because it can be iterated. But the actual movement through elements is handled by a **separate Iterator object** created by calling `iterator()`.

This separation allows clean, reusable, and safe iteration logic.

### 5.6 Fail-Fast vs Fail-Safe Iterators (Standalone Deep Dive)

This is one of the **most frequently asked concurrency-adjacent interview topics**, and it is important enough to understand as its own subject, not just as a side effect of `ConcurrentModificationException`.

**Fail-fast iterators** (used by `ArrayList`, `HashMap`, `HashSet`, `LinkedList`, etc.) detect structural modification of the underlying collection during iteration and immediately throw `ConcurrentModificationException`. This detection is implemented using an internal counter called `modCount`. Every structural change (add/remove, not `set()`) increments `modCount`. When the iterator is created, it captures the current `modCount`. Every call to `next()` checks whether `modCount` still matches the captured value — if not, it fails fast rather than allowing unpredictable traversal.

```java
List<Integer> list = new ArrayList<>(List.of(1, 2, 3));
for (Integer i : list) {
    if (i == 2) list.remove(i); // throws ConcurrentModificationException
}
```

Fail-fast behavior is a **best-effort safety net**, not a hard guarantee — the JavaDocs explicitly say it should not be relied upon for correctness, only used to detect bugs.

**Fail-safe iterators** (used by `CopyOnWriteArrayList`, `CopyOnWriteArraySet`, `ConcurrentHashMap`, `ConcurrentSkipListMap`) do not throw `ConcurrentModificationException`. They either iterate over a **snapshot** of the data (as in `CopyOnWriteArrayList`) or provide a **weakly consistent view** (as in `ConcurrentHashMap`) that may or may not reflect concurrent updates, but never fails.

| Aspect | Fail-Fast | Fail-Safe |
|---|---|---|
| Examples | `ArrayList`, `HashMap`, `HashSet` | `CopyOnWriteArrayList`, `ConcurrentHashMap` |
| Throws CME? | Yes | No |
| Works on | Original collection | Snapshot / weakly consistent view |
| Memory overhead | None extra | Extra (copy or internal structure) |
| Use case | Single-threaded / detect bugs early | Multi-threaded, read-heavy |

> **Real-world relevance:** This distinction bites people most often in Spring services iterating over a shared, mutable `List` field while another thread (e.g., a scheduled task or event listener) modifies it — a classic production `ConcurrentModificationException`. The fix is either proper synchronization, switching to `CopyOnWriteArrayList` for read-heavy/rarely-mutated shared state (e.g., a list of registered webhook listeners), or using `ConcurrentHashMap`/`ConcurrentLinkedQueue` for high-throughput shared maps/queues.

### 5.7 Why Direct remove() Fails During Iteration

One of the most common runtime errors beginners face is `ConcurrentModificationException`. This happens when you try to modify a collection directly while iterating over it.

```java
List<Integer> numbers =
    new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

for (Integer n : numbers) {
    if (n % 2 == 0) {
        numbers.remove(n); // ❌ throws exception
    }
}
```

This fails because the iterator internally tracks a modification count. When the collection is modified outside the iterator, the iterator detects this mismatch and throws an exception to prevent unpredictable behavior.

### 5.8 Safe Removal Using Iterator.remove()

The correct and safe way to remove elements during traversal is to use the **Iterator's own `remove()` method**.

```java
List<Integer> numbers =
    new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

Iterator<Integer> it = numbers.iterator();

while (it.hasNext()) {
    int num = it.next();
    if (num % 2 == 0) {
        it.remove(); // ✅ safe
    }
}

System.out.println(numbers); // [1, 3, 5]
```

Here, the iterator knows exactly which element was returned last by `next()`, so it can safely remove it without corrupting the traversal state — because `Iterator.remove()` updates `modCount` on both the iterator and the collection in sync.

This is the **only correct way** to remove elements while iterating over most collections (the modern alternative for simple predicate-based removal is `Collection.removeIf()`, which does the same thing internally but with cleaner syntax).

### 5.9 Enter ListIterator: Power for Lists

`ListIterator` is a more powerful iterator designed specifically for **lists**. While `Iterator` only moves forward, `ListIterator` can move **both forward and backward**.

It also supports:

* Replacing elements (`set()`)
* Adding elements during iteration (`add()`)
* Index awareness (`nextIndex()`, `previousIndex()`)

```java
ListIterator<Integer> lit = numbers.listIterator();
```

Once you have a `ListIterator`, you can traverse in both directions.

### 5.10 Modifying Elements with ListIterator

Unlike `Iterator`, `ListIterator` allows modifying elements during iteration without exceptions.

```java
List<Integer> numbers =
    new ArrayList<>(Arrays.asList(1, 2, 3));

ListIterator<Integer> lit = numbers.listIterator();

while (lit.hasNext()) {
    if (lit.next() == 1) {
        lit.set(10); // Replace current element
    }
}

System.out.println(numbers); // [10, 2, 3]
```

This ability makes `ListIterator` ideal when you need **in-place updates** while traversing a list.

### 5.11 Reverse Traversal with ListIterator

Because lists are ordered, `ListIterator` allows reverse traversal as well.

```java
while (lit.hasPrevious()) {
    System.out.print(lit.previous() + " ");
}
```

This is especially useful for algorithms that need backward movement, something impossible with a plain `Iterator`.

### 5.12 Final Mental Model

* **Iterable** → *"I can give you an iterator"*
* **Iterator** → *"I move forward and can safely remove"*
* **ListIterator** → *"I move forward, backward, and can modify"*
* **for-each loop** → *Compiler-generated Iterator code*

---

## 6. List Interface

The `List` interface represents an **ordered collection** where duplicates are allowed and every element has a positional index. It extends `Collection` and adds index-based operations (`get(int)`, `set(int, E)`, `indexOf()`, `subList()`, etc.).

> **Real-world relevance:** Lists are the default return type for anything paginated or sequential — repository `findAll()` results, API response arrays, audit trails, ordered form-field lists, playlists, timelines.

### 6.1 ArrayList — Deep Dive

#### 6.1.1 What ArrayList Really Is

`ArrayList` (from `java.util.ArrayList`) is one of the most commonly used data structures in Java, and it represents a **dynamic, resizable array**. Unlike traditional arrays where the size must be fixed at the time of creation, an `ArrayList` automatically grows when more elements are added beyond its current capacity.

This dynamic behavior makes it extremely flexible for real-world applications where the number of elements is not known in advance. Internally, however, it still behaves like an array, meaning elements are stored in a **contiguous block of memory**, which allows very fast access using indexes.

The most important idea to understand is that `ArrayList` combines:

* The **speed of arrays (for access)**
* The **flexibility of dynamic resizing**

However, this combination also introduces trade-offs, especially when elements need to be inserted or removed from the middle.

#### 6.1.2 Internal Working & Memory Structure

At its core, `ArrayList` is implemented using:

```java
transient Object[] elementData;
private int size;
```

The `elementData` array stores all elements, while `size` keeps track of how many elements are actually present.

To understand this clearly, consider the internal structure:

```text
Index:   0    1    2    3    4
        -------------------------
Data:   [A]  [B]  [C]  [ ]  [ ]
Size = 3
Capacity = 5
```

Here, the array has space for 5 elements (capacity), but only 3 elements are stored (size). This separation between size and capacity is crucial for understanding how resizing works.

#### 6.1.3 Resizing Mechanism (Most Important Internal Behavior)

When the internal array becomes full and a new element is added, `ArrayList` does not simply expand the existing array. Instead, it performs a multi-step process:

```text
Step 1 → Create a new larger array
Step 2 → Copy all existing elements
Step 3 → Replace old array reference
```

The new capacity is calculated using:

```text
newCapacity = oldCapacity + (oldCapacity / 2)
```

This means the array grows by **1.5x (50% increase)**.

Example growth:

```text
10 → 15 → 22 → 33 → ...
```

This strategy ensures:

* Fewer resizing operations
* Balanced memory usage

Although resizing is expensive (O(n)), it happens infrequently, which is why adding elements at the end is considered **amortized O(1)**.

#### 6.1.4 Why ArrayList is Fast for Access

One of the biggest strengths of `ArrayList` is its ability to access elements quickly using an index.

When you call:

```java
list.get(2);
```

Java calculates the memory location directly:

```text
Address = base + (index × sizeOfElement)
```

This means there is **no traversal involved**, unlike `LinkedList`.

👉 Result: **O(1) time complexity**

This makes `ArrayList` extremely efficient for:

* Iteration
* Searching by index
* Read-heavy operations

#### 6.1.5 Insertions and Deletions (Where Cost Comes From)

The main drawback of `ArrayList` comes from how it handles insertions and deletions.

When inserting in the middle:

```java
list.add(1, "X");
```

Internally:

```text
Before:
[A][B][C]

After:
[A][X][B][C]
```

To make space, all elements after index 1 must be shifted to the right.

Similarly, when removing:

```java
list.remove(1);
```

Elements shift left to fill the gap.

👉 This shifting operation is the reason:

* Insert (middle) = **O(n)**
* Delete (middle) = **O(n)**
* Insert/Delete at end = **O(1) amortized**

So the real cost of `ArrayList` is not resizing — it is **element shifting**.

#### 6.1.6 Capacity Management (Performance Optimization)

In real-world systems, repeated resizing can become expensive. To avoid this, Java allows you to define an initial capacity:

```java
List<Integer> list = new ArrayList<>(1000);
```

By doing this, you:

* Reduce memory reallocations
* Avoid repeated copying
* Improve performance

This is especially useful in:

* Large data processing (e.g., batch ETL jobs reading a known row-count from a DB cursor)
* Batch operations (e.g., building a request payload for a bulk API call)
* High-performance systems

#### 6.1.7 Thread Safety

`ArrayList` is **not thread-safe**, meaning multiple threads modifying it at the same time can cause unpredictable behavior.

Problems include:

* Data corruption
* Incorrect size tracking
* Runtime exceptions

To make it thread-safe:

```java
List<Integer> syncList = Collections.synchronizedList(new ArrayList<>());
```

This ensures only one thread can modify the list at a time. However, this introduces locking overhead, and you must still manually synchronize when iterating (`synchronized(syncList) { for (...) }`), otherwise you can still get a `ConcurrentModificationException`.

Modern alternative for read-heavy concurrent scenarios:

```java
CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<>();
```

This avoids locking during reads and is more efficient for read-heavy scenarios (see section 6.4).

#### 6.1.8 Advanced Operations

**Sorting**

```java
list.sort(Comparator.naturalOrder());
```

This sorts elements in ascending order.

**Replacing Elements**

```java
list.replaceAll(e -> e * 2);
```

This applies a transformation to every element.

**SubList (Very Important Concept)**

```java
List<Integer> sub = list.subList(1, 3);
```

This does NOT create a new list. Instead, it creates a **view** of the original list.

```text
Original: [10, 20, 30, 40]
SubList:        [20, 30]
```

If you modify the sublist:

```text
sub.set(0, 99)

Original becomes:
[10, 99, 30, 40]
```

👉 This behavior is critical and often misunderstood, and it is a common source of subtle production bugs — e.g., calling `list.subList(a, b).clear()` to bulk-delete a range actually mutates the *backing* list. Also note: any structural modification made *directly* to the backing list (not through the sublist view) invalidates the sublist and will throw `ConcurrentModificationException` on next use.

**Functional Operations**

```java
list.removeIf(x -> x % 2 == 0);
list.stream().forEach(System.out::println);
```

These operations show how `ArrayList` works with modern Java functional programming.

#### 6.1.9 Complete Code Example

```java
import java.util.*;

public class ArrayListDeepExample {
    public static void main(String[] args) {

        List<Integer> list = new ArrayList<>(5);

        list.add(10);
        list.add(20);
        list.add(30);

        list.add(1, 15);

        list.sort(Comparator.naturalOrder());

        list.replaceAll(e -> e * 2);

        List<Integer> sub = list.subList(1, 3);

        sub.set(0, 999);

        list.removeIf(x -> x > 100);

        list.stream().forEach(System.out::println);
    }
}
```

In this program, the list starts as a simple dynamic array, but gradually demonstrates insertion, sorting, transformation, sublist behavior, filtering, and streaming. The most important part to observe is how modifying the sublist directly affects the original list, proving that it is not a copy but a view.

#### 6.1.10 Deep Insight — Why ArrayList is Used Everywhere

`ArrayList` is the default choice in most applications because it aligns well with modern system behavior:

* CPU cache favors contiguous memory
* Most applications are read-heavy
* Memory usage is compact
* Iteration is fast

> **Real-world relevance:** `ArrayList` is the default backing structure for JPA/Hibernate `@OneToMany` collections when eagerly fetched, JSON deserialization targets (Jackson maps JSON arrays to `ArrayList` by default), and REST response bodies. Pre-sizing with `new ArrayList<>(expectedSize)` is a common micro-optimization in high-throughput batch jobs (e.g., building a list of 100k records from a DB cursor) to avoid repeated resize/copy cycles.

---

### 6.2 LinkedList — Deep Dive

#### 6.2.1 What LinkedList Really Is

`LinkedList` in Java is a class that implements both the `List` and `Deque` interfaces, which means it is not limited to behaving like a simple list. It can act as an ordered collection where duplicates are allowed, but at the same time it can also function as a queue (First-In-First-Out) or a stack (Last-In-First-Out). This multi-purpose nature makes it conceptually different from `ArrayList`, which is purely designed for indexed access.

The most important difference lies in how data is stored internally. Unlike `ArrayList`, which keeps elements in a continuous block of memory, `LinkedList` stores each element in a **separate object called a node**, and these nodes are connected to each other through references. This means elements are not stored next to each other in memory, but rather linked together like a chain.

Because of this design, `LinkedList` does not depend on fixed positions or indexes for storage. Instead of shifting elements like arrays do, it simply changes the connections between nodes. This is the fundamental reason why `LinkedList` performs very well when frequent insertions and deletions are required — at the ends.

#### 6.2.2 Internal Node Structure

The entire behavior of `LinkedList` revolves around its internal node structure. Each element is wrapped inside a node that looks like this:

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}
```

Each node contains the actual data along with two references: one pointing to the next node and one pointing to the previous node. This is why it is called a **doubly linked list**.

To visualize this, imagine the structure as:

```text
null <- [A] <-> [B] <-> [C] -> null
```

Here, each node knows both its previous and next neighbor. Java also internally maintains references to the first node (head) and the last node (tail), along with a size variable that keeps track of the number of elements.

Because the head and tail are directly accessible, operations that involve adding or removing elements at the beginning or end become extremely efficient, as there is no need to traverse the entire structure.

#### 6.2.3 Why LinkedList Cannot Do Fast Random Access

One of the most critical concepts to understand is why `LinkedList` is slow for index-based access. Even though it implements the `List` interface and provides methods like `get(index)`, it does not behave like an array internally.

When you call:

```java
list.get(4);
```

The list does not jump directly to index 4. Instead, it performs a traversal:

```text
Step 1 → Start from head or tail (whichever is closer)
Step 2 → Move node by node
Step 3 → Stop when the index is reached
```

This traversal process takes time proportional to the number of elements, resulting in **O(n)** time complexity.

This is the biggest conceptual trap for beginners. Just because `LinkedList` supports indexing does not mean it is efficient at it. Internally, it is still a chain of nodes, not an indexed array.

#### 6.2.4 Efficient Insertions & Deletions (Main Strength)

The real strength of `LinkedList` becomes visible when elements need to be inserted or removed at the **head or tail**. Since elements are not stored in contiguous memory, there is no need to shift elements like in `ArrayList`.

For example, when adding an element at the beginning:

```java
list.addFirst("X");
```

The structure changes like this:

```text
Before:
[A] <-> [B]

After:
[X] <-> [A] <-> [B]
```

Only the references are updated, and no existing elements are moved. This makes the operation extremely efficient, with a time complexity of **O(1)**.

The same applies to removing elements from the beginning or end:

```java
list.removeFirst();
list.removeLast();
```

Again, only pointer adjustments are required, making these operations constant time.

#### 6.2.5 Insertions & Deletions in the Middle

When inserting or removing elements in the middle, the process involves two steps. First, the list must traverse to the desired position, which takes **O(n)** time. Once the correct node is reached, the actual insertion or deletion is done by updating pointers, which takes **O(1)** time.

So the overall complexity becomes **O(n)**.

Even though this is the same complexity as `ArrayList`, the underlying cost is different. `ArrayList` spends time shifting elements, while `LinkedList` spends time traversing nodes. This difference becomes important depending on how frequently modifications are performed.

#### 6.2.6 Using LinkedList as Queue or Deque

One of the most powerful features of `LinkedList` is that it implements the `Deque` interface, allowing it to function as both a queue and a stack.

For example, using it as a queue:

```java
LinkedList<String> queue = new LinkedList<>();

queue.offer("A");
queue.offer("B");

System.out.println(queue.poll()); // A
System.out.println(queue.peek()); // B
```

These methods make the code safer and more expressive compared to manually handling indexes.

This capability makes `LinkedList` very useful in scenarios like task scheduling, message processing systems, or pipelines where elements need to be processed in a specific order.

#### 6.2.7 Iteration & Bidirectional Traversal

Because `LinkedList` is doubly linked, it supports traversal in both directions. This is achieved using `ListIterator`.

```java
ListIterator<String> it = list.listIterator();

while(it.hasNext()) {
    System.out.println(it.next());
}
```

You can also move backward:

```java
while(it.hasPrevious()) {
    System.out.println(it.previous());
}
```

This bidirectional traversal is not possible in a simple singly linked list and is one of the advantages of the doubly linked structure used in Java.

#### 6.2.8 Memory Cost (Important Trade-off)

While `LinkedList` avoids the overhead of resizing and shifting, it introduces a different kind of cost: memory overhead.

Each element is stored as a node containing:

```text
[Data | Prev | Next]
```

This means that for every element, extra memory is required to store two references. In large datasets, this overhead becomes significant.

In contrast, `ArrayList` stores elements compactly:

```text
[A][B][C][D]
```

Because of this, `LinkedList` is less cache-friendly and generally consumes more memory.

#### 6.2.9 When to Use LinkedList

`LinkedList` should be chosen when the application involves frequent insertions and deletions, especially at the beginning or end of the list. It is also a good choice when the data structure needs to behave like a queue or deque.

However, it should be avoided in scenarios where fast access using indexes is required or where memory efficiency is important. In most real-world applications, data access patterns are read-heavy, which makes `ArrayList` a better choice.

#### 6.2.10 Thread Safety

Like `ArrayList`, `LinkedList` is not thread-safe. If multiple threads modify the list at the same time, it can lead to inconsistent data or runtime exceptions. To use it safely in a multi-threaded environment, synchronization must be handled externally, or alternative concurrent data structures (e.g., `ConcurrentLinkedDeque`) should be used depending on the use case.

#### 6.2.11 Deep Comparison — ArrayList vs LinkedList

```text
ArrayList  → Dynamic Array → Fast Access (O(1)) → Slow Middle-Modification (O(n))
LinkedList → Node Chain    → Slow Access (O(n)) → Fast End-Modification (O(1))
```

However, the real-world decision is not just about complexity, but about actual usage patterns.

#### 6.2.12 Real-World Insight — Why LinkedList is Rarely Used in Production

Even though `LinkedList` appears powerful due to its flexibility, it is rarely used in production systems. The main reasons are poor cache performance, higher memory usage, and slower traversal compared to array-based structures.

Modern applications tend to favor `ArrayList` because most operations involve reading and iterating over data rather than constantly modifying it. In fact, in most cases where you'd reach for `LinkedList` as a queue/deque, `ArrayDeque` (section 8.2) is a strictly better choice — it's array-backed, more cache-friendly, and avoids per-node object overhead.

Understanding `LinkedList` is important not because you will use it frequently, but because it teaches you how different data structures trade performance for flexibility.

> **Real-world relevance:** `LinkedList` shows up in interview whiteboarding far more than in production code. In real systems, `ArrayDeque` replaces it as a stack/queue, and `ArrayList`/`ConcurrentLinkedQueue` replace it elsewhere. One legitimate production use: implementing an LRU-adjacent doubly-linked structure manually (though `LinkedHashMap` — section 9.2 — already does this for you).

---

### 6.3 Vector — Deep Dive

#### 6.3.1 What Vector Really Is

`Vector` is one of the earliest data structures introduced in Java, long before the modern Java Collections Framework even existed. Because of this, it is often referred to as a **legacy class**, meaning it belongs to an older design philosophy of Java. Later, when Java introduced the `List` interface, `Vector` was adapted to implement it, so today it behaves like other list implementations such as `ArrayList`, but its internal design decisions still reflect its original purpose.

At a fundamental level, `Vector` is a **dynamic array**, which means it stores elements in a continuous block of memory just like a normal array but can grow automatically when it becomes full. This allows it to support index-based access, maintain insertion order, and allow duplicate elements and null values. However, the defining characteristic that makes `Vector` different from `ArrayList` is that **every method in Vector is synchronized**, making it thread-safe by default.

This built-in synchronization was extremely useful in early Java applications where multi-threading was common but developer control over concurrency was limited. However, as Java evolved, this design choice became less efficient compared to more flexible modern approaches.

#### 6.3.2 Why Vector is Synchronized (And What That Really Means)

Synchronization in Java means that only one thread can execute a method at a time on a particular object. In `Vector`, methods such as `add()`, `remove()`, and even `get()` are internally synchronized, meaning they automatically acquire a lock before executing and release it afterward.

To understand this behavior, imagine two threads trying to add elements to the same list simultaneously. In a non-synchronized structure like `ArrayList`, both threads might attempt to modify the internal array at the same time, which can lead to inconsistent data, incorrect size tracking, or even runtime exceptions. `Vector` prevents this problem by ensuring that one thread completes its operation fully before another thread can begin.

Conceptually, the execution looks like this:

```text
Thread 1 → lock acquired → operation → lock released
Thread 2 → waits → lock acquired → operation
```

While this guarantees safety, it introduces overhead. Even in situations where only one thread is accessing the `Vector`, the locking mechanism still executes, which unnecessarily slows down performance. This is the main reason why `Vector` is rarely used in modern applications.

#### 6.3.3 Internal Structure: How Vector Stores Data

Internally, `Vector` is built on an array, similar to `ArrayList`. It maintains two key properties: **size** and **capacity**.

* The **size** represents the number of elements currently stored
* The **capacity** represents the total allocated space in memory

ASCII representation:

```text
Index:   0    1    2    3    4
        -------------------------
Data:   [A]  [B]  [C]  [ ]  [ ]
Size = 3
Capacity = 5
```

Unlike `ArrayList`, `Vector` exposes its capacity through the `capacity()` method, allowing developers to inspect how memory is being managed internally.

#### 6.3.4 Constructors and Capacity Growth Behavior

When you create a `Vector`, you can control both the **initial capacity** and how it **grows when full**. By default, `Vector` follows a **doubling strategy**, meaning it increases its capacity to twice its current size — unlike `ArrayList`'s 1.5x growth.

```java
Vector<Integer> v1 = new Vector<>();
```

This creates a `Vector` with a default capacity of **10**. When this capacity is exceeded, the internal array **doubles in size**:

```text
Capacity Growth: 10 → 20 → 40 → 80 → ...
```

```java
Vector<Integer> v2 = new Vector<>(5);
```

Here, the initial capacity is 5. Once those 5 slots are filled, the capacity doubles to 10, then 20, and so on.

`Vector` also provides a unique feature that `ArrayList` does not: the ability to control growth using a **capacity increment**.

```java
Vector<Integer> v3 = new Vector<>(5, 3);
```

In this case, instead of doubling, the capacity increases by a fixed value:

```text
5 → 8 → 11 → 14 → ...
```

This gives developers fine control over memory allocation, although it may result in more frequent resizing operations.

```java
Vector<Integer> v4 = new Vector<>(Arrays.asList(1, 2, 3));
```

This constructor creates a `Vector` from an existing collection, copying its elements while preserving order.

#### 6.3.5 size() vs capacity(): A Common Interview Trap

```java
Vector<Integer> v = new Vector<>(5, 3);

v.add(1); v.add(2); v.add(3);
v.add(4); v.add(5); v.add(6);
```

After inserting six elements:

```text
size() = 6       → number of actual elements
capacity() = 8   → allocated memory after resizing (5 → 8 via increment of 3)
```

This clearly shows that **capacity is about memory allocation**, while **size is about actual data**. Understanding this distinction is important when dealing with performance tuning and memory optimization.

#### 6.3.6 Core Operations and Their Behavior

Since `Vector` is backed by an array, its operations behave similarly to `ArrayList`, but with additional synchronization overhead.

When accessing an element using `get(index)`, the operation is extremely fast because the array allows direct memory access — constant time complexity.

However, when inserting or removing elements in the middle, all subsequent elements must be shifted:

```text
Before:
[A][B][C]

Insert at index 1:
[A][X][B][C]
```

This shifting operation takes linear time and becomes expensive for large datasets. The same applies to deletions, where elements are shifted to fill the gap.

```java
Vector<String> v = new Vector<>();
v.add("Apple");
v.add("Banana");
v.add("Orange");

System.out.println(v.get(1)); // Banana
```

#### 6.3.7 Legacy Methods: Why Vector Feels Outdated

Because `Vector` was designed before modern Java practices, it includes older methods that are rarely used today.

```java
v.addElement("A");
v.removeElement("B");

Enumeration<String> e = v.elements();
```

The `Enumeration` interface is an older alternative to `Iterator`. It lacks features like element removal during iteration and is generally considered outdated. These methods still exist only for backward compatibility with legacy systems.

#### 6.3.8 Iteration and Order Preservation

`Vector` maintains insertion order, meaning elements are retrieved in the same sequence in which they were added. This makes iteration predictable.

```java
for (String item : v) {
    System.out.println(item);
}
```

However, even iteration involves synchronization internally, which adds overhead compared to non-synchronized collections.

#### 6.3.9 Thread Safety vs Performance (Real-World Scenario)

Consider a scenario where two threads are adding elements simultaneously. With `ArrayList`, the lack of synchronization can result in incorrect final size or data inconsistency (you often end up with a final size less than expected, because both threads overwrite internal state during resizing). With `Vector`, the result is always correct — the final size is always exactly as expected — because every operation is protected by synchronization.

However, this reliability comes at the cost of performance. In high-throughput applications, the constant locking mechanism becomes a bottleneck, making `Vector` significantly slower, since every operation involves acquiring and releasing a lock even when there is no real concurrency:

```text
Operation → Lock → Execute → Unlock
```

This repeated locking introduces latency and reduces overall throughput, which is why developers avoid using `Vector` in modern, performance-sensitive systems.

#### 6.3.10 Modern Alternatives

If you need synchronization:

```java
List<Integer> list = Collections.synchronizedList(new ArrayList<>());
```

This gives more control (you choose when to synchronize) than the blanket synchronization baked into `Vector`.

For read-heavy applications:

```java
CopyOnWriteArrayList<Integer> list = new CopyOnWriteArrayList<>();
```

This avoids locking during reads and provides better scalability (see 6.4).

#### 6.3.11 Why Vector Still Exists

`Vector` is not used in modern application development because better alternatives exist. However, it still remains part of Java for:

* Backward compatibility with legacy systems
* Historical reasons
* Interview discussions

Understanding `Vector` is important not because you will use it frequently, but because it helps you understand how Java evolved in handling concurrency and data structures.

> **Real-world relevance:** You will almost never write `new Vector<>()` in new production code — but you will encounter it maintaining legacy enterprise codebases (pre-Java 5 systems, old Swing UI code, or `java.sql` APIs like `ResultSetMetaData` internals). Knowing it exists and why it's slow helps you justify a refactor PR that replaces it with `ArrayList` + explicit synchronization or `CopyOnWriteArrayList`.

---

### 6.4 CopyOnWriteArrayList — Deep Dive

#### 6.4.1 Why CopyOnWriteArrayList Exists

In real-world applications, especially in multi-threaded environments, a very common pattern appears where data is read far more frequently than it is modified. For example, consider configuration settings, application rules, event listeners, or cached values. These structures may be accessed thousands of times per second by multiple threads, but updates to them happen only occasionally.

Using a normal `ArrayList` in such scenarios is risky because it is not thread-safe. If one thread modifies the list while another thread is iterating over it, Java throws a `ConcurrentModificationException` to prevent inconsistent behavior. On the other hand, `Vector` solves this by synchronizing every operation, but that introduces heavy locking, making even simple read operations slow.

To solve this exact problem, Java introduced `CopyOnWriteArrayList`. Its primary goal is to provide **safe, lock-free reads in a concurrent environment**, even when occasional modifications happen. It is designed specifically for scenarios where **reads dominate writes**, making it a specialized but extremely powerful data structure.

#### 6.4.2 The Core Idea: "Copy On Write" Explained

The name `CopyOnWriteArrayList` directly reflects its behavior. Instead of modifying the same underlying array that other threads might be reading, it creates a completely new copy of the array whenever a write operation occurs.

To understand this intuitively, imagine a shared notepad in an office. Multiple people are reading from it at the same time. If someone wants to make a change, they do not edit the original page directly. Instead, they create a photocopy of the page, make their changes on the copy, and then replace the original with the updated version. Meanwhile, all the people who were already reading continue using the old page without interruption.

This approach ensures that readers always see a stable and consistent view of the data, while writers can safely make changes without interfering with ongoing operations.

#### 6.4.3 Internal Working During Reads (Why It Is Extremely Fast)

When a thread reads from a `CopyOnWriteArrayList`, no locks are involved. Internally, the list maintains a reference to an immutable array, and all read operations simply access that array directly.

Because there is no synchronization overhead, reads are extremely fast and scalable — often faster than even `Vector`, because there is no locking at all. Thousands of threads can read from the list simultaneously without blocking each other.

Another important aspect is that iterators operate on a **snapshot** of the array. This means that once an iterator is created, it works on a fixed version of the data that will never change during iteration, even if other threads modify the list. This guarantees consistency and eliminates the risk of concurrent modification issues.

#### 6.4.4 Internal Working During Writes (The Expensive Operation)

Write operations in `CopyOnWriteArrayList` are fundamentally different and more expensive compared to reads. When a method like `add()`, `remove()`, or `set()` is called, the following steps occur internally:

1. The list acquires a lock to ensure that only one thread performs the write operation at a time.
2. It creates a completely new copy of the internal array.
3. The modification is applied to this new array.
4. The internal reference is atomically updated to point to the new array.

```text
Old Array: [A][B][C]

Write Operation → Copy → Modify

New Array: [A][B][C][D]
```

The old array remains untouched and continues to be used by any threads that were already reading it. New readers will see the updated array.

Because the entire array is copied, the time complexity of write operations is **O(n)**, which makes this structure inefficient for frequent updates.

#### 6.4.5 Why Iterators Are Fail-Safe (No Exceptions Ever)

In a normal `ArrayList`, iterators are fail-fast. If the structure of the list changes during iteration, Java throws a `ConcurrentModificationException` to prevent unpredictable behavior.

In `CopyOnWriteArrayList`, iterators behave differently. They are **fail-safe**, meaning they never throw such exceptions. This is because they operate on a snapshot of the array taken at the time the iterator was created.

```java
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.*;

public class Example {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list =
            new CopyOnWriteArrayList<>(Arrays.asList("milk", "eggs", "bread"));

        for (String item : list) {
            if (item.equals("eggs")) {
                list.add("butter");
            }
        }

        System.out.println(list);
    }
}
```

In this example, the loop iterates over the original elements, and the newly added `"butter"` does not appear during iteration. However, after the loop completes, the list contains the new element. This behavior is intentional and ensures safe iteration.

#### 6.4.6 Multithreading Behavior (Real-World Reliability)

One of the biggest advantages of `CopyOnWriteArrayList` is how naturally it handles concurrent access without requiring complex synchronization logic.

```java
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.*;

public class MultiThreadExample {
    public static void main(String[] args) {

        CopyOnWriteArrayList<Integer> list =
            new CopyOnWriteArrayList<>(Arrays.asList(1, 2, 3));

        new Thread(() -> {
            for (int i : list) {
                System.out.println(i);
            }
        }).start();

        new Thread(() -> {
            list.add(4);
            list.remove(Integer.valueOf(1));
        }).start();
    }
}
```

In this example, one thread iterates over the list while another modifies it. The iteration proceeds without interruption, and no exception is thrown. The reading thread sees a consistent snapshot, while the writing thread safely updates the list. This makes the class extremely reliable and easy to use in concurrent applications.

#### 6.4.7 Comparison with ArrayList and Vector

Compared to `ArrayList`, `CopyOnWriteArrayList` sacrifices write performance in exchange for thread safety and consistent iteration. While `ArrayList` is faster for both reads and writes in single-threaded environments, it cannot handle concurrent modifications safely.

Compared to `Vector`, `CopyOnWriteArrayList` provides better scalability because it avoids locking during reads. `Vector` synchronizes every operation, including reads, which creates unnecessary contention and reduces performance in multi-threaded environments.

The key difference lies in the design approach: `Vector` uses locking, while `CopyOnWriteArrayList` uses immutability and snapshots.

#### 6.4.8 Memory Cost and Trade-offs

Because every write operation creates a new copy of the array, `CopyOnWriteArrayList` has a higher memory overhead compared to other list implementations. Each modification temporarily doubles memory usage until the old array is garbage collected.

In scenarios where writes are frequent or the list is very large, this can lead to increased memory pressure and frequent garbage collection cycles, negatively impacting performance.

This is why it is not suitable for:

* Write-heavy applications
* Large datasets
* Streaming or real-time processing systems
* Producer–consumer patterns (use `ConcurrentLinkedQueue` / `BlockingQueue` instead)

#### 6.4.9 Where CopyOnWriteArrayList is Used in Real Life

Despite its limitations, `CopyOnWriteArrayList` is extremely useful in specific scenarios where reads dominate writes. It is commonly used in:

* Event listener management systems
* Application configuration storage
* Security rule engines
* Observer patterns
* Caching mechanisms (small, rarely-changing caches)

In these use cases, the cost of copying is negligible compared to the benefit of fast, safe, and lock-free reads.

> **Real-world relevance:** A classic real production use case is a Spring `ApplicationListener`/observer registry, or an in-memory feature-flag cache refreshed every few minutes by a scheduled job while thousands of request threads read it concurrently. Another is maintaining a small in-memory allow-list/deny-list of IPs or tenant IDs that's updated rarely (via an admin API) but checked on every single incoming request.

#### 6.4.10 Deep Understanding

The real strength of `CopyOnWriteArrayList` lies in its design philosophy. Instead of trying to control concurrency using locks, it avoids the problem entirely by ensuring that readers never see a changing structure.

This makes it one of the simplest and safest ways to handle concurrency in read-heavy applications. However, this simplicity comes at the cost of expensive write operations, which is why it should be used carefully based on the access pattern of the application. Understanding this trade-off is what separates a beginner from someone who truly understands concurrent data structures in Java.

---

## 7. Set Interface

### 7.1 What is Set in Java?

**Set** is a collection that **stores unique elements only**.

* No duplicates allowed
* Order depends on implementation
* Part of the **Java Collection Framework**
* Extends `Collection`, not `List`

```java
public interface Set<E> extends Collection<E>
```

📌 If you try to add a duplicate → it is **ignored**.

### 7.2 Why Set Does NOT Allow Duplicates

Internally, **Set is backed by a Map**.

| Set | Backing Map |
|---|---|
| HashSet | HashMap |
| LinkedHashSet | LinkedHashMap |
| TreeSet | TreeMap |

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

📌 Elements of Set are treated as **keys**.
📌 Keys in Map are **always unique** → hence Set uniqueness.

### 7.3 How Set Decides Duplicates

**For HashSet / LinkedHashSet** — uses `hashCode()` and `equals()`. Duplicate condition: `Same hashCode AND equals() returns true`.

**For TreeSet** — uses `compareTo()` OR `Comparator.compare()`. Duplicate condition: `compare() == 0`.

⚠️ **Interview trap:** If `compareTo()` returns `0`, the element is treated as a duplicate even if `equals()` is false. This is a very common bug source when a `Comparator` used for sorting is reused for a `TreeSet`/`TreeMap` without realizing that "equal by comparator" silently drops entries.

```java
Set<Integer> set = new HashSet<>();
set.add(12);
set.add(1);
set.add(67);
set.add(1); // duplicate, ignored

System.out.println(set); // [1, 12, 67] (order not guaranteed for HashSet)
```

### 7.4 Set vs List

| Feature | List | Set |
|---|---|---|
| Duplicates | Allowed | ❌ Not allowed |
| Index access | Yes | ❌ No |
| Ordering | Preserved | Depends on impl |
| Nulls | Allowed | Depends on impl |
| Use case | Ordered data | Unique data |

### 7.5 HashSet (Most Used Set)

**Characteristics:** Unordered, fastest, backed by `HashMap`, allows one `null`.

```java
Set<Integer> hashSet = new HashSet<>();
```

**Time Complexity:** `add`, `remove`, `contains` → O(1) average (degrades to O(log n) worst case in Java 8+ due to treeification of heavily-collided buckets, same as `HashMap`).

📌 **Best choice when order doesn't matter.**

> **Real-world relevance:** Deduplicating a batch of IDs before a bulk DB `IN` query, checking role/permission membership (`if (userRoles.contains("ADMIN"))`), tracking "already processed" message IDs in an idempotent consumer.

### 7.6 LinkedHashSet (Insertion Order)

**Characteristics:** Maintains insertion order, slightly slower than `HashSet`, backed by `LinkedHashMap`.

```java
Set<Integer> linkedSet = new LinkedHashSet<>();
linkedSet.add(12);
linkedSet.add(1);
linkedSet.add(67);
```

Output: `[12, 1, 67]`

📌 Use when **order matters + no duplicates** — e.g., preserving the order in which unique tags were first encountered while parsing a document.

### 7.7 TreeSet / NavigableSet (Sorted Set)

**Characteristics:** Sorted order, backed by `TreeMap`, uses a **Red-Black Tree**, does NOT allow `null` (throws `NullPointerException` when comparing against existing elements).

```java
Set<Integer> treeSet = new TreeSet<>();
treeSet.add(12);
treeSet.add(1);
treeSet.add(67);
```

Output: `[1, 12, 67]`

**Time Complexity:** `add`, `remove`, `contains` → O(log n).

📌 Use when **sorted unique data** is required — e.g., a leaderboard where you need both uniqueness and continuous sorted order, or range queries.

**NavigableSet (TreeSet's Power) — additional navigation methods:**

```java
NavigableSet<Integer> nav = new TreeSet<>();
nav.add(10);
nav.add(20);
nav.add(30);

nav.lower(20);    // 10  (strictly less than)
nav.floor(20);    // 20  (less than or equal)
nav.ceiling(25);  // 30  (greater than or equal)
nav.higher(20);   // 30  (strictly greater than)
```

📌 Very common interview topic, and directly useful for things like "find the next available slot at or after time T" scheduling logic.

### 7.8 Set Implementations Comparison (Quick Memory)

| Set | Order | Speed | Structure |
|---|---|---|---|
| HashSet | ❌ No | Fastest | HashMap |
| LinkedHashSet | Insertion | Fast | LinkedHashMap |
| TreeSet | Sorted | Slower | TreeMap (Red-Black Tree) |

### 7.9 Thread-Safe Set Options

**❌ HashSet is NOT thread-safe.**

**⚠️ Synchronized Set (old way):**

```java
Set<Integer> syncSet =
    Collections.synchronizedSet(new HashSet<>());
```

Full locking, poor performance, and you still must manually synchronize during iteration.

**✅ CopyOnWriteArraySet (read-heavy):**

```java
CopyOnWriteArraySet<Integer> cowSet =
    new CopyOnWriteArraySet<>();
```

Internally backed by a `CopyOnWriteArrayList` (with duplicate checks on insert), so it inherits the same "copy the whole backing array on every write" behavior. ✔ Thread-safe ✔ No `ConcurrentModificationException` ❌ Writes are expensive (O(n), copies array + does an O(n) contains-check before insert). 📌 Best for **many reads, few writes** — e.g., a small, rarely-changing set of feature-flag names or whitelisted client IDs read on every request.

**✅ ConcurrentSkipListSet (Sorted + Concurrent):**

```java
ConcurrentSkipListSet<Integer> skipSet =
    new ConcurrentSkipListSet<>();
```

Backed by a **skip list** (a probabilistic, layered linked-list structure that gives O(log n) average search/insert/delete, similar in spirit to a balanced tree but lock-free and highly concurrent). ✔ Thread-safe ✔ Sorted ✔ High concurrency — multiple threads can read and write simultaneously without a single global lock, unlike a synchronized `TreeSet`. 📌 Best replacement for `TreeSet` in multi-threaded code, e.g., a concurrently-updated, always-sorted priority list of active sessions ordered by expiry time.

### 7.10 CopyOnWriteArraySet vs ConcurrentSkipListSet

| Feature | CopyOnWriteArraySet | ConcurrentSkipListSet |
|---|---|---|
| Thread-safe | Yes | Yes |
| Sorted | ❌ No | ✅ Yes |
| Read-heavy | Best | OK |
| Write-heavy | ❌ Bad | ✅ Good |
| Iteration | Snapshot (fail-safe) | Weakly consistent (fail-safe) |

### 7.11 EnumSet (New Topic)

`EnumSet` is a highly specialized, high-performance `Set` implementation designed **exclusively for enum types**. Internally, it is represented as a **bit vector** — each enum constant corresponds to a single bit in a `long` (or an array of `long`s if the enum has more than 64 constants). This makes operations like `add()`, `remove()`, `contains()`, union, and intersection extraordinarily fast — essentially bitwise operations — and far more memory-efficient than a `HashSet<MyEnum>`, which would box every enum constant and hash it.

```java
enum Day { MON, TUE, WED, THU, FRI, SAT, SUN }

EnumSet<Day> weekdays = EnumSet.range(Day.MON, Day.FRI);
EnumSet<Day> weekend = EnumSet.complementOf(weekdays);
EnumSet<Day> specific = EnumSet.of(Day.MON, Day.WED, Day.FRI);
```

Iteration order always follows the **natural (declaration) order** of the enum, regardless of insertion order — another benefit of the bit-vector representation.

`EnumSet` is not thread-safe by default; wrap with `Collections.synchronizedSet()` if shared across threads. There is no public constructor — you always use static factory methods (`of`, `range`, `allOf`, `noneOf`, `complementOf`).

> **Real-world relevance:** `EnumSet` is the idiomatic replacement for old-style bitmask flags (`int flags = FLAG_A | FLAG_B`). It's common in domain modeling — e.g., `EnumSet<Permission>` for a role's permission set, `EnumSet<DayOfWeek>` for recurring-schedule rules, or `EnumSet<HttpMethod>` for allowed methods on an endpoint — giving you type safety and set semantics with the performance of raw bit flags.

---

## 8. Queue & Deque Interface

### 8.1 Queue

The `Queue` interface models a collection designed for holding elements prior to processing, typically in **FIFO (First-In-First-Out)** order, though implementations like `PriorityQueue` order elements by priority instead of insertion time.

`Queue` provides two flavors of each core operation — one that throws an exception on failure, and one that returns a special value (`null` or `false`):

| Operation | Throws Exception | Returns Special Value |
|---|---|---|
| Insert | `add(e)` | `offer(e)` |
| Remove | `remove()` | `poll()` |
| Examine (peek) | `element()` | `peek()` |

This dual-method design is deliberate: in concurrent/bounded contexts (e.g., a full queue), you often *expect* failure as a normal condition rather than an exceptional one, so `offer()`/`poll()` are generally preferred in production code over `add()`/`remove()`.

```java
Queue<String> queue = new LinkedList<>();
queue.offer("A");
queue.offer("B");
System.out.println(queue.poll());  // A
System.out.println(queue.peek());  // B
```

> **Real-world relevance:** Task schedulers, print/job queues, breadth-first traversal in graph algorithms, and as the backbone of thread-pool work queues (`ThreadPoolExecutor` takes a `BlockingQueue<Runnable>`).

### 8.2 Deque / ArrayDeque (New Topic)

`Deque` (Double-Ended Queue, pronounced "deck") extends `Queue` and allows insertion and removal from **both ends** — the head and the tail. This makes it capable of acting as either a **Queue (FIFO)** or a **Stack (LIFO)**, all through one unified interface.

```java
public interface Deque<E> extends Queue<E> {
    void addFirst(E e);
    void addLast(E e);
    E removeFirst();
    E removeLast();
    E peekFirst();
    E peekLast();
    // plus offerFirst/offerLast, pollFirst/pollLast, push, pop, ...
}
```

**`ArrayDeque`** is the modern, high-performance implementation of `Deque`, backed by a **resizable circular array** (a circular buffer). Unlike `LinkedList`, it has no per-element node overhead (no `prev`/`next` object references), so it is significantly more cache-friendly and memory-efficient.

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);   // addFirst
stack.push(2);
stack.push(3);
System.out.println(stack.pop());  // 3 (LIFO)

Deque<Integer> queue = new ArrayDeque<>();
queue.offer(1);  // addLast
queue.offer(2);
System.out.println(queue.poll()); // 1 (FIFO)
```

**Why ArrayDeque replaced both `Stack` and `LinkedList` for these roles:**

* `java.util.Stack` is a **legacy class** that extends `Vector`, meaning it is synchronized (with the same performance penalty as `Vector`) and semantically odd (a stack that also lets you access elements by index because it's secretly a `Vector`). The JavaDoc itself recommends `Deque`/`ArrayDeque` instead.
* `LinkedList` as a queue/deque works, but every element is a heap-allocated node with two extra references, hurting cache locality and increasing GC pressure. `ArrayDeque` avoids this entirely.

`ArrayDeque` does **not allow `null` elements** (null is used internally as a sentinel to indicate an empty slot). It is **not thread-safe**.

**Time complexity:** `addFirst`/`addLast`/`removeFirst`/`removeLast`/`peek` → amortized **O(1)**. Resizing (doubling) happens the same way as `ArrayList`, but growth is amortized across the circular buffer.

> **Real-world relevance:** `ArrayDeque` is the go-to choice for implementing an **undo/redo stack** in an editor, a **sliding-window** algorithm (e.g., max-in-window problems), or a work-stealing/task stack in a custom thread pool. It's also frequently the correct answer to "implement a stack in Java" interview questions — not `java.util.Stack`.

### 8.3 PriorityQueue — Deep Dive (New Topic)

`PriorityQueue` is a `Queue` implementation where elements are **not** ordered by insertion time, but by **priority** — determined either by the elements' natural ordering (`Comparable`) or by an explicit `Comparator` supplied at construction time.

```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>(); // natural order (ascending) → min-heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Comparator.reverseOrder()); // max-heap
```

**Internal structure — Binary Heap:** Internally, `PriorityQueue` is backed by an **array-based binary heap** (specifically, a min-heap by default). A binary heap is a complete binary tree stored implicitly in an array, where for any element at index `i`:

* Its children are at indices `2*i + 1` and `2*i + 2`
* Its parent is at index `(i - 1) / 2`

The heap property guarantees that the parent is always "smaller than" (or "higher priority than") its children — but siblings and deeper descendants are **not** necessarily sorted relative to each other. This is the biggest source of confusion for engineers new to `PriorityQueue`:

⚠️ **Critical gotcha:** Iterating over a `PriorityQueue` (via `for-each`, `iterator()`, or `toString()`) does **NOT** return elements in priority order! Only repeated calls to `poll()` (which removes the current root/minimum and re-heapifies) guarantee sorted-order extraction.

```java
PriorityQueue<Integer> pq = new PriorityQueue<>(List.of(5, 1, 4, 2, 3));
System.out.println(pq);       // e.g. [1, 2, 4, 5, 3] — NOT fully sorted!
System.out.println(pq.poll()); // 1
System.out.println(pq.poll()); // 2
System.out.println(pq.poll()); // 3
```

**Time complexity:**

| Operation | Complexity | Why |
|---|---|---|
| `offer()` / `add()` | O(log n) | Insert at end, "sift up" to restore heap property |
| `poll()` / `remove()` | O(log n) | Remove root, move last element to root, "sift down" |
| `peek()` | O(1) | Root is always at index 0 |
| `contains()` | O(n) | No ordering guarantee across the array, must scan linearly |

**Custom-object example (common real-world pattern — task scheduling by priority):**

```java
record Task(String name, int priority) {}

PriorityQueue<Task> tasks = new PriorityQueue<>(
    Comparator.comparingInt(Task::priority)
);

tasks.offer(new Task("Low", 3));
tasks.offer(new Task("Critical", 1));
tasks.offer(new Task("Medium", 2));

while (!tasks.isEmpty()) {
    System.out.println(tasks.poll().name()); // Critical, Medium, Low
}
```

`PriorityQueue` does **not allow `null` elements**, and it is **not thread-safe** — the concurrent equivalent is `PriorityBlockingQueue` (section 8.4).

> **Real-world relevance:** `PriorityQueue` is the standard tool for **Dijkstra's algorithm**, **A\* pathfinding**, **event-driven simulations** (process the next event by timestamp), **top-K problems** (maintain a bounded min-heap of size K to track the K largest elements streaming through a system — very common in analytics/leaderboard pipelines), and **job schedulers** that need to always pick the highest-priority pending task.

### 8.4 BlockingQueue Family (New Topic)

`BlockingQueue` is a `java.util.concurrent` interface that extends `Queue` with **blocking** semantics: instead of failing or returning a sentinel when the queue is empty (on take) or full (on put), the calling thread simply **waits** until the operation can succeed. This makes `BlockingQueue` the foundation of the classic **producer-consumer pattern** in Java, without any manual `wait()`/`notify()` bookkeeping.

| Method type | Throws Exception | Special Value | Blocks | Times Out |
|---|---|---|---|---|
| Insert | `add(e)` | `offer(e)` | `put(e)` | `offer(e, timeout, unit)` |
| Remove | `remove()` | `poll()` | `take()` | `poll(timeout, unit)` |

**Key implementations:**

* **`ArrayBlockingQueue`** — fixed-capacity, array-backed, single lock shared by both `put` and `take`. Used when you need a strictly bounded buffer (e.g., a bounded task queue to apply backpressure so producers can't overwhelm memory).
* **`LinkedBlockingQueue`** — optionally bounded (defaults to `Integer.MAX_VALUE`, effectively unbounded), node-based, and uses **two separate locks** (one for `put`, one for `take`), allowing a producer and a consumer to operate truly concurrently without contending on the same lock. This is the default work queue used internally by `Executors.newFixedThreadPool()`.
* **`PriorityBlockingQueue`** — an unbounded blocking queue that orders elements like `PriorityQueue` (heap-based), but is thread-safe. `take()` blocks if the queue is empty.
* **`SynchronousQueue`** — a queue with **zero capacity**: every `put()` must wait for a matching `take()` and vice versa. It's essentially a direct hand-off channel between exactly one producer and one consumer at a time. Used internally by `Executors.newCachedThreadPool()`.
* **`DelayQueue`** — an unbounded blocking queue of `Delayed` elements, where an element can only be taken once its delay has expired. Useful for scheduling retries or expiring cache entries.

```java
BlockingQueue<String> queue = new LinkedBlockingQueue<>(100);

// Producer thread
new Thread(() -> {
    try {
        queue.put("job-1"); // blocks if queue is full
    } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
}).start();

// Consumer thread
new Thread(() -> {
    try {
        String job = queue.take(); // blocks if queue is empty
        System.out.println("Processing: " + job);
    } catch (InterruptedException e) { Thread.currentThread().interrupt(); }
}).start();
```

> **Real-world relevance:** `BlockingQueue` implementations are the literal backbone of Java's `ExecutorService`/thread pools — every `ThreadPoolExecutor` you configure in Spring (`ThreadPoolTaskExecutor`) is backed by a `BlockingQueue<Runnable>`, and choosing `ArrayBlockingQueue` (bounded, backpressure) vs `LinkedBlockingQueue` (defaults to unbounded, risk of `OutOfMemoryError` under sustained overload) is a real production tuning decision. Custom producer-consumer pipelines — e.g., a log-shipping agent reading from disk and pushing to Kafka on a separate thread — are also commonly built directly on `BlockingQueue`.

---

## 9. Map Interface (Separate Hierarchy)

The `Map` interface is **not part of the `Collection` hierarchy** (it does not extend `Collection`/`Iterable`), but it is still considered a core part of the JCF. It stores data in **key-value pairs**, where each key is unique. This is extremely useful when you need **fast lookups**, such as retrieving user details by user ID.

> **Real-world relevance:** Maps are everywhere in backend engineering — in-memory caches, DB row → object indexes, request-scoped context objects, configuration property stores, grouping results of a `Collectors.groupingBy()` stream operation, and JSON object representations.

### 9.1 HashMap — Deep Dive

#### 9.1.1 What a HashMap Really Is

In Java, a `HashMap` is a data structure used to store information in the form of **key–value pairs**, where each key is unique and is used to quickly retrieve its associated value. What makes `HashMap` extremely powerful is not just that it stores pairs, but **how it stores them internally**. Instead of keeping elements in a simple list or array and searching linearly, `HashMap` uses a clever mechanism called **hashing** to achieve **constant-time (O(1)) average performance** for both insertion (`put`) and retrieval (`get`).

Internally, a `HashMap` maintains an **array of buckets**. Each bucket can store zero or more entries. By default, when you create a new `HashMap` without specifying a size, it creates an internal array of **size 16**, and this size is always kept as a **power of 2** (16, 32, 64, …). This design is intentional because it allows very fast index calculations using bitwise operations instead of slower arithmetic like modulo (`%`).

Each key you insert is first converted into a **hash value**, and that hash determines **which bucket** the key-value pair will be stored in. This is the foundation of how `HashMap` achieves speed.

#### 9.1.2 From Key to Bucket: Hashing Explained

Whenever you insert a key into a `HashMap`, Java calls the key's `hashCode()` method. This method returns an integer that represents the key. However, Java doesn't directly use this raw hash code. Instead, it applies a **refinement step** to spread bits more evenly across buckets and reduce collisions.

In Java 8+, the refined hash is calculated like this:

```java
int hash = key.hashCode() ^ (key.hashCode() >>> 16);
```

What this does is mix the higher bits of the hash code with the lower bits. This is important because the bucket index calculation depends mainly on the lower bits. Without this mixing, many keys might land in the same bucket, degrading performance.

Once the refined hash is calculated, Java determines the bucket index using this formula:

```java
index = (table.length - 1) & hash;
```

This bitwise AND operation is extremely fast and works correctly only because the array length is always a power of 2. For example, if the array size is 16, valid indices range from 0 to 15.

#### 9.1.3 Understanding the Put Operation (Insertion Flow)

When you call `put(key, value)`, the `HashMap` follows a very systematic process. First, it computes the refined hash of the key and calculates the bucket index. If the bucket at that index is **empty**, the new key-value pair is simply placed there, and the operation completes in constant time.

However, things become more interesting when the bucket is **already occupied**. This situation is called a **collision**, meaning two different keys have produced the same bucket index. In this case, Java does not overwrite the existing entry. Instead, it traverses the existing entries in that bucket and compares keys using the `equals()` method.

If a key is found that is considered equal, the old value is replaced with the new value. If no matching key exists, a new node is appended to the bucket's structure.

```java
HashMap<String, Integer> map = new HashMap<>();

map.put("apple", 50);
map.put("orange", 80);
map.put("grapes", 20);
```

If `"orange"` and `"grapes"` both map to the same bucket index (say index 14), the structure inside that bucket will look like:

```
orange -> grapes
```

This chaining ensures that no data is lost even when collisions occur.

#### 9.1.4 Internal Node Structure

Each entry inside a `HashMap` is stored as a **Node object**. The array doesn't store key-value pairs directly; instead, it stores references to these nodes.

```java
static class Node<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;
}
```

Each node stores the hash, the key, the value, and a reference to the next node in case of a collision. This linked structure allows multiple entries to exist in the same bucket.

An important detail is that `HashMap` allows **one null key**. Since `null` has no hash code, Java assigns it to bucket index `0`. Any additional null keys are treated as duplicates and overwrite the existing one.

#### 9.1.5 Get Operation: How Retrieval Works Internally

The `get(key)` operation mirrors the insertion logic. First, Java calculates the refined hash of the key and determines the bucket index. If the bucket is empty, `null` is immediately returned.

If the bucket contains entries, Java compares the stored hash values first (for quick elimination) and then uses `equals()` to find the exact matching key. Once found, the associated value is returned.

This means that even during collisions, the search remains efficient because comparisons are limited to only the entries inside a single bucket — not the entire map.

#### 9.1.6 Collision Handling Evolution (Linked List → Tree)

Initially, when collisions occur, entries in a bucket are stored as a **linked list**. Searching a linked list takes linear time, but since collisions are expected to be rare, this usually isn't a problem.

However, if too many entries accumulate in a single bucket (more than **8 entries**, and the table size is at least 64), Java automatically converts the linked list into a **Red-Black Tree**, which is a self-balancing binary search tree. This change ensures that lookup time improves from O(n) to **O(log n)** even in worst-case scenarios. If entries in a treeified bucket later drop below 6, Java converts it back to a linked list (untreeify) to avoid unnecessary tree overhead for small buckets.

This improvement was introduced in **Java 8** specifically to protect against performance degradation due to poor hash functions or **malicious/adversarial inputs** (e.g., an attacker deliberately submitting many keys with the same `hashCode()` to force an O(n) linked-list DoS — this was a real security concern in earlier Java versions, sometimes called a "hash flooding" attack).

#### 9.1.7 Resizing and Rehashing (Why Capacity Matters)

A `HashMap` does not grow endlessly in the same array. It uses a concept called a **load factor**, which is `0.75` by default. This means that when 75% of the buckets are filled, the map resizes.

For example, with a capacity of 16:

```
Threshold = 16 × 0.75 = 12
```

Once the 13th entry is added, the internal array size doubles to 32. At this point, **every existing entry is rehashed** and placed into a new bucket based on the new array size. This operation is expensive, which is why frequent resizing should be avoided.

That's why, when you know the expected number of entries in advance, it's a good practice to specify an initial capacity:

```java
HashMap<String, Integer> map = new HashMap<>(32);
```

📌 **Practical tip:** To avoid *any* resize for `n` known entries, size the map as `(int) (n / 0.75) + 1` — this is a common code-review point when a service builds a large map from a bulk DB fetch in a hot path.

#### 9.1.8 Custom Keys: Why hashCode() and equals() Matter

When using custom objects as keys, `HashMap` relies entirely on the correct implementation of `hashCode()` and `equals()`. If two objects are considered equal by `equals()`, they **must** return the same hash code. Violating this contract leads to unpredictable behavior — e.g., you `put()` a key and then `get()` with an "equal" object returns `null` because it landed in a different bucket.

```java
class Student {
    int id;
    String name;

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Student)) return false;
        Student s = (Student) obj;
        return this.id == s.id;
    }
}
```

Here, students are uniquely identified by `id`, not by name. This ensures consistent behavior when used as keys in a `HashMap`.

⚠️ **Mutability trap (very common production bug):** If a mutable field used in `hashCode()`/`equals()` is changed *after* the object has been inserted as a map key, the object becomes "lost" — it now hashes to a different bucket than the one it's actually stored in, so `get()`/`containsKey()` will fail to find it even though `map.toString()` shows it's there. Prefer **immutable objects** (or immutable ID fields) as map keys.

#### 9.1.9 Thread Safety Consideration

One important limitation of `HashMap` is that it is **not synchronized**. In multi-threaded environments, concurrent modifications can lead to inconsistent data or even infinite loops during resizing (a well-known Java 7 bug where concurrent resize could create a circular linked list, causing `get()` to spin forever — largely mitigated but still a strong reason to avoid raw `HashMap` under concurrent writes). For such scenarios, Java provides `ConcurrentHashMap` (section 9.8), which is designed for high concurrency without locking the entire map.

> **Real-world relevance:** `HashMap` is the default backing structure Jackson uses when deserializing a generic JSON object (`Map<String, Object>`), the default choice for an in-memory lookup cache/index built once at startup (e.g., mapping country codes to country names), and the implicit structure behind `Collectors.toMap()` / `Collectors.groupingBy()` in Streams.

---

### 9.2 LinkedHashMap — Deep Dive (incl. LRU Cache)

#### 9.2.1 Why LinkedHashMap Exists

A normal `HashMap` is excellent for fast lookups, but it has one big limitation: **it does not remember the order in which entries were added or accessed**. When you iterate over a `HashMap`, the order looks random and unpredictable. This becomes a problem in real-world applications where order actually matters, such as caching, logging, or maintaining recently accessed data.

`LinkedHashMap` was introduced to solve this exact issue. It extends `HashMap` and adds **order-awareness** without sacrificing performance. Internally, it combines the **hashing power of HashMap** with a **doubly linked list** that keeps track of entry order. This means you still get **O(1) average time complexity** for `put()` and `get()`, but now with a predictable iteration order.

#### 9.2.2 Internal Structure: HashMap + Doubly Linked List

Internally, `LinkedHashMap` works just like a `HashMap` for storage. Entries are placed into buckets based on hashing. On top of that, every entry is also linked together using a **doubly linked list**. Each entry stores references to the **previous** and **next** entry.

This linked list is what preserves order. The "head" of the list represents the **oldest entry**, and the "tail" represents the **most recent entry**. Depending on configuration, "recent" can mean either *recently inserted* or *recently accessed*.

This hybrid design is the reason `LinkedHashMap` is slightly heavier than `HashMap`, but still extremely fast.

#### 9.2.3 Insertion Order Mode (Default Behavior)

By default, `LinkedHashMap` maintains **insertion order**. This means that elements are iterated in the same order in which they were added to the map.

```java
LinkedHashMap<String, String> map = new LinkedHashMap<>();
map.put("orange", "L");
map.put("apple", "M");
map.put("guava", "S");

System.out.println(map);
```

Output: `{orange=L, apple=M, guava=S}`

Even if you call `get("orange")` or `get("apple")`, the order will not change. The linked list preserves the original insertion sequence. This makes `LinkedHashMap` useful for **ordered logs**, **configuration maps**, and **data that must remain stable in sequence**.

#### 9.2.4 Access Order Mode (The Game Changer)

`LinkedHashMap` becomes truly powerful when you enable **access order**. This is done by passing `true` as the third constructor parameter.

```java
LinkedHashMap<String, String> map =
    new LinkedHashMap<>(16, 0.75f, true);
```

In this mode, **every access matters**. Whenever you call `get()` or update an existing key using `put()`, that entry is **moved to the end of the linked list**. The map now represents **least recently used → most recently used** order.

This behavior is what makes `LinkedHashMap` perfect for implementing **LRU caches**.

#### 9.2.5 Understanding Access Order Through Example

```java
LinkedHashMap<String, String> map =
    new LinkedHashMap<>(16, 0.75f, true);

map.put("orange", "L");
map.put("apple", "M");
map.put("guava", "S");

System.out.println(map);
```

Even though entries were inserted in the order `orange → apple → guava`, iteration starts from the **least recently accessed**. Now observe what happens when we access elements:

```java
map.get("apple");
System.out.println(map); // {guava=S, orange=L, apple=M}
```

`apple` moves to the end because it was just accessed. If we then access `"orange"`:

```java
map.get("orange");
System.out.println(map); // {guava=S, apple=M, orange=L}
```

This dynamic rearrangement is automatic and requires no manual tracking.

#### 9.2.6 How LinkedHashMap Enables LRU Cache

An **LRU (Least Recently Used) cache** automatically removes the least recently accessed item when the cache reaches its capacity. `LinkedHashMap` makes this trivial by providing a protected method called `removeEldestEntry()`.

This method is called **after every put operation**. If it returns `true`, the **eldest entry (head of the list)** is removed.

```java
class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private int capacity;

    public LRUCache(int capacity) {
        super(capacity, 0.75f, true); // access order enabled
        this.capacity = capacity;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }
}
```

Here, the cache always removes the least recently used entry when the size exceeds the defined limit.

#### 9.2.7 LRU Cache in Action

```java
LRUCache<String, Integer> cache = new LRUCache<>(3);

cache.put("bob", 99);
cache.put("alice", 89);
cache.put("ram", 91);

System.out.println(cache);
```

At this point, the cache contains the three most recent entries. Now watch what happens when we add one more:

```java
cache.put("vipul", 89);
System.out.println(cache);
```

The oldest entry (`bob`) is automatically removed, because the cache exceeded its capacity. Any attempt to access `"bob"` now returns `null`.

```java
System.out.println(cache.get("bob")); // null
```

This entire eviction logic is handled internally by `LinkedHashMap` with no additional code.

#### 9.2.8 Utility Methods That Make Life Easier

Because `LinkedHashMap` extends `HashMap`, it inherits many helpful utility methods.

```java
map.getOrDefault("vipul", 0);
map.putIfAbsent("bob", 92);
```

These methods reduce the need for null checks and make cache logic much cleaner and safer.

#### 9.2.9 Performance and Trade-offs

`LinkedHashMap` maintains **O(1) average performance** for `get()` and `put()`, just like `HashMap`. The only added cost is maintaining the doubly linked list, which introduces a small memory and pointer-management overhead.

It is **not thread-safe**, so in concurrent environments you must either synchronize externally (`Collections.synchronizedMap()`) or use a purpose-built concurrent LRU implementation (e.g., backed by `Caffeine` or `Guava Cache` in real production systems, rather than a raw synchronized `LinkedHashMap`).

#### 9.2.10 Real-World Use Cases

`LinkedHashMap` is commonly used for:

* LRU caches (in-memory caching)
* Ordered logs
* Session tracking
* Access-frequency monitoring
* Maintaining predictable iteration order

Whenever **order + fast lookup** are both required, `LinkedHashMap` is the ideal choice.

> **Real-world relevance:** Before reaching for a full-blown caching library (Caffeine, Guava, Redis), a simple in-process bounded cache — e.g., caching the last 500 resolved DNS lookups, or the last N computed report results for a dashboard — is often implemented with exactly the `LinkedHashMap` + `removeEldestEntry()` pattern shown above. It's also a very common interview coding exercise: "implement an LRU cache."

---

### 9.3 SortedMap / NavigableMap / TreeMap

#### 9.3.1 What is SortedMap in Java?

**SortedMap** is a sub-interface of `Map` that **maintains its keys in sorted order**.

* Sorting is done by natural ordering of keys (`Comparable`) OR by a custom `Comparator`
* Values are **not sorted**, only keys are

```
Map → SortedMap → NavigableMap
```

📌 **Important:** Sorting happens **automatically** when keys are inserted.

#### 9.3.2 Primary Implementation — TreeMap

```java
SortedMap<K, V> map = new TreeMap<>();
```

**Why TreeMap?** Backed by a **Red-Black Tree** — a self-balancing Binary Search Tree — guaranteeing **O(log n)** time for `put()`, `get()`, `remove()`, `containsKey()`.

A **Red-Black Tree** is a self-balancing BST that prevents the tree from becoming skewed, ensuring height ≈ `log n`. This avoids the worst-case `O(n)` behavior seen in unbalanced trees (e.g., inserting already-sorted data into a naive BST).

#### 9.3.3 Basic SortedMap Example

```java
SortedMap<String, Integer> map = new TreeMap<>();
map.put("Vivek", 91);
map.put("Shubham", 99);
map.put("Mohit", 78);
map.put("Vipul", 77);
System.out.println(map);
// {Mohit=78, Shubham=99, Vipul=77, Vivek=91} — lexicographic key order
```

#### 9.3.4 Essential SortedMap Methods

Assume keys = `{77, 78, 91, 99}`

| Method | Description | Result |
|---|---|---|
| `firstKey()` | Smallest key | 77 |
| `lastKey()` | Largest key | 99 |
| `headMap(91)` | `< 91` | `{77,78}` |
| `tailMap(91)` | `≥ 91` | `{91,99}` |
| `subMap(78,91)` | `≥78 and <91` | `{78}` |

📌 These methods return **views**, not copies — mutating the view mutates the backing map, same caveat as `List.subList()`.

#### 9.3.5 Custom Sorting in SortedMap (Comparator)

```java
SortedMap<Integer, String> map =
    new TreeMap<>((a, b) -> b - a); // descending
```

📌 Sorting logic is applied **only to keys**.

#### 9.3.6 NavigableMap — Advanced SortedMap

**NavigableMap** extends `SortedMap` and adds **navigation & range operations**.

```java
NavigableMap<Integer, String> map = new TreeMap<>();
```

Navigation methods (assume keys = `{1, 3, 5}`):

```java
nav.floorKey(4);    // 3
nav.ceilingKey(2);  // 3
nav.lowerKey(3);    // 1
nav.higherKey(3);   // 5
```

| Method | Meaning |
|---|---|
| floorKey(k) | ≤ k (greatest) |
| ceilingKey(k) | ≥ k (smallest) |
| lowerKey(k) | < k |
| higherKey(k) | > k |

**Entry-level navigation:**

```java
nav.higherEntry(1); // returns Map.Entry<Integer,String>
```

📌 Useful when **both key and value** are needed in one call.

**Descending views:**

```java
NavigableMap<Integer, String> desc = map.descendingMap();
```

📌 Creates a **reverse-sorted view**, not a new map.

#### 9.3.7 Time Complexity Comparison

| Operation | HashMap | TreeMap |
|---|---|---|
| put/get/remove | O(1) avg | O(log n) |
| containsKey | O(1) avg | O(log n) |
| Iteration | Random | Sorted |
| Ordering | ❌ No | ✅ Yes |

👉 Use **TreeMap** when sorted data is required.

#### 9.3.8 Reference Type Flexibility

```java
Map<String,Integer> m = new TreeMap<>();
SortedMap<String,Integer> sm = new TreeMap<>();
NavigableMap<String,Integer> nm = new TreeMap<>();
```

| Reference | Access |
|---|---|
| Map | Basic map ops |
| SortedMap | + range & boundary ops |
| NavigableMap | + navigation & descending |

📌 Object decides behavior, reference decides **access**.

#### 9.3.9 Key Rules & Constraints

✔ Keys must be **Comparable** or a `Comparator` must be provided
✔ Duplicate keys → **value replaced**
✔ Null keys → ❌ (`TreeMap` throws `NullPointerException`)
✔ Values → can be null

#### 9.3.10 Real-World Use Cases

| Scenario | Why TreeMap |
|---|---|
| Leaderboard | Sorted scores |
| Phone directory | Alphabetical order |
| Configuration files | Ordered properties |
| Range queries | headMap / subMap |
| Time-series bucketing | `floorKey`/`ceilingKey` to find nearest timestamp bucket |

> **Real-world relevance:** `TreeMap` (or `ConcurrentSkipListMap` for concurrent use) is a strong choice whenever you need "closest match" lookups — e.g., rate-limiting tiers keyed by request-count thresholds, versioned configuration where you want the config active *as of* a given timestamp (`floorEntry(timestamp)`), or a scheduling system's sorted-by-time event map.

#### 9.3.11 SortedMap vs PriorityQueue (Quick Clarification)

| Feature | TreeMap | PriorityQueue |
|---|---|---|
| Data structure | Map | Queue |
| Sorting | Entire map | Only head |
| Access | Random (any key) | Head only |
| Use case | Ordered key-value | Min/Max element |

---

### 9.4 Hashtable

#### 9.4.1 Why Hashtable Was Introduced (Historical Context)

`Hashtable` is one of the **oldest classes in Java**, introduced way back in **JDK 1.0**, long before the Java Collections Framework existed. At that time, Java was just starting to support multithreading, and developers needed a **thread-safe key–value data structure**. `Hashtable` was Java's first attempt at solving this problem.

When the Collections Framework was later introduced in JDK 1.2, `Hashtable` was retrofitted to implement the `Map` interface. However, its original design decisions remained unchanged, which is why today it is considered a **legacy class**.

#### 9.4.2 How Hashtable Achieves Thread Safety

The most important thing to understand about `Hashtable` is **how it ensures thread safety**. Every public method in `Hashtable`, such as `put()`, `get()`, and `remove()`, is declared as `synchronized`.

This means **only one thread at a time** can access the map, regardless of whether the operation is a read or a write. Even two threads trying to just read values cannot proceed in parallel. They must wait for each other.

Conceptually, this is equivalent to placing a **single lock on the entire map**. While this guarantees correctness, it creates severe performance problems in modern applications where many threads operate concurrently.

#### 9.4.3 Why Full-Map Locking Is So Slow

```java
synchronized public V get(Object key) {
    // entire table locked
}
```

This design causes **high contention**, meaning threads spend more time waiting for locks than doing useful work. As the number of threads increases, performance degrades sharply. This is the biggest reason why `Hashtable` does not scale well.

#### 9.4.4 No Null Keys or Values

Unlike `HashMap`, `Hashtable` does **not allow null keys or null values**. Any attempt to insert a null will immediately throw a `NullPointerException`.

```java
Hashtable<String, String> table = new Hashtable<>();
table.put("apple", "1");     // OK
table.put(null, "2");        // ❌ NullPointerException
table.put("banana", null);   // ❌ NullPointerException
```

This restriction was originally added to avoid ambiguity in concurrent environments, but modern concurrent maps solve this problem more elegantly without such harsh limitations.

#### 9.4.5 Internal Data Structure

Internally, `Hashtable` uses an **array of buckets**, where each bucket stores entries in a **linked list** when collisions occur — similar to how `HashMap` worked before Java 8. Unlike modern `HashMap`/`ConcurrentHashMap`, `Hashtable` **does not support treeification**, so even with many collisions in one bucket, performance can degrade to **O(n)** in worst-case scenarios.

#### 9.4.6 Why ConcurrentHashMap Replaced Hashtable

`ConcurrentHashMap` was introduced to fix exactly what `Hashtable` got wrong. Instead of locking the entire map, it uses **fine-grained synchronization**, allowing many threads to read and write concurrently as long as they are working on different parts of the map. See section 9.8 for the full deep dive.

#### 9.4.7 Why Hashtable Is Considered Legacy Today

Although `Hashtable` still exists for backward compatibility, it is **strongly discouraged** in modern Java development. Today, it is mainly encountered when maintaining very old legacy systems, migrating ancient codebases, or in interviews.

**Mental model:** `Hashtable = Thread-safe by brute force` — it locks everything, blocks everyone, and guarantees correctness, but at the cost of performance and scalability.

> **Real-world relevance:** You'll mostly encounter `Hashtable` today via `java.util.Properties` (which extends `Hashtable<Object,Object>` for historical reasons) when reading `.properties` config files. For any new thread-safe map requirement, `ConcurrentHashMap` is the correct choice.

---

### 9.5 WeakHashMap

#### 9.5.1 Why WeakHashMap Exists (The Memory Leak Problem)

In Java, one of the most dangerous and silent problems is a **memory leak**. This usually happens not because Java lacks garbage collection, but because **objects are still strongly referenced**, so the JVM is not allowed to remove them. Normal maps like `HashMap` and `LinkedHashMap` keep **strong references to keys**, which means that once you put something into the map, it will stay there **forever** unless you explicitly remove it.

This becomes a serious issue in **caching scenarios**. Imagine caching image thumbnails, video metadata, or computed results. These objects are useful only temporarily. If the cache keeps growing and nothing removes old entries, memory usage keeps increasing until the application crashes.

`WeakHashMap` was designed to solve exactly this problem. It allows the JVM to **automatically remove entries** when they are no longer useful, without requiring manual cleanup logic.

#### 9.5.2 Understanding Garbage Collection Through References

To understand `WeakHashMap`, you must first understand **reference strength**. In Java, the most common reference type is a **strong reference**.

```java
Phone p = new Phone("iPhone");
```

As long as `p` exists, the `Phone` object **cannot be garbage-collected**. When you do `p = null;`, the object becomes **eligible for garbage collection**, meaning the JVM *may* remove it in the next GC cycle.

Now comes the key idea: **WeakReference**.

```java
WeakReference<Phone> weakRef =
    new WeakReference<>(new Phone("iPhone"));
```

Here, the JVM is allowed to collect the `Phone` object **even though the WeakReference still exists**. After garbage collection, calling `weakRef.get()` may return `null`, because the object has already been destroyed.

This behavior is the foundation of `WeakHashMap`.

#### 9.5.3 What Makes WeakHashMap Special Internally

A `WeakHashMap` works just like a normal map, except for one critical difference: 👉 **Keys are stored as weak references**, not strong references.

This means the map does *not* protect the key from garbage collection. If there is **no strong reference anywhere else in the application** pointing to the key object, the JVM is free to destroy it. When that happens, the corresponding **entire map entry is automatically removed**.

#### 9.5.4 WeakHashMap in Action: Simple Cache Example

```java
class Image {
    String name;
    Image(String name) { this.name = name; }
    public String toString() { return name; }
}

WeakHashMap<Image, String> imageCache = new WeakHashMap<>();

Image img1 = new Image("img1");
Image img2 = new Image("img2");

imageCache.put(img1, "thumbnail1");
imageCache.put(img2, "thumbnail2");

System.out.println(imageCache); // both entries present

img1 = null;
img2 = null;   // No strong references remain

System.gc();

System.out.println(imageCache); // becomes empty automatically
```

Because there are **no strong references to the keys anymore**, the garbage collector removes them, and the `WeakHashMap` becomes empty **automatically**, without calling `remove()` even once.

#### 9.5.5 The String Literal Trap (Very Important!)

One of the most common mistakes with `WeakHashMap` is using **string literals as keys**.

```java
WeakHashMap<String, String> map = new WeakHashMap<>();
map.put("img1", "thumbnail"); // will NEVER be removed
```

Why? Because string literals are stored in the **String Pool**, and the JVM keeps a strong reference to them for the entire lifetime of the application. To make the key weakly reachable, you must use `new String("img1")` or use non-literal keys such as custom objects.

#### 9.5.6 Iteration Behavior

Iterating over a `WeakHashMap` is different from iterating over a `HashMap`. Because garbage collection can happen **at any time**, the map may shrink during iteration. This makes `WeakHashMap` unsuitable for logic that depends on stable iteration order or guaranteed presence of data.

#### 9.5.7 When to Use / Avoid WeakHashMap

**Use when:** cached data can be recomputed, you want the JVM to manage memory automatically, stale data should disappear without manual cleanup, keys naturally become unreachable over time (image caches, metadata caches, canonicalization maps, listener registries).

**Avoid for:** critical business data, configuration values, data that must not disappear unexpectedly, or scenarios where cache misses are expensive/dangerous.

Like `HashMap`, `WeakHashMap` is **not thread-safe**, and there is no dedicated concurrent variant.

> **Real-world relevance:** `WeakHashMap` is commonly used for canonicalizing/interning objects (mapping equivalent objects to a shared instance while letting unused ones be collected), or for building listener/callback registries where you don't want the registry itself to keep a listener object alive after the rest of the application has dropped it (avoiding a classic "listener leak"). It's less common in typical CRUD backend code and more common in frameworks/library-level code.

---

### 9.6 IdentityHashMap

#### 9.6.1 Why IdentityHashMap Exists (A Very Different Idea)

In Java, almost all collections are designed around **logical equality**. When you use a `HashMap`, Java does not care whether two objects are the *same instance* in memory; it only cares whether they are **logically equal** according to `equals()` and `hashCode()`.

However, there are rare but important situations where this behavior is actually wrong. Sometimes, you want to treat **each object instance as unique**, even if two objects look identical in content. This is exactly why `IdentityHashMap` exists.

`IdentityHashMap` is a special-purpose map that compares keys using **reference equality (`==`) instead of `equals()`**, meaning it only considers two keys equal if they are literally the **same object in memory**.

#### 9.6.2 equals() vs == — The Fundamental Difference

`equals()` checks **logical equality** (usually content). `==` checks **reference equality** (same object in memory). `HashMap` uses `equals()`. `IdentityHashMap` uses `==` instead — this single design decision changes everything about how the map behaves.

#### 9.6.3 How IdentityHashMap Handles hashCode()

In `IdentityHashMap`, **all overridden `hashCode()` methods are ignored**. Instead, Java always uses `System.identityHashCode(key)`, a value based on the object's identity (roughly related to its memory address). Two distinct objects will almost always have different identity hash codes, even if their contents are identical. This ensures that **object identity**, not object content, determines map behavior.

#### 9.6.4 The Classic String Example

```java
String key1 = new String("key");
String key2 = new String("key");
```

These two objects contain the same text, but they are **two different objects in memory**.

```java
HashMap<String, String> hashMap = new HashMap<>();
hashMap.put(key1, "value1");
hashMap.put(key2, "value2");
System.out.println(hashMap); // {key=value2} — overwritten because equals() is true

IdentityHashMap<String, String> idMap = new IdentityHashMap<>();
idMap.put(key1, "value1");
idMap.put(key2, "value2");
System.out.println(idMap.size()); // 2 — key1 == key2 is false, distinct keys
```

#### 9.6.5 Why String Literals Behave Differently

```java
String s1 = "key";
String s2 = "key";
```

Here, `s1 == s2` is `true` because string literals are interned in the **String Pool**, and Java reuses the same object. If you put these into an `IdentityHashMap`, they will be treated as the **same key** — this is due to JVM interning, not map behavior.

#### 9.6.6 When IdentityHashMap Is Useful

* **Graph traversal** — tracking visited nodes by actual object identity, not logical equality, avoiding infinite loops when objects reference each other.
* **Object canonicalization** — mapping many equivalent-looking objects to a single canonical instance while still distinguishing original references.
* **Debugging frameworks, proxy systems, and serialization tools** — where tracking the exact object instance is critical (e.g., `ObjectOutputStream` internally uses an `IdentityHashMap`-like structure to detect already-serialized objects and avoid infinite recursion on circular references).

#### 9.6.7 Why It's Rarely Used

For most applications, `IdentityHashMap` is unnecessary and even dangerous — using reference equality can lead to subtle bugs if developers expect logical equality. `HashMap` covers about 99% of real-world use cases. It is also **not thread-safe**.

**Mental model:**
> **HashMap asks:** "Do these two keys look the same?"
> **IdentityHashMap asks:** "Are these two keys the same object?"

> **Real-world relevance:** Beyond serialization frameworks and cycle-detection algorithms, `IdentityHashMap` occasionally shows up in dependency-injection containers or bytecode-manipulation libraries (e.g., tracking which specific proxy instances have already been processed) — rarely in standard application/business logic code.

---

### 9.7 EnumMap (New Topic)

`EnumMap` is a specialized `Map` implementation whose keys must all be values from a **single enum type**. Like `EnumSet`, it is internally backed by a simple **array** indexed by the enum constant's `ordinal()` value — not a hash table — making it extremely fast (effectively array-access speed) and far more memory-compact than a `HashMap<MyEnum, V>`.

```java
enum Status { PENDING, APPROVED, REJECTED, CANCELLED }

EnumMap<Status, Integer> countByStatus = new EnumMap<>(Status.class);
countByStatus.put(Status.APPROVED, 120);
countByStatus.put(Status.PENDING, 45);

System.out.println(countByStatus);
// {PENDING=45, APPROVED=120} — always iterates in enum declaration order
```

Because it's array-backed and indexed by ordinal, iteration order is always the enum's **natural declaration order**, regardless of insertion order — a useful, predictable property for reporting/UI display. `EnumMap` does not allow `null` keys (though it does allow `null` values), and it is **not thread-safe** by default (wrap with `Collections.synchronizedMap()` if needed).

> **Real-world relevance:** `EnumMap` is the idiomatic choice whenever you're aggregating or configuring something **per enum constant** — e.g., counting orders by `OrderStatus`, mapping `HttpMethod` to a handler function, or defining per-`LogLevel` formatting rules. It's a small but real performance/memory win over `HashMap` that senior reviewers often flag in code review when they see `Map<SomeEnum, V>` declared as `HashMap`.

---

### 9.8 ConcurrentHashMap — Deep Dive

#### 9.8.1 Why ConcurrentHashMap Exists

In multithreaded applications, especially in **web servers, Spring Boot apps, microservices, and caches**, multiple threads often access and modify shared data at the same time. A normal `HashMap` is **not thread-safe**, so concurrent reads and writes can corrupt internal data structures, cause infinite loops, or crash the application.

Java tried to solve this earlier using `Hashtable`, but it synchronized **every method on the entire map**. This meant only one thread could read or write at a time, making it extremely slow and unscalable.

`ConcurrentHashMap` was introduced to solve both problems at once:

* Ensure **thread safety**
* Avoid **full-map locking**, so many threads can work concurrently

#### 9.8.2 What Makes ConcurrentHashMap Special

The key idea behind `ConcurrentHashMap` is **fine-grained synchronization**. Instead of locking the entire map, it locks only **small portions of data**, allowing multiple threads to operate in parallel as long as they are not modifying the same part.

Reads are designed to be **lock-free** and extremely fast, while writes use smart synchronization techniques that minimize contention. This makes `ConcurrentHashMap` the **default choice** for shared mutable maps in concurrent Java applications.

#### 9.8.3 Java 7 Internals: Segment-Based Locking (Historical)

In Java 7 and earlier, `ConcurrentHashMap` was internally divided into **segments**. Each segment was like a small independent `HashMap` with its own lock. By default, there were **16 segments**, controlled by a parameter called `concurrencyLevel`. This meant up to 16 threads could safely write to the map at the same time, as long as they were writing to different segments.

When a thread wanted to update a key, it first determined which segment the key belonged to, then locked **only that segment**, leaving other segments completely free for other threads. Reads were mostly **lock-free**, unless a write was happening in the same segment at the same time.

This approach was a huge improvement over `Hashtable`, but the number of concurrent writers was capped by the number of segments, and the design was more complex than what came later.

#### 9.8.4 Java 8+ Revolution: CAS + Bucket-Level Locking

Starting with Java 8, `ConcurrentHashMap` was **redesigned completely**. The segment concept was removed entirely. Instead, Java uses a single array of buckets (like `HashMap`) but combines:

* **CAS (Compare-And-Swap)** operations
* **Synchronized blocks at the bucket level**

Reads are now **always lock-free**. Java uses `volatile` variables and memory visibility guarantees so threads always see the latest value safely.

When a write happens, Java first tries to insert the entry using a CAS operation on the bucket head. If there is no contention, the write completes without locking at all. Only when collisions occur, or when resizing is needed, does Java fall back to synchronizing **on that specific bucket**, not the entire map.

This design allows **massive scalability** even under heavy concurrent access.

#### 9.8.5 How Reads Work (Always Lock-Free)

When you call `get(key)` on a `ConcurrentHashMap`, **no lock is acquired**. The map simply calculates the bucket index and reads the value using volatile reads. This ensures extremely fast reads, no thread blocking, and safe visibility of the latest updates — which is why `ConcurrentHashMap` performs exceptionally well in **read-heavy workloads** like caches and configuration stores.

#### 9.8.6 How Writes Work (CAS First, Lock Only If Needed)

When a thread calls `put()` or `remove()`:

1. Java calculates the bucket index.
2. It tries to insert using **CAS**.
3. If CAS succeeds, no lock is used.
4. If CAS fails (collision or resize), Java synchronizes **only that bucket**.
5. Other buckets remain free for other threads.

This fine-grained locking ensures high throughput even when many threads are updating the map simultaneously. Like `HashMap`, buckets that accumulate too many collisions (>8 entries, table size ≥64) are treeified into Red-Black Trees for guaranteed O(log n) worst-case lookup.

#### 9.8.7 Resizing Without Stopping the World

In a normal `HashMap`, resizing is a **blocking and expensive** operation — one thread doubles the array and rehashes everything. In `ConcurrentHashMap`, resizing is **incremental and cooperative**: multiple threads can help with resizing, moving buckets gradually instead of stopping all operations. This prevents long pauses and keeps the application responsive even under load.

#### 9.8.8 Iteration Behavior: Weakly Consistent, Not Fail-Fast

Unlike `HashMap`, `ConcurrentHashMap` **does not throw `ConcurrentModificationException`** during iteration. Its iterators are **weakly consistent**, meaning:

* They reflect the state of the map at some point in time
* They may or may not see concurrent updates
* They never throw exceptions
* They never block writers

This makes iteration safe and predictable in concurrent environments, even though the view may not be perfectly up to date.

#### 9.8.9 Atomic Methods That Replace Manual Locking

`ConcurrentHashMap` provides several atomic methods that eliminate the need for explicit synchronization.

```java
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();

map.putIfAbsent("key", 1);      // Adds only if missing
map.replace("key", 1, 2);       // Replaces only if current value is 1
map.remove("key", 2);           // Removes only if value matches

// Java 8+ atomic aggregate helpers, very useful for counters:
map.compute("key", (k, v) -> (v == null) ? 1 : v + 1);
map.computeIfAbsent("key2", k -> new ArrayList<>());
map.merge("key", 1, Integer::sum);
```

These methods ensure correctness even when many threads are racing, without you having to write synchronized blocks. `compute`/`merge`-style atomic updates are the idiomatic way to build thread-safe in-memory counters and grouping structures.

#### 9.8.10 Why Null Keys and Values Are Forbidden

`ConcurrentHashMap` does **not allow null keys or null values**. This is intentional. In concurrent code, `null` could mean "the key is absent" OR "the key exists but maps to null" — a genuine ambiguity when another thread might be concurrently modifying the map (`containsKey()` followed by `get()` is not atomic). Allowing null would introduce ambiguity and race conditions. By forbidding nulls, `ConcurrentHashMap` keeps its API safe and predictable.

#### 9.8.11 Comparing HashMap, ConcurrentHashMap, and Hashtable

`HashMap` is fastest but unsafe in multithreading. `Hashtable` is thread-safe but slow because it locks the entire map. `ConcurrentHashMap` sits perfectly in the middle: safe, scalable, and fast. This is why `ConcurrentHashMap` has replaced `Hashtable` almost everywhere in modern Java.

#### 9.8.12 Real-World Usage in Production

`ConcurrentHashMap` is heavily used in:

* Web server / application-level caches
* Session storage
* Application-level counters (metrics, rate limiters)
* Spring Boot shared configuration/bean registries
* Rate limiting (token buckets keyed by client ID)
* Feature flags

Whenever multiple threads must safely read and write shared key-value data, `ConcurrentHashMap` is the **correct default choice**.

**Mental model:**
> **HashMap**: "Fast but dangerous"
> **Hashtable**: "Safe but slow"
> **ConcurrentHashMap**: "Safe *and* scalable"

> **Real-world relevance:** In a Spring `@Service` that's a singleton bean shared across every request thread, any mutable `Map` field storing shared state (an in-memory rate limiter, a request-deduplication cache, a metrics counter map) must be a `ConcurrentHashMap` (or a purpose-built cache library) — never a plain `HashMap`, since Spring singletons are, by definition, shared across concurrent request threads.

---

## 10. Comparable vs Comparator

### 10.1 Why Java Needs Comparable and Comparator

In Java, sorting is not automatic for custom objects. Java has no idea whether a `Student` should be sorted by name, GPA, age, or roll number unless **you explicitly tell it how to compare two objects**. This is where `Comparable` and `Comparator` come into play.

Both interfaces solve the same problem — **ordering objects** — but they solve it in very different ways. `Comparable` defines a **natural order** that belongs to the class itself, while `Comparator` defines **custom or multiple orders** that live outside the class. Understanding this distinction is critical for clean design and interview clarity.

### 10.2 Comparable: Defining a Natural Order Inside the Class

`Comparable<T>` is an interface used to define the **natural (default) ordering** of objects of a class. It is implemented **inside the class**, provides **one fixed sorting logic**, and is used automatically by Java's sorting APIs.

```java
public interface Comparable<T> {
    int compareTo(T other);
}
```

📌 Examples of natural ordering: `Integer` → numeric ascending; `String` → lexicographical order; `Student` → GPA / roll number / ID (as defined by the developer).

**Why Comparable is needed:** Java needs to know **how to compare objects** while sorting. For primitives & wrappers, this is already implemented. For custom classes, Java has no idea how to compare them — you must implement `Comparable`, or sorting throws a `ClassCastException`:

```java
students.sort(null); // ❌ ClassCastException if Student doesn't implement Comparable
```

**Example — Student sorted by GPA (descending), natural order:**

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

### 10.3 How compareTo() / compare() Return Values Work

Both `compareTo()` (Comparable) and `compare()` (Comparator) follow the same contract:

| Return Value | Meaning |
|---|---|
| `< 0` | first object comes **before** second |
| `0` | both are **equal** for sorting purposes (relative order preserved due to stable sort) |
| `> 0` | first object comes **after** second |

This contract is the backbone of all Java sorting mechanisms, including `Collections.sort()`, `list.sort()`, `TreeSet`, and `TreeMap`.

⚠️ **Avoid direct subtraction for comparisons** — `s1.length() - s2.length()` or `o.getGpa() - this.getGpa()` looks simple but is **dangerous**:

* For `int`, subtraction can silently **overflow** for large magnitude values (e.g., `Integer.MIN_VALUE - 1` wraps around), producing an incorrect sign.
* For `double`/`float`, subtraction causes **precision loss** and mishandles special values like `NaN` and `-0.0`.

✅ **Correct approach** — always use the type-safe static comparator methods:

```java
Integer.compare(a, b);
Double.compare(a, b);
Long.compare(a, b);
```

These safely compare values and handle edge cases correctly. Mentioning this in an interview is a strong signal of Java fundamentals maturity.

### 10.4 Comparable vs Comparator — Side-by-Side

| Comparable | Comparator |
|---|---|
| Defines **natural order** | Defines **custom order** |
| Implemented **inside** the class | Implemented **externally** |
| `compareTo()` — single-argument (`this` vs `other`) | `compare()` — two-argument (`o1` vs `o2`) |
| Only **one** sorting logic per class | **Multiple** sorting logics possible |
| Modifies the class | No class modification needed |
| Objects compared: `this` vs `other` | Objects compared: `o1` vs `o2` |

👉 **Interview rule:**
* **Comparable** → "What is the *default* order?"
* **Comparator** → "How *else* can I sort?"

If a class has one obvious natural identity (e.g., `Integer`, `String`), use `Comparable`. If sorting depends on context (e.g., sort a `List<Student>` by GPA in one screen and by name in another), use `Comparator`.

### 10.5 How Sorting Is Applied

```java
Collections.sort(list);            // uses Comparable (natural order)
Collections.sort(list, null);      // same as above — null means "use natural order"
Collections.sort(list, comparator); // uses the given Comparator
list.sort(comparator);              // Java 8+ instance method
```

👉 Sorting algorithm repeatedly calls `compare()`/`compareTo()` until the list is ordered.

### 10.6 Implementing Comparator — All Approaches

**A) Separate Comparator Class**

```java
class StringLengthComparator implements Comparator<String> {
    @Override
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
}
```

📌 Best when logic is complex or reused in multiple places.

**B) Anonymous Class**

```java
words.sort(new Comparator<String>() {
    public int compare(String s1, String s2) {
        return Integer.compare(s1.length(), s2.length());
    }
});
```

📌 Less common after Java 8.

**C) Lambda Expression (Java 8+)**

```java
list.sort((a, b) -> Integer.compare(a.length(), b.length())); // Asc
list.sort((a, b) -> Integer.compare(b.length(), a.length())); // Desc
```

📌 Works because `Comparator` is a **functional interface** (single abstract method `compare()`); the lambda supplies its implementation.

### 10.7 Real-World Example: Student GPA Sorting

**Requirement:** Sort by GPA descending; if GPA ties, sort by name ascending.

**❌ Bad approach (risky):**

```java
(int)(s2.getGpa() - s1.getGpa()); // precision loss, wrong ordering for close values
```

**✅ Correct approach:**

```java
students.sort((s1, s2) -> {
    int gpaCompare = Double.compare(s2.getGpa(), s1.getGpa());
    if (gpaCompare != 0) return gpaCompare;
    return s1.getName().compareTo(s2.getName());
});
```

### 10.8 Java 8 Comparator Utility Methods (Very Important for Production Code)

**Single field sorting:**

```java
Comparator<Student> byGpa = Comparator.comparing(Student::getGpa);
```

**Descending order:**

```java
Comparator<Student> byGpaDesc = Comparator.comparing(Student::getGpa).reversed();
```

**Multi-field sorting (chaining):**

```java
Comparator<Student> byGpaThenName =
    Comparator.comparing(Student::getGpa)
              .reversed()
              .thenComparing(Student::getName);
```

📌 Explanation: `comparing()` → primary sort key; `thenComparing()` → tie-breaker; `reversed()` → flips the previous comparator. This reads almost like English and is by far the **most common real-world style** in Spring Boot / backend projects.

**Primitive-specific comparators (best practice — avoid boxing overhead):**

| Type | Method |
|---|---|
| int | `Comparator.comparingInt()` |
| double | `Comparator.comparingDouble()` |
| long | `Comparator.comparingLong()` |

```java
students.sort(
    Comparator.comparingDouble(Student::getGpa).reversed()
);
```

**Null-safe comparators:**

```java
Comparator.nullsFirst(Comparator.naturalOrder());
Comparator.nullsLast(Comparator.naturalOrder());
```

### 10.9 Stable Sorting (Important Concept)

Java's `Collections.sort()` and `List.sort()` use **TimSort** (a hybrid of merge sort and insertion sort), which is **stable**.

📌 Meaning: if `compare()` returns `0` for two elements, their **original relative order is preserved** in the output.

✔ Critical for **multi-level sorting** — e.g., sort by GPA first, then by name: elements with the same GPA retain their pre-existing relative order among themselves before the name tie-breaker is even needed.
✔ Frequently asked in interviews: *"Is Java's sort stable, and why does it matter?"*

### 10.10 Multi-Field Sorting with Comparable (Tie-Breakers Inside compareTo)

Comparable allows only **one** `compareTo()` method, but the logic inside it can include tie-breakers:

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

📌 Primary → GPA (desc); Secondary → Name (asc).

### 10.11 Comparable in Sorted Collections

`Comparable` (or an explicit `Comparator`) is used automatically by:

| Collection | Purpose |
|---|---|
| `TreeSet` | Sorted unique elements |
| `TreeMap` | Sorted keys |
| `PriorityQueue` | Priority ordering |

📌 If `compareTo()` is implemented incorrectly, these data structures behave incorrectly (elements silently dropped, wrong ordering, etc.).

### 10.12 The equals() and compareTo() Contract (Critical Gotcha)

⚠️ **VERY IMPORTANT.** If `compareTo() == 0`, then ideally `equals()` should also return `true`. If this contract is violated:

* `TreeSet`/`TreeMap` may silently **drop elements it considers "equal" via `compareTo()`**, even if `equals()` says they're different — this is the same trap noted in the Set chapter (section 7.3) when a sorting `Comparator` is mistakenly reused for uniqueness checks.
* `TreeMap` may **overwrite keys** that `equals()` would have treated as distinct.

👉 Best practice: keep `compareTo()`/`compare()` consistent with `equals()` and `hashCode()`, or be very deliberate when you intentionally want "equal by comparator" to mean "duplicate" (e.g., deliberately deduplicating by a derived field using a `TreeSet` with a custom `Comparator`).

### 10.13 When NOT to Use Comparable

❌ When multiple sorting orders are required, when sorting logic changes frequently, or when you don't own the class (e.g., a third-party library DTO) — you cannot add `Comparable` to a class you don't control.

✅ Use **Comparator** instead in all these cases.

### 10.14 Quick Reference — Rules Cheat Sheet

| Requirement | Correct Approach |
|---|---|
| Ascending int | `Integer.compare(a, b)` |
| Descending int | `Integer.compare(b, a)` |
| Ascending double | `Double.compare(a, b)` |
| Descending double | `Double.compare(b, a)` |
| Ascending String | `a.compareTo(b)` |
| Descending String | `b.compareTo(a)` |
| Multi-field | `Comparator.comparing(...).thenComparing(...)` |
| Null-safe | `Comparator.nullsFirst(...)` / `nullsLast(...)` |

### 10.15 Common Interview Pitfalls

❌ Using subtraction for comparison (overflow/precision bugs)
❌ Forgetting secondary sorting (unstable-looking results on ties)
❌ Mixing Comparable & Comparator incorrectly (e.g., expecting `sort(null)` to work without implementing `Comparable`)
❌ Ignoring null handling (use `Comparator.nullsFirst()`/`nullsLast()`)
❌ Violating the `compareTo()`/`equals()` consistency contract when using `TreeSet`/`TreeMap`

### 10.16 Interview One-Line Summary

> **Comparable** defines *what the natural order is*, implemented inside the class via `compareTo()`, supporting exactly one sorting strategy. **Comparator** defines *how else you can sort*, implemented externally via `compare()`, supporting unlimited sorting strategies, lambda expressions, and method chaining (`comparing().thenComparing().reversed()`) in Java 8+.

> **Real-world relevance:** In real codebases, `Comparator.comparing(...).thenComparing(...)` chains are everywhere — sorting API response DTOs by multiple criteria for a UI table, ordering log entries by severity-then-timestamp, or ranking search results by relevance-then-recency. `Comparable` is reserved for domain objects with one obvious, unambiguous natural order (e.g., a `Money` value object sorted by amount, or a `Version` class sorted semantically).

---

## 11. Collections Utility Class

`java.util.Collections` is a class of **static utility methods** that operate on or return collections. It is easy to confuse with the `Collection` interface, but they are completely different things: `Collection` is a type; `Collections` is a toolbox of helper methods, conceptually similar to how `Arrays` is a toolbox for arrays (see section 12).

**Key method categories:**

**Sorting & searching:**

```java
Collections.sort(list);
Collections.sort(list, comparator);
Collections.binarySearch(sortedList, key); // list MUST already be sorted
```

**Reversing, shuffling, rotating:**

```java
Collections.reverse(list);
Collections.shuffle(list);          // useful for randomized test data, lottery-style selection
Collections.rotate(list, 2);
```

**Min/Max:**

```java
Collections.max(list);
Collections.min(list, comparator);
```

**Thread-safe wrappers (synchronized views):**

```java
List<Integer> syncList = Collections.synchronizedList(new ArrayList<>());
Map<K, V> syncMap = Collections.synchronizedMap(new HashMap<>());
Set<E> syncSet = Collections.synchronizedSet(new HashSet<>());
```

⚠️ These wrap every method call in a lock on the collection object, but **iteration must still be manually synchronized** by the caller:

```java
synchronized (syncList) {
    for (Integer i : syncList) { ... } // required to avoid CME
}
```

**Unmodifiable (read-only) views:**

```java
List<Integer> readOnly = Collections.unmodifiableList(list);
```

📌 This does **not** create a deep copy — it's a **view**. If the underlying `list` is modified directly, changes are reflected in `readOnly` too, and any write attempt through `readOnly` itself throws `UnsupportedOperationException`. This is different from the truly immutable `List.of()` (section 13).

**Empty / singleton collections (avoids unnecessary allocation):**

```java
Collections.emptyList();
Collections.singletonList("only-one");
Collections.emptyMap();
```

**Frequency & disjoint checks:**

```java
Collections.frequency(list, "apple"); // count of occurrences
Collections.disjoint(listA, listB);   // true if no common elements
```

> **Real-world relevance:** `Collections.unmodifiableList()` is the classic pattern for safely exposing an internal mutable field from a getter — e.g., `return Collections.unmodifiableList(this.items);` — so callers can read but not accidentally mutate your internal state (a common code-review flag when a getter returns the raw mutable field directly). `Collections.emptyList()`/`emptyMap()` avoid needless allocations and are the idiomatic "return nothing" for methods with a `List`/`Map` return type instead of returning `null` (which forces every caller to null-check).

---

## 12. Arrays vs Collections

Just as `Collections` is the utility class for the `Collection` framework, **`java.util.Arrays`** is the equivalent utility class for raw arrays. Both provide static helper methods, but they operate on fundamentally different data structures.

| Utility | Operates On | Example |
|---|---|---|
| `Arrays` | `T[]`, primitive arrays | `Arrays.sort(arr)`, `Arrays.asList(arr)`, `Arrays.binarySearch(arr, key)`, `Arrays.fill(arr, 0)`, `Arrays.equals(a, b)` |
| `Collections` | `Collection` / `List` / `Set` / `Map` | `Collections.sort(list)`, `Collections.binarySearch(list, key)`, `Collections.unmodifiableList(list)` |

**A notorious gotcha — `Arrays.asList()`:**

```java
Integer[] arr = {1, 2, 3};
List<Integer> list = Arrays.asList(arr);

list.set(0, 99);   // ✅ OK — modifies the underlying array
list.add(4);       // ❌ UnsupportedOperationException
```

`Arrays.asList()` returns a **fixed-size list backed directly by the array** — it is not a true `ArrayList`, and structural modifications (`add`/`remove`) are unsupported because that would require resizing the backing array, which isn't possible. This is one of the most common sources of a surprising `UnsupportedOperationException` in production code, especially when a method further down the call chain tries to `add()` to what looks like an ordinary `List`.

```java
// Correct way to get a真正 mutable, independent ArrayList from an array:
List<Integer> mutable = new ArrayList<>(Arrays.asList(arr));
```

**Converting between the two:**

```java
// Array → List
List<String> list = Arrays.asList("a", "b", "c");       // fixed-size view
List<String> mutableList = new ArrayList<>(List.of("a", "b")); // fully mutable copy

// List → Array
String[] arr = list.toArray(new String[0]);
```

> **Real-world relevance:** `Arrays.asList()` shows up constantly as a quick way to seed test data or construct a small fixed list inline (`Arrays.asList("A", "B", "C")`), but engineers frequently get bitten when that "list" is later passed into code that calls `.add()`/`.remove()` on it. Always wrap with `new ArrayList<>(...)` if the list needs to be mutable downstream.

---

## 13. Immutable Collections (Java 9+)

Java 9 introduced convenient static factory methods for creating genuinely **immutable** collections — a big improvement over the more verbose `Collections.unmodifiableList(new ArrayList<>(...))` pattern.

```java
List<Integer> list = List.of(1, 2, 3);
Set<String> set = Set.of("a", "b", "c");
Map<String, Integer> map = Map.of("x", 1, "y", 2);

list.add(4);   // ❌ UnsupportedOperationException
```

**Key differences from `Collections.unmodifiableXxx()`:**

| Aspect | `Collections.unmodifiableList(list)` | `List.of(...)` |
|---|---|---|
| Backing data | View over an existing (possibly mutable) list | Genuinely immutable, self-contained storage |
| Underlying mutation visible? | ✅ Yes — if the source list changes, the view changes too | ❌ No — no underlying mutable list exists at all |
| Null elements | Depends on source list | ❌ Throws `NullPointerException` if any element/key/value is null |
| Performance | Extra indirection layer | Often more memory/CPU efficient (specialized implementations for 0, 1, 2, and N elements) |

`Set.of()` and `Map.of()` also **throw `IllegalArgumentException`** if duplicate elements/keys are passed, unlike `HashSet`/`HashMap`, which silently ignore/overwrite duplicates — a subtle but important behavioral difference to know.

For larger, pre-built collections, `Map.ofEntries()` reads more cleanly than a long `Map.of(k1, v1, k2, v2, ...)` chain:

```java
Map<String, Integer> map = Map.ofEntries(
    Map.entry("a", 1),
    Map.entry("b", 2)
);
```

> **Real-world relevance:** Immutable collections are the correct choice for constants, configuration defaults, enum-like fixed value sets, and any method parameter/return value that should never be mutated by the caller — e.g., `private static final List<String> ALLOWED_ROLES = List.of("ADMIN", "USER", "AUDITOR");`. They also communicate intent clearly to other engineers reading the code: "this collection is fixed, don't try to mutate it," which is more explicit and safer than a defensively-wrapped mutable collection.

---

## 14. Streams Synergy with Collections

While the Stream API (`java.util.stream`) is a separate framework from JCF, in real backend code the two are used together constantly — Streams consume a `Collection` as a source and typically produce a new `Collection` as output via **collectors**.

**Collection → Stream → Collection:**

```java
List<String> names = employees.stream()
    .filter(e -> e.getSalary() > 50000)
    .map(Employee::getName)
    .sorted()
    .collect(Collectors.toList());
```

**Building a Map from a List (a very common real-world pattern — indexing):**

```java
Map<Long, Employee> byId = employees.stream()
    .collect(Collectors.toMap(Employee::getId, Function.identity()));
```

⚠️ `Collectors.toMap()` throws `IllegalStateException` on duplicate keys unless you supply a merge function:

```java
Collectors.toMap(Employee::getDept, Function.identity(), (a, b) -> a); // keep first on conflict
```

**Grouping (backed internally by `HashMap<K, List<V>>`):**

```java
Map<String, List<Employee>> byDept = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment));

// Grouping + downstream aggregation
Map<String, Long> countByDept = employees.stream()
    .collect(Collectors.groupingBy(Employee::getDepartment, Collectors.counting()));
```

**Partitioning (always produces exactly 2 buckets, backed by a `Map<Boolean, List<T>>`):**

```java
Map<Boolean, List<Employee>> partitioned = employees.stream()
    .collect(Collectors.partitioningBy(e -> e.getSalary() > 50000));
```

**Producing other collection types explicitly:**

```java
Set<String> uniqueNames = employees.stream()
    .map(Employee::getName)
    .collect(Collectors.toSet()); // backed by HashSet — no order guarantee

List<String> immutableNames = employees.stream()
    .map(Employee::getName)
    .collect(Collectors.toUnmodifiableList()); // Java 10+
```

**Primitive streams avoid boxing overhead for aggregation:**

```java
double totalSalary = employees.stream()
    .mapToDouble(Employee::getSalary)
    .sum();
```

> **Real-world relevance:** `Collectors.groupingBy()` / `toMap()` are the standard tools for turning a flat DB query result (`List<Row>`) into an indexed or grouped in-memory structure for a service layer — e.g., grouping orders by customer, indexing a product catalog by SKU for O(1) lookup instead of repeated linear scans, or building a `Map<String, List<Permission>>` grouped by role from a flat permissions table. Understanding which concrete collection each collector produces (and its ordering/uniqueness/null guarantees) is essential to avoid subtle bugs, like assuming `groupingBy()` preserves insertion order (`HashMap`-backed, by default it does not — use `Collectors.groupingBy(fn, LinkedHashMap::new, ...)` if order matters).

---

## 15. Thread-Safety Cheat Sheet

| Need | Non-Thread-Safe | Thread-Safe Alternative | Notes |
|---|---|---|---|
| List, read-heavy | `ArrayList` | `CopyOnWriteArrayList` | Writes are O(n); best for rarely-mutated shared lists |
| List, write-heavy / general sync | `ArrayList` | `Collections.synchronizedList(new ArrayList<>())` | Manual sync required for iteration |
| Set, read-heavy | `HashSet` | `CopyOnWriteArraySet` | Same trade-off as above |
| Sorted Set, concurrent | `TreeSet` | `ConcurrentSkipListSet` | Lock-free, high concurrency, sorted |
| Map, general concurrent use | `HashMap` | `ConcurrentHashMap` | Default choice for shared maps — fine-grained locking |
| Sorted Map, concurrent | `TreeMap` | `ConcurrentSkipListMap` | Lock-free, sorted, concurrent |
| Legacy fully-synchronized map | — | `Hashtable` | Avoid in new code; use `ConcurrentHashMap` |
| Queue, producer-consumer, bounded | — | `ArrayBlockingQueue` | Blocking, fixed capacity, backpressure |
| Queue, producer-consumer, high concurrency | — | `LinkedBlockingQueue` | Two-lock design, effectively unbounded by default |
| Queue, non-blocking concurrent FIFO | — | `ConcurrentLinkedQueue` | Lock-free, unbounded, no blocking semantics |
| Priority queue, concurrent | `PriorityQueue` | `PriorityBlockingQueue` | Thread-safe heap-based priority queue |

> **Real-world relevance:** This table is the practical decision-making tool for code review: seeing a plain `HashMap` or `ArrayList` as a field on a Spring singleton `@Service`/`@Component` bean that's mutated after construction should immediately raise the question "is this thread-safe?" — and usually the answer is "swap it for the appropriate row in this table."

---

## 16. Real-World Collection Selection Guide

| Scenario | Recommended Collection | Why |
|---|---|---|
| Paginated API response / ordered list of DTOs | `ArrayList` | Fast reads, preserves order, most common case |
| Undo/redo stack, DFS traversal | `ArrayDeque` (as stack) | O(1) push/pop, no synchronization overhead of `Stack` |
| Task scheduler, BFS traversal, thread-pool queue | `ArrayDeque` / `LinkedBlockingQueue` | FIFO semantics, cache-friendly |
| Deduplicating a batch of IDs before a query | `HashSet` | O(1) uniqueness check |
| Preserving first-seen order while deduplicating | `LinkedHashSet` | Order + uniqueness |
| Leaderboard / always-sorted unique values | `TreeSet` | Sorted + unique, O(log n) ops |
| Fast key-based lookup / index / cache | `HashMap` | O(1) average lookup |
| LRU cache | `LinkedHashMap` (access-order + `removeEldestEntry`) | Built-in eviction support |
| Config active "as of" a timestamp / nearest-match lookup | `TreeMap` | `floorEntry()`/`ceilingEntry()` |
| Per-enum-constant aggregation (counts, config, handlers) | `EnumMap` / `EnumSet` | Array-backed, fastest, most memory-efficient |
| Shared mutable map across request threads | `ConcurrentHashMap` | Thread-safe, scalable |
| Rarely-mutated shared list read by many threads (listeners, flags) | `CopyOnWriteArrayList` / `CopyOnWriteArraySet` | Lock-free reads |
| Task/job priority processing | `PriorityQueue` / `PriorityBlockingQueue` | Heap-based priority ordering |
| Producer-consumer pipeline with backpressure | `ArrayBlockingQueue` | Bounded, blocking |
| Constant / fixed configuration values | `List.of()` / `Set.of()` / `Map.of()` | Genuinely immutable, communicates intent |
| Grouping/aggregating query results in service layer | `Collectors.groupingBy()` / `toMap()` | Declarative, concise |

---
