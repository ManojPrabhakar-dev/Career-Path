.. include:: include.rst

.. _csharp_concept_Level5:

***************
Multi Threading
***************

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

