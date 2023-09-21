# TreeSet

TreeSet is basically an implementation of a self-balancing binary search tree like a [[红黑树|Red-Black Tree]]. The elements in TreeSet are **sorted**.

to init a TreeSet:
```java
TreeSet treeSet = new TreeSet();

// specify sorting order
TreeSet treeSet = new TreeSet(Comparator comp);

// build a TreeSet with elements from a collection
TreeSet treeSet = new TreeSet(Collection col);
```

methods of TreeSet：

- `add(Object o)`: shouldn't add `null` to a TreeSet, otherwise it will throw `NullPointException`.
- `addAll(Collection co)`
- `contains(Object o)`
- `ceiling(E e)`: This method returns the least element in this set greater than or equal to the given element, or null if there is no such element.
- `floor(E e)`: This method returns the greatest element in this set less than or equal to the given element, or null if there is no such element.
- `higher(E e)`: This method returns the least element in this set strictly greater than the given element, or null if there is no such element.
- `first()`: his method will return the first element in TreeSet if TreeSet is not null else it will throw `NoSuchElementException`.
- `last()`: This method will return the last element in TreeSet if TreeSet is not null else it will throw `NoSuchElementException`.
- `clear()`
- etc.

## Time complexity

The [[时间复杂度|Time complexity]] for TreeSet's methods `add`, `remove` and `contains` is `O(logn)`.

## NOT Thread-safe

 The implementation of a TreeSet is not synchronized. To make TreeSet be Thread-safe, it can be inited likes:

```java
TreeSet ts = new TreeSet(); 
Set syncSet = Collections.synchronziedSet(ts);
```