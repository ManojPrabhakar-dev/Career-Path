.. include:: include.rst

.. _csharp_concept_Level2:

##################
C# Concepts Level2
##################

*********
Delegates
*********

A delegate is a reference type variable that can holds a reference to the methods. Delegates in C# are similar to the function pointer in C/C++. 
It provides a way which tells which method is to be called when an event is triggered.

* Provides a good way to encapsulate the methods.

* Delegates are mainly used in implementing the call-back methods and events.

* Delegates can be chained together as two or more methods can be called on a single event.


**Syntax** :

[modifier] **delegate** [return_type] [delegate_name] ([parameter_list]);

*Instantiation & Invocation of Delegates*

[delegate_name]  [instance_name] = new [delegate_name](calling_method_name);

Example :

public delegate void Func_Add(int a, int b);

public void sum(int a, int b) 
{ 
    Console.WriteLine("Sum of two numbers = {0}", (a + b)); 
} 

Func_Add del_func = new Func_Add(sum);

//Func_Add del_func; //also method can be assigned directly without new keyword. 
// del_func = Sum  

del_func(1,2);   or

//Can be called using Invoke method
del_func.Invoke(1,2);

**Multicasting of a Delegate**

Multicasting of delegate is an extension of the normal delegate(sometimes termed as Single Cast Delegate). 
It helps the user to point more than one method in a single call.

*Properties*:

* Delegates are combined and when you call a delegate then a complete list of methods is called.

* All methods are called in First in First Out(FIFO) order.

* ‘+’ or ‘+=’ Operator is used to add the methods to delegates.

* ‘–’ or ‘-=’ Operator is used to remove the methods from the delegates list.

**Note**:*Remember, multicasting of delegate should have a return type of Void otherwise it will throw a runtime exception*. 
*Also, the multicasting of delegate will return the value only from the last method added in the multicast*.

public void sub(int a, int b) 
{ 
    Console.WriteLine("Subtract of two numbers = {0}", (a - b)); 
} 

del_func += sub;

//Will call Sum method firstand then call Sub method since both method are subscribed to this delegate

del_func.Invoke(1,2); 

**Types of Delegates**

**Func Delegate**

Func is a in-built generic delegate . It can contain minimum 0 and maximum of 16 input parameters in it and contain only one out parameter. 
The last parameter of the Func delegate is the out parameter which is considered as return type and used for the result.

public delegate TResult Func<in T1, in T2, out TResult>(T arg);

public static int Sum(int x, int y)
{
    return x + y;
}

// Using Func delegate 
// Here, Func delegate contains 
// the two parameters of int type 
// one result parameter of int type 

**Func**<int,int, int> add = Sum;

int result = add(10, 10); //result = 20

*Func with Zero Input Parameter*

Func<int> getRandomNumber;

Important Points:

* Func delegate type must return a value.

* Func delegate does not allow ref and out parameters.

Example:

Func with Anonymous Method
Func<int> getRandomNumber = delegate()
                            {
                                Random rnd = new Random();
                                return rnd.Next(1, 100);
                            };

Func with lambda expression
Func<int> getRandomNumber = () => new Random().Next(1, 100);

//Or 

Func<int, int, int>  Sum  = (x, y) => x + y;


**Action Delegate**

Action delegate is an in-built generic type delegate. This delegate saves you from defining a custom delegate as shown in the below examples and make your program more readable and optimized. 
It can contain minimum 1 and maximum of 16 input parameters and does not contain any output parameter. 

The Action delegate is generally used for those methods which do not contain any return value.

Example :

public static void myfun(int p, int q) 
{ 
    Console.WriteLine(p - q); 
} 

//Action delegate contains two input parameters
Action<int, int> val = myfun; 
val(20, 5); //15

*Important Points*:

* The only difference between Action Delegates and Function Delegates is that Action Delegates does not return anything i.e. having void return type.

* An Action Delegate can also be initialized using the new keyword.
Action<int> val = new Action<int>(myfun);

* An Action Delegate can also be initialized by directly assigning to a method.
Action<int> val = myfun;

* You can also use an Action delegate with an anonymous method as shown in the below example:

Action<string> val = delegate(string str) 
{ 
    Console.WriteLine(str); 
}; 
val("C# Anonymous example"); 

You can also use a Action delegate with the lambda expressions as shown in the below example:

Action<string> val = (str) = > Console.WriteLine(str); 
val("C# Lamba example"); 

**Predicate delegate**

Predicate represents a method containing a set of criteria and checks whether the passed parameter meets those criteria.

A predicate delegate methods must take one input parameter and return a boolean - true or false.

public delegate bool Predicate<in T>(T obj);

static bool IsUpperCase(string str)
{
    return str.Equals(str.ToUpper());
}

