ArrayBlockingQueue
==================

The `ArrayBlockingQueue` class implements the [`BlockingQueue`](blockingqueue.html) interface. Read the [`BlockingQueue`](blockingqueue.html) text for more information about the interface.

`ArrayBlockingQueue` is a bounded, blocking queue that stores the elements internally in an array. That it is bounded means that it cannot store unlimited amounts of elements. There is an upper bound on the number of elements it can store at the same time. You set the upper bound at instantiation time, and after that it cannot be changed.

The `ArrayBlockingQueue` stores the elements internally in FIFO (First In, First Out) order. The `head` of the queue is the element which has been in queue the longest time, and the `tail` of the queue is the element which has been in the queue the shortest time.

Here is how to instantiate and use an `ArrayBlockingQueue`:

BlockingQueue queue = new ArrayBlockingQueue(1024);

queue.put("1");

Object object = queue.take();

Here is a `BlockingQueue` example that uses Java Generics. Notice how you can put and take String's instead of :

BlockingQueue<String> queue = new ArrayBlockingQueue<String>(1024);

queue.put("1");

String string = queue.take();






