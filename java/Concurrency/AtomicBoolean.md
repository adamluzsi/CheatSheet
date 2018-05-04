AtomicBoolean
=============

*   [Creating an AtomicBoolean](#creating-an-atomicboolean)
*   [Getting the AtomicBoolean's Value](#getting-the-atomicboolean-value)
*   [Setting the AtomicBoolean's Value](#setting-the-atomicboolean-value)
*   [Swapping the AtomicBoolean's Value](#swapping-the-atomicboolean-value)
*   [Compare and Set AtomicBoolean's Value](#compare-and-set-atomicboolean-value)


The `AtomicBoolean` class provides you with a `boolean` variable which can be read and written atomically, and which also contains advanced atomic operations like `compareAndSet()`. The `AtomicBoolean` class is located in the `java.util.concurrent.atomic` package, so the full class name is `java.util.concurrent.atomic.AtomicBoolean` . This text describes the version of `AtomicBoolean` found in Java 8, but the first version was added in Java 5.

The reasoning behind the `AtomicBoolean` design is explained in my Java Concurrency tutorial in the text about [Compare and Swap](/java-concurrency/compare-and-swap.html).

Creating an AtomicBoolean
-------------------------

You create an `AtomicBoolean` like this:

AtomicBoolean atomicBoolean = new AtomicBoolean();

This example creates a new `AtomicBoolean` with the value `false`;

If you need to set an explicit initial value for the `AtomicBoolean` instance, you can pass the initial value to the `AtomicBoolean` constructor like this:

AtomicBoolean atomicBoolean = new AtomicBoolean(**true**);

Getting the AtomicBoolean's Value
---------------------------------

You can get the value of an `AtomicBoolean` using the `get()` method. Here is an example:

AtomicBoolean atomicBoolean = new AtomicBoolean(true);

boolean value = atomicBoolean.get();

After executing this code the `value` variable will contain the value `true`.

Setting the AtomicBoolean's Value
---------------------------------

You can set the value of an `AtomicBoolean` using the `set()` method. Here is an example:

AtomicBoolean atomicBoolean = new AtomicBoolean(true);

atomicBoolean.set(false);

After executing this code the `AtomicBoolean` variable will contain the value `false`.

Swapping the AtomicBoolean's Value
----------------------------------

You can swap the value of an `AtomicBoolean` using the `getAndSet()` method. The `getAndSet()` method returns the `AtomicBoolean`'s current value, and sets a new value for it. Here is an example:

AtomicBoolean atomicBoolean = new AtomicBoolean(true);

boolean oldValue = atomicBoolean.getAndSet(false);

After executing this code the `oldValue` variable will contain the value `true`, and the `AtomicBoolean` instance will contain the value `false`. The code effectively swaps the value `false` for the `AtomicBoolean`'s current value which is `true`.

Compare and Set AtomicBoolean's Value
-------------------------------------

The method `compareAndSet()` allows you to compare the current value of the `AtomicBoolean` to an expected value, and if current value is equal to the expected value, a new value can be set on the `AtomicBoolean`. The `compareAndSet()` method is atomic, so only a single thread can execute it at the same time. Thus, the `compareAndSet()` method can be used to implemented simple synchronizers like locks.

Here is a `compareAndSet()` example:

AtomicBoolean atomicBoolean = new AtomicBoolean(true);

boolean expectedValue = true;
boolean newValue      = false;

boolean wasNewValueSet = atomicBoolean.compareAndSet(
    expectedValue, newValue);

This example compares the current value of the `AtomicBoolean` to `true` and if the two values are equal, sets the new value of the `AtomicBoolean` to `false` .






