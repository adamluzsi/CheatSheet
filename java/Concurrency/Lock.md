Lock
====

*   [Java Lock Example](#java-lock-example)
*   [Java Lock Implementations](#java-lock-implementations)
*   [Main Differences Between Locks and Synchronized Blocks](#main-differences-between-locks-and-synchronized-blocks)
*   [Lock Methods](#lock-methods)






A `java.util.concurrent.locks.Lock` is a thread synchronization mechanism just like `synchronized` blocks. A `Lock` is, however, more flexible and more sophisticated than a synchronized block.

By the way, in my [Java Concurrency tutorial](/java-concurrency/index.html) I have described how to implement your own locks, in case you are interested (or need it). See my text on [Locks](/java-concurrency/locks.html) for more details.

Java Lock Example
-----------------

Since `Lock` is an interface, you need to use one of its implementations to use a `Lock` in your applications. Here is a simple usage example:

Lock lock = new ReentrantLock();

lock.lock();

//critical section

lock.unlock();

First a `Lock` is created. Then it's `lock()` method is called. Now the `Lock` instance is locked. Any other thread calling `lock()` will be blocked until the thread that locked the lock calls `unlock()`. Finally `unlock()` is called, and the `Lock` is now unlocked so other threads can lock it.

Java Lock Implementations
-------------------------

The `java.util.concurrent.locks` package has the following implementations of the `Lock` interface:

*   ReentrantLock

Main Differences Between Locks and Synchronized Blocks
------------------------------------------------------

The main differences between a `Lock` and a synchronized block are:

*   A synchronized block makes no guarantees about the sequence in which threads waiting to entering it are granted access.
*   You cannot pass any parameters to the entry of a synchronized block. Thus, having a timeout trying to get access to a synchronized block is not possible.
*   The synchronized block must be fully contained within a single method. A `Lock` can have it's calls to `lock()` and `unlock()` in separate methods.

Lock Methods
------------

The `Lock` interface has the following primary methods:

*   lock()
*   lockInterruptibly()
*   tryLock()
*   tryLock(long timeout, TimeUnit timeUnit)
*   unlock()

The `lock()` method locks the `Lock` instance if possible. If the `Lock` instance is already locked, the thread calling `lock()` is blocked until the `Lock` is unlocked.

The `lockInterruptibly()` method locks the `Lock` unless the thread calling the method has been interrupted. Additionally, if a thread is blocked waiting to lock the `Lock` via this method, and it is interrupted, it exits this method calls.

The `tryLock()` method attempts to lock the `Lock` instance immediately. It returns `true` if the locking succeeds, false if `Lock` is already locked. This method never blocks.

The `tryLock(long timeout, TimeUnit timeUnit)` works like the `tryLock()` method, except it waits up the given timeout before giving up trying to lock the `Lock`.

The `unlock()` method unlocks the `Lock` instance. Typically, a `Lock` implementation will only allow the thread that has locked the `Lock` to call this method. Other threads calling this method may result in an unchecked exception (`RuntimeException`).






