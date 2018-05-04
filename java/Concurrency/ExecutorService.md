ExecutorService
===============

*   [ExecutorService Example](#executorservice-example)
*   [Task Delegation](#task-delegation)
*   [ExecutorService Implementations](#executorservice-implementations)
*   [Creating an ExecutorService](#creating-an-executorservice)
*   [ExecutorService Usage](#executorservice-usage)
*   [ExecutorService Shutdown](#executorservice-shutdown)






The `java.util.concurrent.ExecutorService` interface represents an asynchronous execution mechanism which is capable of executing tasks in the background. An `ExecutorService` is thus very similar to a [thread pool](/java-concurrency/thread-pools.html). In fact, the implementation of `ExecutorService` present in the `java.util.concurrent` package is a thread pool implementation.

ExecutorService Example
-----------------------

Here is a simple Java `ExectorService` example:

ExecutorService executorService = Executors.newFixedThreadPool(10);

executorService.execute(new Runnable() {
    public void run() {
        System.out.println("Asynchronous task");
    }
});

executorService.shutdown();

First an `ExecutorService` is created using the `newFixedThreadPool()` factory method. This creates a thread pool with 10 threads executing tasks.

Second, an anonymous implementation of the `Runnable` interface is passed to the `execute()` method. This causes the `Runnable` to be executed by one of the threads in the `ExecutorService`.

Task Delegation
---------------

Here is a diagram illustrating a thread delegating a task to an `ExecutorService` for asynchronous execution:

![A thread delegating a task to an ExecutorService for asynchronous execution.](ExecutorService/executor-service.png)

**A thread delegating a task to an ExecutorService for asynchronous execution.**

Once the thread has delegated the task to the `ExecutorService`, the thread continues its own execution independent of the execution of that task.

ExecutorService Implementations
-------------------------------

Since `ExecutorService` is an interface, you need to its implementations in order to make any use of it. The `ExecutorService` has the following implementation in the `java.util.concurrent` package:

*   [ThreadPoolExecutor](threadpoolexecutor.html)
*   [ScheduledThreadPoolExecutor](scheduledexecutorservice.html)

Creating an ExecutorService
---------------------------

How you create an `ExecutorService` depends on the implementation you use. However, you can use the `Executors` factory class to create `ExecutorService` instances too. Here are a few examples of creating an `ExecutorService`:

ExecutorService executorService1 = Executors.newSingleThreadExecutor();

ExecutorService executorService2 = Executors.newFixedThreadPool(10);

ExecutorService executorService3 = Executors.newScheduledThreadPool(10);

ExecutorService Usage
---------------------

There are a few different ways to delegate tasks for execution to an `ExecutorService`:

*   execute(Runnable)
*   submit(Runnable)
*   submit(Callable)
*   invokeAny(...)
*   invokeAll(...)

I will take a look at each of these methods in the following sections.



### execute(Runnable)

The `execute(Runnable)` method takes a `java.lang.Runnable` object, and executes it asynchronously. Here is an example of executing a `Runnable` with an `ExecutorService`:

ExecutorService executorService = Executors.newSingleThreadExecutor();

executorService.execute(new Runnable() {
    public void run() {
        System.out.println("Asynchronous task");
    }
});

executorService.shutdown();

There is no way of obtaining the result of the executed `Runnable`, if necessary. You will have to use a `Callable` for that (explained in the following sections).



### submit(Runnable)

The `submit(Runnable)` method also takes a `Runnable` implementation, but returns a `Future` object. This `Future` object can be used to check if the `Runnable` as finished executing.

Here is a `ExecutorService` `submit()` example:

Future future = executorService.submit(new Runnable() {
    public void run() {
        System.out.println("Asynchronous task");
    }
});

future.get();  //returns null if the task has finished correctly.



### submit(Callable)

The `submit(Callable)` method is similar to the `submit(Runnable)` method except for the type of parameter it takes. The `Callable` instance is very similar to a `Runnable` except that its `call()` method can return a result. The `Runnable.run()` method cannot return a result.

The `Callable`'s result can be obtained via the `Future` object returned by the `submit(Callable)` method. Here is an `ExecutorService` `Callable` example:

Future future = executorService.submit(new Callable(){
    public Object call() throws Exception {
        System.out.println("Asynchronous Callable");
        return "Callable Result";
    }
});

System.out.println("future.get() = " + future.get());

The above code example will output this:

Asynchronous Callable
future.get() = Callable Result



### invokeAny()

The `invokeAny()` method takes a collection of `Callable` objects, or subinterfaces of `Callable`. Invoking this method does not return a `Future`, but returns the result of one of the `Callable` objects. You have no guarantee about which of the `Callable`'s results you get. Just one of the ones that finish.

If one of the tasks complete (or throws an exception), the rest of the `Callable`'s are cancelled.

Here is a code example:

ExecutorService executorService = Executors.newSingleThreadExecutor();

Set<Callable<String>> callables = new HashSet<Callable<String>>();

callables.add(new Callable<String>() {
    public String call() throws Exception {
        return "Task 1";
    }
});
callables.add(new Callable<String>() {
    public String call() throws Exception {
        return "Task 2";
    }
});
callables.add(new Callable<String>() {
    public String call() throws Exception {
        return "Task 3";
    }
});

String result = executorService.invokeAny(callables);

System.out.println("result = " + result);

executorService.shutdown();

This code example will print out the object returned by one of the `Callable`'s in the given collection. I have tried running it a few times, and the result changes. Sometimes it is "Task 1", sometimes "Task 2" etc.



### invokeAll()

The `invokeAll()` method invokes all of the `Callable` objects you pass to it in the collection passed as parameter. The `invokeAll()` returns a list of `Future` objects via which you can obtain the results of the executions of each `Callable`.

Keep in mind that a task might finish due to an exception, so it may not have "succeeded". There is no way on a `Future` to tell the difference.

Here is a code example:

```java
ExecutorService executorService = Executors.newSingleThreadExecutor();

Set<Callable<String>> callables = new HashSet<Callable<String>>();

callables.add(new Callable<String>() {
    public String call() throws Exception {
        return "Task 1";
    }
});
callables.add(new Callable<String>() {
    public String call() throws Exception {
        return "Task 2";
    }
});
callables.add(new Callable<String>() {
    public String call() throws Exception {
        return "Task 3";
    }
});

List<Future<String>> futures = executorService.invokeAll(callables);

for(Future<String> future : futures){
    System.out.println("future.get = " + future.get());
}

executorService.shutdown();
```

ExecutorService Shutdown
------------------------

When you are done using the `ExecutorService` you should shut it down, so the threads do not keep running.

For instance, if your application is started via a `main()` method and your main thread exits your application, the application will keep running if you have an active `ExexutorService` in your application. The active threads inside this `ExecutorService` prevents the JVM from shutting down.

To terminate the threads inside the `ExecutorService` you call its `shutdown()` method. The `ExecutorService` will not shut down immediately, but it will no longer accept new tasks, and once all threads have finished current tasks, the `ExecutorService` shuts down. All tasks submitted to the `ExecutorService` before `shutdown()` is called, are executed.

If you want to shut down the `ExecutorService` immediately, you can call the `shutdownNow()` method. This will attempt to stop all executing tasks right away, and skips all submitted but non-processed tasks. There are no guarantees given about the executing tasks. Perhaps they stop, perhaps the execute until the end. It is a best effort attempt.






