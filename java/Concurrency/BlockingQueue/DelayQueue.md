DelayQueue
==========

`DelayQueue` class implements the [`BlockingQueue`](blockingqueue.html) interface. Read the [`BlockingQueue`](blockingqueue.html) text for more information about the interface.

The `DelayQueue` blocks the elements internally until a certain delay has expired. The elements must implement the interface `java.util.concurrent.Delayed`. Here is how the interface looks:

public interface Delayed extends Comparable<Delayed< {

 public long getDelay(TimeUnit timeUnit);

}

The value returned by the `getDelay()` method should be the delay remaining before this element can be released. If 0 or a negative value is returned, the delay will be considered expired, and the element released at the next `take()` etc. call on the `DelayQueue`.

The `TimeUnit` instance passed to the `getDelay()` method is an `Enum` that tells which time unit the delay should be returned in. The `TimeUnit` enum can take these values:

DAYS
HOURS
MINUTES
SECONDS
MILLISECONDS
MICROSECONDS
NANOSECONDS

The `Delayed` interface also extends the `java.lang.Comparable` interface, as you can see, which means that `Delayed` objects can be compared to each other. This is probably used internally in the `DelayQueue` to order the elements in the queue, so they are released ordered by their expiration time.

Here is an example of how to use the `DelayQueue`:

public class DelayQueueExample {

    public static void main(String\[\] args) {
        DelayQueue queue = new DelayQueue();

        Delayed element1 = new DelayedElement();

        queue.put(element1);

        Delayed element2 = queue.take();
    }
}

The `DelayedElement` is an implementation of the `Delayed` interface that I have created. It is not part of the `java.util.concurrent` package. You will have to create your own implementation of the `Delayed` interface to use the `DelayQueue` class.






