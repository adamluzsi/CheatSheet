ConcurrentNavigableMap
======================

*   [headMap()](#headmap)
*   [tailMap()](#tailmap)
*   [subMap()](#submap)
*   [More Methods](#more-methods)






The `java.util.concurrent.ConcurrentNavigableMap` class is a [java.util.NavigableMap](/java-collections/navigablemap.html) with support for concurrent access, and which has concurrent access enabled for its submaps. The "submaps" are the maps returned by various methods like `headMap()`, `subMap()` and `tailMap()`.

Rather than re-explain all methods found in the `NavigableMap` I will just look at the methods added by `ConcurrentNavigableMap`.

headMap()
---------

The `headMap(T toKey)` method returns a view of the map containing the keys which are strictly less than the given key.

If you make changes to the original map, these changes are reflected in the head map.

Here is an example illustrating the use of the `headMap()` method.

ConcurrentNavigableMap map = new ConcurrentSkipListMap();

map.put("1", "one");
map.put("2", "two");
map.put("3", "three");

ConcurrentNavigableMap headMap = map.headMap("2");

The `headMap` will point to a `ConcurrentNavigableMap` which only contains the key `"1"`, since only this key is strictly less than `"2"`.

See the JavaDoc for more specific details of how this method works, and how its overloaded versions work.

tailMap()
---------

The `tailMap(T fromKey)` method returns a view of the map containing the keys which are greater than or equal to the given `fromKey`.

If you make changes to the original map, these changes are reflected in the tail map.

Here is an example illustrating the use of the `tailMap()` method:

ConcurrentNavigableMap map = new ConcurrentSkipListMap();

map.put("1", "one");
map.put("2", "two");
map.put("3", "three");

ConcurrentNavigableMap tailMap = map.tailMap("2");

The `tailMap` will contain the keys `"2"` and `"3"` because these two keys are greather than or equal to the given key, `"2"`.

See the JavaDoc for more specific details of how this method works, and how its overloaded versions work.

subMap()
--------

The `subMap()` method returns a view of the original map which contains all keys from (including), to (excluding) two keys given as parameters to the method. Here is an example:

ConcurrentNavigableMap map = new ConcurrentSkipListMap();

map.put("1", "one");
map.put("2", "two");
map.put("3", "three");

ConcurrentNavigableMap subMap = map.subMap("2", "3");

The returned submap contains only the key `"2"`, because only this key is greater than or equal to `"2"`, and smaller than `"3"`.

More Methods
------------

The `ConcurrentNavigableMap` interface contains a few more methods that might be of use. For instance:

*   descendingKeySet()
*   descendingMap()
*   navigableKeySet()

See the official JavaDoc for more information on these methods.






