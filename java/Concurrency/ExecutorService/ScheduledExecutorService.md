ScheduledExecutorService
========================

*   [ScheduledExecutorService Example](#scheduledexecutorservice-example)
*   [ScheduledExecutorService Implementations](#scheduledexecutorservice-implementations)
*   [Creating a ScheduledExecutorService](#creating-a-scheduledexecutorservice)
*   [ScheduledExecutorService Usage](#scheduledexecutorservice-usage)
    *   [schedule (Callable task, long delay, TimeUnit timeunit)](#schedule-callable)
    *   [schedule (Runnable task, long delay, TimeUnit timeunit)](#schedule-runnable)
    *   [scheduleAtFixedRate (Runnable, long initialDelay, long period, TimeUnit timeunit)](#scheduleatfixedrate)
    *   [scheduleWithFixedDelay (Runnable, long initialDelay, long period, TimeUnit timeunit)](#schedulewithfixeddelay)
*   [ScheduledExecutorService Shutdown](#scheduledexecutorservice-shutdown)






The `java.util.concurrent.ScheduledExecutorService` is an [`ExecutorService`](executorservice.html) which can schedule tasks to run after a delay, or to execute repeatedly with a fixed interval of time in between each execution. Tasks are executed asynchronously by a worker thread, and not by the thread handing the task to the `ScheduledExecutorService`.

ScheduledExecutorService Example
--------------------------------

Here is a simple `ScheduledExecutorService` example:

ScheduledExecutorService scheduledExecutorService =
        Executors.newScheduledThreadPool(5);

ScheduledFuture scheduledFuture =
    scheduledExecutorService.schedule(new Callable() {
        public Object call() throws Exception {
            System.out.println("Executed!");
            return "Called!";
        }
    },
    5,
    TimeUnit.SECONDS);

First a `ScheduledExecutorService` is created with 5 threads in. Then an anonymous implementation of the `Callable` interface is created and passed to the `schedule()` method. The two last parameters specify that the `Callable` should be executed after 5 seconds.

ScheduledExecutorService Implementations
----------------------------------------

Since `ScheduledExecutorService` is an interface, you will have to use its implementation in the `java.util.concurrent` package, in order to use it. `ScheduledExecutorService` as the following implementation:

*   ScheduledThreadPoolExecutor

Creating a ScheduledExecutorService
-----------------------------------

How you create an `ScheduledExecutorService` depends on the implementation you use. However, you can use the `Executors` factory class to create `ScheduledExecutorService` instances too. Here is an example:

ScheduledExecutorService scheduledExecutorService =

        Executors.newScheduledThreadPool(5);

ScheduledExecutorService Usage
------------------------------

Once you have created a `ScheduledExecutorService` you use it by calling one of its methods:

*   schedule (Callable task, long delay, TimeUnit timeunit)
*   schedule (Runnable task, long delay, TimeUnit timeunit)
*   scheduleAtFixedRate (Runnable, long initialDelay, long period, TimeUnit timeunit)
*   scheduleWithFixedDelay (Runnable, long initialDelay, long period, TimeUnit timeunit)

I will briefly cover each of these methods below.

### schedule (Callable task, long delay, TimeUnit timeunit)

This method schedules the given `Callable` for execution after the given delay.

The method returns a `ScheduledFuture` which you can use to either cancel the task before it has started executing, or obtain the result once it is executed.

Here is an example:

ScheduledExecutorService scheduledExecutorService =
        Executors.newScheduledThreadPool(5);

ScheduledFuture scheduledFuture =
    scheduledExecutorService.schedule(new Callable() {
        public Object call() throws Exception {
            System.out.println("Executed!");
            return "Called!";
        }
    },
    5,
    TimeUnit.SECONDS);

System.out.println("result = " + scheduledFuture.get());

scheduledExecutorService.shutdown();

This example outputs:

Executed!
result = Called!

### schedule (Runnable task, long delay, TimeUnit timeunit)

This method works like the method version taking a `Callable` as parameter, except a `Runnable` cannot return a value, so the `ScheduledFuture.get()` method returns null when the task is finished.

### scheduleAtFixedRate (Runnable, long initialDelay, long period, TimeUnit timeunit)

This method schedules a task to be executed periodically. The task is executed the first time after the `initialDelay`, and then recurringly every time the `period` expires.

If any execution of the given task throws an exception, the task is no longer executed. If no exceptions are thrown, the task will continue to be executed until the `ScheduledExecutorService` is shut down.

If a task takes longer to execute than the period between its scheduled executions, the next execution will start after the current execution finishes. The scheduled task will not be executed by more than one thread at a time.

### scheduleWithFixedDelay (Runnable, long initialDelay, long period, TimeUnit timeunit)

This method works very much like `scheduleAtFixedRate()` except that the `period` is interpreted differently.

In the `scheduleAtFixedRate()` method the `period` is interpreted as a delay between the start of the previous execution, until the start of the next execution.

In this method, however, the `period` is interpreted as the delay between the **end** of the previous execution, until the start of the next. The delay is thus between finished executions, not between the beginning of executions.

ScheduledExecutorService Shutdown
---------------------------------

Just like an `ExecutorService`, the `ScheduledExecutorService` needs to be shut down when you are finished using it. If not, it will keep the JVM running, even when all other threads have been shut down.

You shut down a `ScheduledExecutorService` using the `shutdown()` or `shutdownNow()` methods which are inherited from the `ExecutorService` interface. See the [ExecutorService Shutdown](executorservice.html#executorservice-shutdown) section for more information.