**Predicate**<string> isUpper = IsUpperCase;

bool result = isUpper("hello world!!");

Console.WriteLine(result);  //False

**Points to Remember**

Predicate delegate takes one input parameter and boolean return type.

Anonymous method and Lambda expression can be assigned to the predicate delegate.

******
Events
******

An event provide notifications to client applications when some state changes of an object.
Events is an encapsulated delegate (provide one level of abstraction for delegate) and it follows the *observer design pattern*

The class who raises events is called **Publisher**, and the class who receives the notification is called **Subscriber**. 
There can be multiple subscribers of a single event.

For example, a publisher raises an event when some action occurred. The subscribers, who are interested in getting a notification 
when an action occurred, should register with an event and handle it.

Events and Delegates are tightly coupled concept because event handling requires delegate implementation to dispatch events.


An event can be declared in two steps:

* Declare a delegate.
* Declare a variable of the delegate with **event** keyword.


.. code-block:: c#
   :caption: Event example

        public delegate void Notify();  // delegate
                            
        public class ProcessBusinessLogic
        {
            public event Notify ProcessCompleted; // event

            public void StartProcess()
            {                
                OnProcessCompleted();
            }

            protected virtual void OnProcessCompleted() //protected virtual method
            {
                //if ProcessCompleted is not null then call delegate
                ProcessCompleted?.Invoke(); 
            }
        }

        class Program
        {
            public static void Main()
            {
                ProcessBusinessLogic bl = new ProcessBusinessLogic();
                bl.ProcessCompleted += bl_ProcessCompleted; // register with an event
                bl.StartProcess();
            }

            // event handler
            public static void bl_ProcessCompleted()
            {
                Console.WriteLine("Process Completed!");
            }
        }

**Built-in EventHandler Delegate**

.NET Framework includes built-in delegate types **EventHandler** and **EventHandler<TEventArgs>** for the most common events.

Any event should include two parameters: the source of the event and event data. 

* Use the EventHandler delegate for all events that do not include event data. 

* Use EventHandler<TEventArgs> delegate for events that include data to be sent to handlers.

.. code-block:: c#
   :caption: EventHandler example

   // declaring an event using built-in EventHandler
    public event EventHandler ProcessCompleted; 

    public void StartProcess()
    {        
        OnProcessCompleted(EventArgs.Empty); //No event data
    }

    protected virtual void OnProcessCompleted(EventArgs e)
    {
        ProcessCompleted?.Invoke(this, e);
    }


**Passing an Event Data**

Most events send some data to the subscribers. The EventArgs class is the base class for all the event data classes. 
.NET includes many built-in event data classes such as RoutedEventArgs, SerialDataReceivedEventArgs etc. 
It follows a naming pattern of ending all event data classes with **EventArgs**. 


You can create your custom class for event data by deriving EventArgs class.

.. code-block:: c#
   :caption: EventHandler<TEventArgs> example

    // declaring an event using built-in EventHandler
    public event EventHandler<bool> ProcessCompleted; 

    public void StartProcess()
    {       
        OnProcessCompleted(true);       
    }

    protected virtual void OnProcessCompleted(bool IsSuccessful)
    {
        ProcessCompleted?.Invoke(this, IsSuccessful);
    }

If need to pass more than one value as event data, then create a class deriving from the EventArgs base class

.. code-block:: c#
   :caption: Custom EventArgs example

   class ProcessEventArgs : EventArgs
    {
        public bool IsSuccessful { get; set; }
        public DateTime CompletionTime { get; set; }
    }

    // declaring an event using built-in EventHandler
    public event EventHandler<ProcessEventArgs> ProcessCompleted; 

    public void StartProcess()
    {
        var data = new ProcessEventArgs();		  
        data.IsSuccessful = true;
        data.CompletionTime = DateTime.Now;
        OnProcessCompleted(data);        
    }

    protected virtual void OnProcessCompleted(ProcessEventArgs e)
    {
        ProcessCompleted?.Invoke(this, e);
    }

**Points to Remember :**

* An event is a wrapper around a delegate. It depends on the delegate.

* Use "event" keyword with delegate type variable to declare an event.

* The publisher class raises an event, and the subscriber class registers for an event and provides the event-handler method.

* Name the method which raises an event prefixed with "On" with the event name.

* The signature of the handler method must match the delegate signature.

* Register with an event using the += operator. Unsubscribe it using the -= operator. Cannot use the = operator.

* Events can be declared static, virtual, sealed, and abstract.

* An Interface can include the event as a member.

* Event handlers are invoked synchronously if there are multiple subscribers.

***********
Interfaces
***********

