BlockingQueue
=============

The Java `BlockingQueue` interface in the `java.util.concurrent` package represents a queue which is thread safe to put into, and take instances from. In this text I will show you how to use this `BlockingQueue`.

This text will not discuss how to implement a `BlockingQueue` in Java yourself.

Cheat Sheet table
-----------------

| Class               | Use Case                                                                                                                                                                                                                                          |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SynchronousQueue    | Ensures that no data is in the "queue" so it's up to the sender to how should be handled an error during an interrupt and what must be done in order to rollback safely                                                                           |
| SynchronousQueue    | Implements the CSP handoff sematics, no memory usage cause the object is never stored in the "Queue", but passed to the consumer                                                                                                                  |
| SynchronousQueue    | It's fast in term of throughput, but requires one publisher and one consumer to meet at a rendezvous point                                                                                                                                        |
| SynchronousQueue    | can support optionally fairness policy for FIFO in/out                                                                                                                                                                                            |
| ArrayBlockingQueue  | can be created with a configurable (on/off) scheduling fairness policy. This is great if you need fairness or want to avoid producer/consumer starvation, but it will cost you in throughput.                                                     |
| ArrayBlockingQueue  | pre-allocates its backing array, so it doesn't allocate nodes during its usage, but it immediately takes what can be a considerable chunk of memory, which can be a problem if your memory is fragmented.                                         |
| ArrayBlockingQueue  | should have less variability in performance, because it has less moving parts overall, it uses a simpler and less-sophisticated single-lock algorithm, it does not create nodes during usage, and its cache behavior should be fairly consistent. |
| LinkedBlockingQueue | should have better throughput, because it uses separate locks for the head and the tail.                                                                                                                                                          |
| LinkedBlockingQueue | does not pre-allocate nodes, which means that its memory footprint will roughly match its size, but it also means that it will incur some work for allocation and freeing of nodes.                                                               |
| LinkedBlockingQueue | will probably have worse cache behavior, which may affect its own performance, but also the performance of other components due to false sharing.                                                                                                 |


BlockingQueue Usage
-------------------

A `BlockingQueue` is typically used to have on thread produce objects, which another thread consumes. Here is a diagram that illustrates this principle:

![A BlockingQueue with one thread putting into it, and another thread taking from it.](BlockingQueue/blocking-queue.png)

**A BlockingQueue with one thread putting into it, and another thread taking from it.**

The producing thread will keep producing new objects and insert them into the queue, until the queue reaches some upper bound on what it can contain. It's limit, in other words. If the blocking queue reaches its upper limit, the producing thread is blocked while trying to insert the new object. It remains blocked until a consuming thread takes an object out of the queue.

The consuming thread keeps taking objects out of the blocking queue, and processes them. If the consuming thread tries to take an object out of an empty queue, the consuming thread is blocked until a producing thread puts an object into the queue.

### BlockingQueue Methods

A `BlockingQueue` has 4 different sets of methods for inserting, removing and examining the elements in the queue. Each set of methods behaves differently in case the requested operation cannot be carried out immediately. Here is a table of the methods:

||Throws Exception|Special Value|Blocks|Times Out|
|-|---------------|-------------|------|---------|
|Insert|add(o)|offer(o)|put(o)|offer(o, timeout, timeunit)|
|Remove|remove(o)|poll()|take()|poll(timeout, timeunit)|
|Examine|element()|peek()|||

The 4 different sets of behaviour means this:

1.  **Throws Exception**:
    If the attempted operation is not possible immediately, an exception is thrown.
2.  **Special Value**:
    If the attempted operation is not possible immediately, a special value is returned (often true / false).
3.  **Blocks**:
    If the attempted operation is not possible immedidately, the method call blocks until it is.
4.  **Times Out**:
    If the attempted operation is not possible immedidately, the method call blocks until it is, but waits no longer than the given timeout. Returns a special value telling whether the operation succeeded or not (typically true / false).

It is not possible to insert `null` into a `BlockingQueue`. If you try to insert null, the `BlockingQueue` will throw a `NullPointerException`.

It is also possible to access all the elements inside a `BlockingQueue`, and not just the elements at the start and end. For instance, say you have queued an object for processing, but your application decides to cancel it. You can then call e.g. `remove(o)` to remove a specific object in the queue. However, this is not done very efficiently, so you should not use these `Collection` methods unless you really have to.

BlockingQueue Implementations
-----------------------------

Since `BlockingQueue` is an interface, you need to use one of its implementations to use it. The `java.util.concurrent` package has the following implementations of the `BlockingQueue` interface (in Java 6):

*   [ArrayBlockingQueue](arrayblockingqueue.html)
*   [DelayQueue](delayqueue.html)
*   [LinkedBlockingQueue](linkedblockingqueue.html)
*   [PriorityBlockingQueue](priorityblockingqueue.html)
*   [SynchronousQueue](synchronousqueue.html)

Click the links in the list to read more about each implementation. If a link cannot be clicked, that implementation has not yet been described. Check back again in the future, or check out the JavaDoc's for more detail.

Java BlockingQueue Example
--------------------------

Here is a Java `BlockingQueue` example. The example uses the `ArrayBlockingQueue` implementation of the `BlockingQueue` interface.

First, the `BlockingQueueExample` class which starts a `Producer` and a `Consumer` in separate threads. The `Producer` inserts strings into a shared `BlockingQueue`, and the `Consumer` takes them out.

```java
public class BlockingQueueExample {

    public static void main(String\[\] args) throws Exception {

        BlockingQueue queue = new ArrayBlockingQueue(1024);

        Producer producer = new Producer(queue);
        Consumer consumer = new Consumer(queue);

        new Thread(producer).start();
        new Thread(consumer).start();

        Thread.sleep(4000);
    }
}
```

Here is the `Producer` class. Notice how it sleeps a second between each `put()` call. This will cause the `Consumer` to block, while waiting for objects in the queue.

```java
public class Producer implements Runnable{

    protected BlockingQueue queue = null;

    public Producer(BlockingQueue queue) {
        this.queue = queue;
    }

    public void run() {
        try {
            queue.put("1");
            Thread.sleep(1000);
            queue.put("2");
            Thread.sleep(1000);
            queue.put("3");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

Here is the `Consumer` class. It just takes out the objects from the queue, and prints them to `System.out`.

```java
public class Consumer implements Runnable{

    protected BlockingQueue queue = null;

    public Consumer(BlockingQueue queue) {
        this.queue = queue;
    }

    public void run() {
        try {
            System.out.println(queue.take());
            System.out.println(queue.take());
            System.out.println(queue.take());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```