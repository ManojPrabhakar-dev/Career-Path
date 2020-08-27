.. include:: include.rst

.. _csharp_concept:

###########
C# Concepts
###########
C# was developed by Microsoft and it can be used to develop software for various platforms including Windows, Web, and Mobile.
C# supports modern object-oriented programming language features including Abstraction, Encapsulation, Polymorphism, and Inheritance. 

C# is a strongly typed language and it supports concepts of classes and objects. Classes have members such as fields, properties, events, and methods.

Since its inception, C# language has gone through various upgrades. The latest version of C# is v8.0.

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

About Events