Interface is same as a class but interface will contain only the declarations of methods, properties, and events that a class or struct can implement
and class can contain both declarations and implementation of methods, properties and events.

An interface in c# is more like a contract and the class that implements an interface must provide an implementation for all the members 
that are specified in the interface.

C# will not support multiple inheritance of classes but that can be achieved by using an interface.

"*An interface includes the declarations of related functionalities*. 
*The entities that implement the interface must provide the implementation of declared functionalities*."

**Why Use Interface ?**

interfaces are a very logical way of grouping objects in terms of behavior.

Using interface-based design concepts provides 

* loose coupling, component-based programming, easier maintainability, makes your code base more scalable and makes code reuse much more accessible because the implementation is separated from the interface. 

* Interfaces add a **plug and play** like architecture into your applications. 

* Interfaces help define a contract (agreement or blueprint), between your application and other objects. 
This indicates what sort of methods, properties, and events are exposed by an object.

* When many components use the same interface it allows us to easily interchange one component for another which is using the same interface. 
  Dynamic programs begin to form easily from this.


.. code-block:: c#
   :caption: Interface example (Implicit)

        //interface members are public by default.
        interface IFile
        {
            void ReadFile();
            void WriteFile(string text);
        }

        class FileInfo : IFile
        {
        //Interface members must be implemented with the public modifier

            public void ReadFile()
            {
                Console.WriteLine("Reading File");
            }

            public void WriteFile(string text)
            {
                Console.WriteLine("Writing to file");
            }
        }

        public static void Main()
        {
            IFile file1 = new FileInfo();
            FileInfo file2 = new FileInfo();
            
            file1.ReadFile(); 
            file1.WriteFile("content"); 

            file2.ReadFile(); 
            file2.WriteFile("content"); 
        }

we created objects of the FileInfo class and assign it to IFile type variable and FileInfo type variable. 
When interface implemented implicitly, you can access IFile members with the IFile type variables as well as FileInfo type variable.

**Explicit Implementation**

An interface can be implemented explicitly using <InterfaceName>.<MemberName>. Explicit implementation is useful when class is implementing multiple interfaces; 
thereby, it is more readable and eliminates the confusion. It is also useful if interfaces have the same method name coincidently.

.. code-block:: c#
   :caption: Interface example (Explicit)
        interface IFile
        {
            void ReadFile();
            void WriteFile(string text);
        }

        class FileInfo : IFile
        {
            public void IFile.ReadFile()
            {  Console.WriteLine("Reading File"); }

            public void IFile.WriteFile(string text)
            {  Console.WriteLine("Writing to file"); }

            public void Search(string text)
            {   Console.WriteLine("Searching in file");    }
        }

        public class Program
        {
            public static void Main()
            {
                IFile file1 = new FileInfo();
                FileInfo file2 = new FileInfo();
                
                file1.ReadFile(); 
                file1.WriteFile("content"); 
                //file1.Search("text to be searched")//compile-time error 
                
                file2.Search("text to be searched");
                //file2.ReadFile(); //compile-time error 
                //file2.WriteFile("content"); //compile-time error 
            }
        }

**Implementing Multiple Interfaces**

A class or struct can implement multiple interfaces. It must provide the implementation of all the members of all interfaces.


.. code-block:: c#
   :caption: Multiple Interface example

        interface IFile
        {
            void ReadFile();
        }

        interface IBinaryFile
        {
            void OpenBinaryFile();
            void ReadFile();
        }

        class FileInfo : IFile, IBinaryFile
        {
            void IFile.ReadFile()
            {
                Console.WriteLine("Reading Text File");
            }

            void IBinaryFile.OpenBinaryFile()
            {
                Console.WriteLine("Opening Binary File");
            }

            void IBinaryFile.ReadFile()
            {
                Console.WriteLine("Reading Binary File");
            }

            public void Search(string text)
            {
                Console.WriteLine("Searching in File");
            }
        }

        public class Program
        {
            public static void Main()
            {
                IFile file1 = new FileInfo();
                IBinaryFile file2 = new FileInfo();
                FileInfo file3 = new FileInfo();
                
                file1.ReadFile(); 
                //file1.OpenBinaryFile(); //compile-time error 
                //file1.SearchFile("text to be searched"); //compile-time error 
                
                file2.OpenBinaryFile();
                file2.ReadFile();
                //file2.SearchFile("text to be searched"); //compile-time error 
            
                file3.SearchFile("text to be searched");
                //file3.ReadFile(); //compile-time error 
                //file3.OpenBinaryFile(); //compile-time error 
            }
        }

The FileInfo implements two interfaces IFile and IBinaryFile explicitly. 
It is recommended to implement interfaces explicitly when implementing multiple interfaces to avoid confusion and more readability.

