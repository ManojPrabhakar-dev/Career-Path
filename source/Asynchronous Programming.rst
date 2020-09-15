.. include:: include.rst

.. _csharp_concept_AsynchronousProgramming:

###############
Multi Threading
###############

**Thread** is a lightweight processand it is defined as the execution path of a program.

A thread is the smallest unit of execution within a process.
So, every program or application has some logic/code and to execute that logic/code, Thread comes into the picture.

By default, every process has at least one thread which is responsible for executing the application code and that thread is called as **Main Thread**.

A single thread can have only one path of execution but as mentioned earlier, sometimes we may need multiple paths of execution and that is where threads play a role.

Usage : complicated and time consuming operations , Concurrent programming etc

Note :

*Under the operating system, we have processes that running our applications. So under the process, an application runs*.
*To run the code of an application, the process will make use of a concept called Thread*.

Two types of threads are as follows :

* Foreground Thread

A thread which keeps on running to complete its work even if the Main thread leaves its process.

* Background Thread

A thread which leaves its process when the Main method leaves its process.

The **Join method** of Thread class in C# blocks the current thread and makes it wait until the child thread on which the Join method invoked completes its execution.


*Protect the Shared Resources in Multithreading*

**Locking** 

private static object _lockObject = new object();

static void DisplayMessage()
{
    lock(_lockObject)
    {
        Console.Write("[Welcome to the ");
        Thread.Sleep(1000);
        Console.WriteLine("world of dotnet!]");
    }
}

**Monitor**

The Monitor class provides a mechanism to *synchronizes access to objects*.

The lock is the shortcut for **Monitor.Enter** with try and finally. So, the lock provides the basic functionality to acquire an exclusive lock on a synchronized object.
But, If you want more control to implement advanced multithreading solutions using TryEnter() Wait(), Pulse(), and PulseAll() methods, then the Monitor class is your option.

**Mutex**

Mutex also works likes a lock i.e. acquired an exclusive lock on a shared resource from concurrent access, but it works across multiple processes. 

The Mutex class provides the **WaitOne()** method which we need to call to lock the resource and similarly it provides **ReleaseMutex()** which is used to unlock the resource.

Note that a Mutex can only be released from the same thread which obtained it.

**Semaphore**

The Semaphore is used to limit the number of threads that can have access to a shared resource concurrently.

In real-time, we need to use Semaphore when we have a limited number of resources and we want to limit the number of threads that can use it.

**DeadLock**

Deadlock is a situation where two or more threads are unmoving or frozen in their execution because they are waiting for each other to finish.

*Avoiding Deadlock by using Monitor.TryEnter method*

Monitor.TryEnter method takes time out in milliseconds. Using that parameter we can specify a timeout for the thread to release the lock. 

If a thread is holding a resource for a long time while the other thread is waiting, then Monitor will provide a time limit and force the lock to release it.
So that the other thread will enter into the critical section.


***********
Thread Pool
***********

Thread pooling is the process of creating a collection of threads during the initialization of a multithreaded application,
and then reusing those threads for new tasks as and when required, instead of creating new threads.

A thread pool comprises a collection of threads and it can be used to perform several activities in the background.

Threads are expensive as they consume a lot of resources in your system for initialization, switching contexts, and releasing the resources they occupy.

A thread pool is a good choice when you want to limit the number of threads that are running at a given point of time and 
want to avoid the overhead of creating and destroying threads in your application.

There are a number of ways to create the thread pool:

Via the Task Parallel Library (from Framework 4.0).

By calling ThreadPool.QueueUserWorkItem.

Via asynchronous delegates.

Via BackgroundWorker.

########################
Asynchronous Programming
########################

Asynchronous programming truly does is increase the amount of requests that can be handled at the same time with the same resources.

code which is asynchronous is NOT the same as code which uses multithreading.  

*Asynchronous Is Not Multithreading*

In multithreading, work is done on totally separate threads, with no interaction between individual threads. 

In asynchronous programming, we define points from which threads can leave and return to at some point in the future.


**Task Parallel Library**

.NET Framework 4 introduces Task Parallel Library (TPL) that makes it easier for developers to write parallel programs that target multi-core machines (automatically use multiple processors)
which improve the performance.

