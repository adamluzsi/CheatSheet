# HashSet Vs TreeSet Vs LinkedHashSet

Even though, HashSet, LinkedHashSet and TreeSet are all implementations of Set interface, there are some differences exist between them.
In this article, I have tried to list out the differences between HashSet, LinkedHashSet and TreeSet in java.
They also have some similarities between them. We will also discuss them at the end.

## Differences Between Set And List

List is an ordered sequence of elements whereas Set is a distinct list of elements which is not necessarily ordered

### List

```java
List<E>:
```
An ordered collection (also known as a sequence).
The user of this interface has precise control over where in the list each element is inserted.
The user can access elements by their integer index (position in the list), and search for elements in the list.

### Set

```java
Set<E>:
```
A collection that contains no duplicate elements.
More formally, sets contain no pair of elements e1 and e2 such that e1.equals(e2), and at most one null element.
As implied by its name, this interface models the mathematical set abstraction.

## Differences Between HashSet, LinkedHashSet and TreeSet

||HashSet|LinkedHashSet|TreeSet|
|-|-------|-------------|-------|
|How they work internally?|HashSet uses HashMap internally to store it’s elements.|LinkedHashSet uses  LinkedHashMap internally to store it’s elements.|TreeSet uses TreeMap internally to store it’s elements.|
|Order Of Elements|HashSet doesn’t maintain any order of elements.|LinkedHashSet maintains insertion order of elements. i.e elements are placed as they are inserted.|TreeSet orders the elements according to supplied Comparator. If no comparator is supplied, elements will be placed in their natural ascending order.|
|Performance|HashSet gives better performance than the LinkedHashSet and TreeSet.|The performance of LinkedHashSet is between HashSet and TreeSet. It’s performance is almost similar to HashSet. But slightly in the slower side as it also maintains LinkedList internally to maintain the insertion order of elements.|TreeSet gives less performance than the HashSet and LinkedHashSet as it has to sort the elements after each insertion and removal operations.|
|Insertion, Removal And Retrieval Operations|HashSet gives performance of order O(1) for insertion, removal and retrieval operations.|LinkedHashSet also gives performance of order O(1) for insertion, removal and retrieval operations.|TreeSet gives performance of order O(log(n)) for insertion, removal and retrieval operations.|
|How they compare the elements?|HashSet uses equals() and hashCode() methods to compare the elements and thus removing the possible duplicate elements.|LinkedHashSet also uses equals() and hashCode() methods to compare the elements.|TreeSet uses compare() or compareTo() methods to compare the elements and thus removing the possible duplicate elements. It doesn’t use equals() and hashCode() methods for comparision of elements.|
|Null elements|HashSet allows maximum one null element.|LinkedHashSet also allows maximum one null element.|TreeSet doesn’t allow even a single null element. If you try to insert null element into TreeSet, it throws NullPointerException.|
|Memory Occupation|HashSet requires less memory than LinkedHashSet and TreeSet as it uses only HashMap internally to store its elements.|LinkedHashSet requires more memory than HashSet as it also maintains LinkedList along with HashMap to store its elements.|TreeSet also requires more memory than HashSet as it also maintains Comparator to sort the elements along with the TreeMap.|
|When To Use?|Use HashSet if you don’t want to maintain any order of elements.|Use LinkedHashSet if you want to maintain insertion order of elements.|Use TreeSet if you want to sort the elements according to some Comparator.|

## Similarities Between HashSet, LinkedHashSet and TreeSet In Java
All three doesn’t allow duplicate elements.
All three are not synchronized.
All three are Cloneable and Serializable.
Iterator returned by all three is fail-fast in nature. i.e You will get ConcurrentModificationException if they are modified after the creation of Iterator object.

## Additional usage

Example code based on
```java
Set<Integer> squares = new HashSet<>();
Set<Integer> cubes = new HashSet<>();

for (int i = 1; i <= 100; i++) {
    squares.add(i * i);
    cubes.add(i * i * i);
}
```

### Union

Adds all of the elements in the specified collection to this set if
they're not already present (optional operation).  If the specified
collection is also a set, the {@code addAll} operation effectively
modifies this set so that its value is the <i>union</i> of the two
sets.  The behavior of this operation is undefined if the specified
collection is modified while the operation is in progress.

squares ∪ cubes
```java
Set<Integer> union = new HashSet<>(squares);
union.addAll(cubes);
```

### Difference

squares - cubes
```java
Set<Integer> diff = new HashSet<>(squares);
diff.removeAll(cubes);
```

### Intersection

Retains only the elements in this set that are contained in the
specified collection (optional operation).  In other words, removes
from this set all of its elements that are not contained in the
specified collection.  If the specified collection is also a set, this
operation effectively modifies this set so that its value is the
intersection of the two sets.

entries that both squares and cubes own

squares ∩ cubes
```java
Set<Integer> intersection = new HashSet<>(squares);
intersection.retainAll(cubes);
```