we can express the code as the Parallel task, which will be run concurrently on the available processors.

Parallel Programming is a type of programming in which many calculations or the execution of processes are carried out simultaneously.

* The Tasks must be independent.

* Order of the execution does not matter.

**Data parallelism:** 

Operation is applied to each element of a collection. Means each process does the same work on unique and independent pieces of data.

*Parallel.For*

number = 10;
Parallel.For(0, number, count =>
{
    Console.WriteLine($"value of count = {count}, thread = {Thread.CurrentThread.ManagedThreadId}");
    //Sleep the loop for 10 miliseconds
    Thread.Sleep(10);
});

*Parallel.ForEach*

List<int> integerList = Enumerable.Range(1, 10).ToList();
Parallel.ForEach(integerList, i =>
{
    long total = DoSomeIndependentTimeconsumingTask();
    Console.WriteLine("{0} - {1}", i, total);
});

*Control the Degree of concurrency*

 var options = new ParallelOptions()
{
    MaxDegreeOfParallelism = 2
};
List<int> integerList = Enumerable.Range(0,10).ToList();
Parallel.ForEach(integerList, options, i =>
{
    Console.WriteLine(@"value of i = {0}, thread = {1}",
        i, Thread.CurrentThread.ManagedThreadId);
});


**Task parallelism:** 

In the case of Task Parallelism independent computations are executed in parallel. Means each process performs a different function or executes different code sections that are independent.

*Parallel.Invoke*

Parallel.Invoke(
        () => DoSomeTask(1),
        () => DoSomeTask(2),
        () => DoSomeTask(3),
        () => DoSomeTask(4),
        () => DoSomeTask(5),
        () => DoSomeTask(6),
        () => DoSomeTask(7)
    );

***********************************
Task-based Asynchronous Programming
***********************************

Task class will always represent a single operation and that operation will be executed asynchronously on a thread pool thread rather than synchronously on the main thread of the application.

A task scheduler is responsible for starting the Task and also responsible for managing it. By default, the Task scheduler uses threads from the thread pool to execute the Task.

**What is Thread pool in C#?**

A Thread pool in C# is a collection of threads which can be used to perform a number of task in the background. Once a thread completes its task, then again it sent to the thread,
so that it can be reused. This reusability of threads avoids an application to create a number of threads which ultimately uses less memory consumption.


*Using the Task class and Start method*

Task task1 = new Task(Loop_Method);  //Eventhough mainthread exited, Task will still run async in separate thread until it finish the operation
task1.Start();

if you want to the task creation and scheduling separately, then you need to create the task separately by using Task class and then call the Start method to schedule the task execution for a later time

**Fire and Forgot**

*Creating a task object using Factory Property*

Task task1 =  Task.Factory.StartNew(Loop_Method); 


*Creating a Task object using the Run method*

Task task1 = Task.Run(() => { Loop_Method(); });

use above method where need to call non-Task returning time consuming API (mostly third-party or pre-defined API).


From a performance point of view, the *Task.Run* or *Task.Factory.StartNew* methods are preferable to create and schedule the tasks.


Reference : `Task based Asynchronous Programming blog <https://www.pluralsight.com/guides/async-programming-task-parallel-library>`_


**Task Return Value**

Using this Task<T> class we can return data or value from a task.

Task<double> task1 = Task.Run(() => 
            {
                return CalculateSum(10);
            });
            
Console.WriteLine($"Sum is: {task1.Result}");  //task1.Result is a blocking call

//task1.getAwaiter().getResult(); //another method to get result by blocking call

**Note :** Don't use this blocking call instead use async/await to get result. Blocking call can be used in console app but not in UI programming.


**Chaining Tasks by Using Continuation Tasks**

Asynchronous programming, it is very common to invoke one asynchronous operation from another asynchronous operation passing the data once it completes its execution.
This is called as continuations and in the traditional approach, this has been done by **using the callback methods** which is little difficult to understand.

Can create a continuation by calling the **ContinueWith** method that is going to execute when its previous method has completed its execution.

Task<string> task1 = Task.Run(() =>
{
    return 12;
}).ContinueWith((previous) =>
{
    return $"The Square of {previous.Result} is: {previous.Result * previous.Result}";
});
Console.WriteLine(task1.Result);

Scheduling different continuation task

.. code-block:: c#
   :caption: Continuation example

        Task<int> task = Task.Run(() =>
        {
            return 10;
        });
        task.ContinueWith((i) =>
        {
            Console.WriteLine("TasK Canceled");
        }, TaskContinuationOptions.OnlyOnCanceled);
        task.ContinueWith((i) =>
        {
            Console.WriteLine("Task Faulted");
        }, TaskContinuationOptions.OnlyOnFaulted);
        var completedTask = task.ContinueWith((i) =>
        {
            Console.WriteLine("Task Completed");
        }, TaskContinuationOptions.OnlyOnRanToCompletion);
        completedTask.Wait();


***************
Async and Await
***************

With use of async-await, a method can be written which looks very similar in structure to standard synchronous code, but can work asynchronously without keeping the calling thread busy.

public async Task<int> GetMessageLengthAsync()
{
    Task<string> stringTask = GetTimeTakingMessageAsync(); //calls a long running method
    DoIndependentWork(); //does some independent work meanwhile
    string message = await stringTask; //suspends & awaits the result
    int length = message.Length; //once done, does some work on awaited result
    return length; //then returns the final int result
}

The **async** keyword can be used as a modifier to a method or anonymous method to tell the C#
compiler that the respective method holds an asynchronous operation in it.

The **await** keyword is used while invoking the async method, to tell the compiler that the
remaining code in that particular method should be executed after the asynchronous operation is
completed.

.. code-block:: c#
   :caption: Async and Await example

        static async void DownloadAndBlur()
        {
            var url = "https://...jpg";
            var fileName = Path.GetFileName(url);
            var originalImageBytes = await DownloadImage(url);
            var originalImagePath = Path.Combine(ImageResourcesPath, fileName);
            await SaveImage(originalImageBytes, originalImagePath);
            var blurredImageBytes = await BlurImage(originalImagePath);
            var blurredFileName = $"{Path.GetFileNameWithoutExtension(fileName)}_blurred.jpg";
            var blurredImagePath = Path.Combine(ImageResourcesPath, blurredFileName);
            await SaveImage(blurredImageBytes, blurredImagePath);
            done = true;
        }

when you need to call some predefined method that does not return a Task?

Task.Run returns a Task which means you can use the await keyword with it!

async void OnButtonClick()
{
  await Task.Run(() => /* your code here*/);
}


**CPU bound, and I/O bound**

A *CPU bound work* is something that does some heavy computation, like running some formula along a huge set of data. This needs continuous CPU involvement.
This will need a dedicated thread, and will generally use a ThreadPool thread. 

*I/O bound work* is something that depends on something outside the CPU system.


launching an operation on a separate thread via **Task.Run** is mainly useful for *CPU-bound operations*, not I/O-bound operations.

Use **await** without *Task.Run* for *I/O operations*. This will have the same positive effect of keeping your application responsive,
but without wastefully occupying additional threads/cores.

**Where to Put a Call to Task.Run?**

It's generally recommended that you put calls to Task.Run as close to the UI code and event handlers as possible.
Most CPU-bound code ends up being written as synchronous, and Task.Run goes in the outermost calling method.

**Don't Continue on the Main Thread Unnecessarily**

await captures information about the current thread when used with Task.Run. It does that so execution can continue on the original thread when it is done processing on the other thread.
But what if the rest of the code in the calling method doesn't need to be run on the original thread?

In that case, it turns out you can give your code a bit of a performance boost by telling await that you don't want to continue in the original context.
This is done by using a Task method called **ConfigureAwait**

static async void OnButtonClick()
{
  byte[] imageData = await LoadImage();
  await Task.Run(() => ProcessImage(ref imageData)).ConfigureAwait(false);
  await SaveImage(imageData);
}

The parameter to ConfigureAwait is a boolean named continueOnCapturedContext, and the default is true. By passing false instead, we're indicating that we wish to continue the rest of the method on a thread pool thread instead of the UI thread.
As long as you're not modifying any UI elements in the code following the await (or doing anything else there that would require the main thread of your application), you can safely use this technique to enable an amount of parallelism.

Reference : `Async and Await blog <https://www.pluralsight.com/guides/csharp-async-await-keywords-getting-started>`_

