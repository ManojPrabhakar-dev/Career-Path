.. include:: include.rst

.. _csharp_concept_Level1:

##################
C# Concepts Level1
##################

C# was developed by Microsoft and it can be used to develop software for various platforms including Windows, Web, and Mobile.
C# supports modern object-oriented programming language features including Abstraction, Encapsulation, Polymorphism, and Inheritance. 

C# is a strongly typed language and it supports concepts of classes and objects. Classes have members such as fields, properties, events, and methods.

Since its inception, C# language has gone through various upgrades. The latest version of C# is v8.0.

**Array** is the data structure that stores a fixed number of literal values (elements) of the same data type.

Types ->Single dimensional array, Multi Dimensional Array and Jagged Array

*Jagged Array* -> It stores array of array.

int[][] jArray1 = new int[2][]; // can include two single-dimensional arrays 
int[][,] jArray2 = new int[3][,]; // can include three two-dimensional arrays 

******
struct
******

Struct is the **value type** data type that represents data structures. It can contain a parameterized constructor, static constructor, constants, fields, methods, properties, indexers, operators, events, and nested types.

Struct can be used to hold small data values that do not require inheritance, e.g. coordinate points, key-value pairs, and complex data structure.

A structure is declared using **struct** keyword. The default modifier is internal for the struct and its members.

struct Coordinate
{
    public int x;
    public int y;
}

//A struct object can be created with or without the new operator, same as primitive type variables.

Coordinate point = new Coordinate();  //calls default constructor and assign values
Console.WriteLine(point.x); //output: 0  
Console.WriteLine(point.y); //output: 0 

Coordinate point; 
Console.Write(point.x); // Compile time error -> if created without new, need to initialize value before accessing them

point.x = 10;
point.y = 20;
Console.Write(point.x); //output: 10  
Console.Write(point.y); //output: 20

* struct is a value type, so it is faster than a class object. 

* Use struct whenever you *want to just store the data*. Generally, structs are good for game programming. 

* It is easier to transfer a class object than a struct. So do not use struct when you are passing data across other classes.

* struct *cannot inherit* another structure or class, and it cannot be the base of a class.

* struct members cannot be specified as *abstract, sealed, virtual, or protected*.

****
Enum
****

An enum (or enumeration type) is used to assign constant names to a group of integer values. 
It makes constant values more readable and can be used to access array/list index using enum items instead of 0,1 etc.

enum Categories
{
    Electronics = 0,  
    Food = 1, 
    Automotive = 2
}

The enum can be of any numeric data type such as byte, sbyte, short, ushort, int, uint, long, or ulong. However, an enum cannot be a string type.

Specify the type after enum name as : type

//byte Enum

enum Categories: byte
{
    Electronics = 1,  
    Food = 5, 
    Automotive = 6
}

**************
String Builder
**************

**string type** is *immutable*. It means a string cannot be changed once created. 

For example, a new string, "Hello World!" will occupy a memory space on the heap. Now, by changing the initial string "Hello World!" to "Hello World! from Tutorials Teacher" 
will create a *new string object on the memory heap* instead of modifying an original string at the same memory address. 

This behavior would hinder the performance if the original string changed multiple times by replacing, appending, removing, or inserting new strings in the original string.

**StringBuilder** doesn't create a new object in the memory but dynamically expands memory to accommodate the modified string

StringBuilder sb = new StringBuilder(); //string will be appended later
//or
StringBuilder sb = new StringBuilder("Hello World!");

you can also specify the maximum capacity of the StringBuilder object using overloaded constructors

StringBuilder sb = new StringBuilder(50); //string will be appended later
//or
StringBuilder sb = new StringBuilder("Hello World!", 50);

C# allocates a maximum of 50 spaces sequentially on the memory heap. This capacity will automatically be doubled once it reaches the specified capacity

**Points to Remember :**

* StringBuilder is mutable.

* StringBuilder performs faster than string when appending multiple string values.

* Use StringBuilder when you need to append more than three or four strings.

* Use the Append() method to add or append strings to the StringBuilder object.

* Use the ToString() method to retrieve a string from the StringBuilder object.

*****************
Class and objects
*****************

A class is a *user-defined blueprint or prototype from which objects are created*. Class combines the fields and methods into a single unit.

Classes are the *user defined data types that represent the state ,behaviour and identity of an object*.

State represents the properties and behaviour is the action that objects can perform.
Identity gives a unique name to an object and enables one object to interact with other objects.

**Key points about classes**

* Classes are reference types that hold the object created dynamically in a heap.

* The default access modifier of a class is Internal.

* The default access modifier of methods and variables is Private.

**Types of Classes**

* Abstract class

An Abstract class is a class that provides a common definition to the subclasses.
We cannot create an object of an abstract class.If need to use it then it must be inherited in a subclass.

* Partial Class

It is a type of class that allows dividing their properties, methods and events into multiple source files and at compile time these files are combined into a single class.

* Sealed class

A Sealed class is a class  that cannot be inherited and used to restrict the properties.
To access the sealed members we must create an object of the class.

* Static Class

It is the type of class that cannot be instantiated. we cannot create an object of that class using the new keyword, 
such that class members can be called directly using their class name.


*************
Partial Class
*************

Partial Class provides ability to implement the functionality of a single class into multiple files and all these files are combined into a single class file when the application is compiled.
A partial class is created by using a **partial** keyword. 

This keyword is also useful to split the functionality of methods, interfaces, or structure into multiple files.

**Advantages :**

* With the help of partial class multiple developers can work simultaneously on the same class in different files.

* With the help of partial class concept you can split the UI of design code and the business logic code to read and understand the code.

* You can also maintain your application in an efficient manner by compressing large classes into small ones.

******
Static
******

static means something which cannot be instantiated. You cannot create an object of a static class and cannot access static members using an object.

C# classes, variables, methods, properties, operators, events, and constructors can be defined as static using the static modifier keyword.

**Static Class**

* Static classes cannot be instantiated.

* All the members of a static class must be static; otherwise the compiler will give an error.

* A static class cannot contain instance members and constructors.

* Static classes are sealed class and therefor Static Members in Non-static Class cannot be inherited and cannot inherit from other classes.

* Static class members can be accessed using ClassName.MemberName.

* A static class remains in memory for the lifetime of the application domain in which your program resides.

**Static Members in Non-static Class**

The normal class (non-static class) can contain one or more static methods, fields, properties etc.

It is more practical to define a non-static class with some static members, than to declare an entire class as static.

**Static Fields**

Static fields of a non-static class is shared across all the instances. So, changes done by one instance would reflect in others.

**Static Method**

Static methods *can be called without creating an object*. You cannot call static methods using an object of the non-static class.

The static methods *can only call other static methods and access static members*. You cannot access non-static members of the class in the static methods.

**Static Constructor**

A non-static class can contain a parameterless static constructor. It can be defined with the static keyword and without access modifiers like public, private, and protected.

The static constructor is called only once whenever the static method is used or creating an instance for the first time.

A static constructor can only access static members. It cannot contain or access instance members.

Note : Static members are stored in a special area in the memory called **High-Frequency Heap**.

****************
Anonymous Method
****************

Anonymous method is a method without a name. 

Anonymous methods in C# can be defined using the delegate keyword and can be assigned to a variable of delegate type.

.. code-block:: c#
   :caption: Anonymous example
        public delegate void Print(int value);

        static void Main(string[] args)
        {
            Print print = delegate(int val) { 
                Console.WriteLine("Inside Anonymous method. Value: {0}", val); 
            };

            print(100);
        }

Anonymous methods can also be passed to a method that accepts the delegate as a parameter.

Anonymous methods can be used as event handlers:

saveButton.Click += delegate(Object o, EventArgs e)
{ 
    MessageBox.Show("Save Successfully!"); 
};

*****************
Lambda Expression
*****************

// C# 3.0(.NET 3.5) introduced the lambda expression along with LINQ

The lambda expression is a shorter way of representing anonymous method using some special syntax.

//Anonymous Method in c#
delegate(Student s) { return s.Age > 12 && s.Age < 20; };

//Lambda Expression in c#
s => s.Age > 12 && s.Age < 20

// can wrap the parameters in parenthesis if you need to pass more than one parameter
(s, youngAge) => s.Age >= youngage;

//You can also give type of each parameters if parameters are confusing
(Student s,int youngAge) => s.Age >= youngage;

//Lambda Expression without Parameter
() => Console.WriteLine("Parameter less lambda expression")

//You can wrap expressions in curly braces if you want to have more than one statement in the body
(s, youngAge) =>
{
  Console.WriteLine("Lambda expression with multiple statements in the body");    
  Return s.Age >= youngAge;
}

************************
Value and Reference Type
************************

Data types are categorized based on how they store their value in the memory. C# includes the following categories of data types:

* Value type

* Reference type

* Pointer type

**Value Type**

A data type is a value type if it holds a *data value* within its own memory space. 
It means the variables of these data types directly contain values.

Pass a value-type variable to a method, the system creates a separate copy of a variable in another method.
e.g., Pass by Value 

e.g., int,float,bool,byte,char,struct etc

**Reference Type**

Reference type doesn't store its value directly. Instead, it stores the address where the value is being stored.

Pass a reference type variable to a method , it doesn't create a new copy; instead, it passes the variable's address.

e.g., Pass by Reference

e.g.,Class,Delegate,String,Array

**Null**

The default value of a reference type variable is null when they are not initialized. 


**************
Nullable Types
**************

a value type cannot be assigned a null value. For example, int i = null will give you a compile time error.

C# 2.0 introduced nullable types that allow you to assign null to value type variables. You can declare nullable types using **Nullable<T>** where T is a type.

Nullable<int> i = null;

A nullable of type int is the same as an ordinary int plus a flag (**HasValue**) that says whether the int has a value or not (is null or not).

You can use the '?' operator to shorthand the syntax e.g. int?, long? instead of using Nullable<T>.

int? i = null;
double? D = null;

Use the '??' operator to assign a nullable type to a non-nullable type.

int? i = null;
            
int j = i ?? 0;  // '??' operator to specify that if i is null then assign 0 to j.

**Points to Remember :**

* Nullable<T> type allows assignment of null to value types.

* ? operator is a shorthand syntax for Nullable types.

* Use value property to get the value of nullable type.

* Use HasValue property to check whether value is assigned to nullable type or not.

* Static Nullable class is a helper class to compare nullable types (Nullable.Compare).

**************
Anonymous Type
**************

Anonymous type is a type (class) without any name that can contain **public read-only properties** only. 
It cannot contain other members, such as fields, methods, events, etc.

You create an anonymous type using the **new** operator with an object initializer syntax. 
The *implicitly typed variable*- **var** is used to hold the reference of anonymous types.

var student = new { Id = 1, FirstName = "James", LastName = "Bond" };
Console.WriteLine(student.Id); //output: 1
Console.WriteLine(student.FirstName); //output: James

An anonymous type will always be **local to the method** where it is defined. It cannot be returned from the method.

************
Dynamic Type
************

C# 4.0 (.NET 4.5) introduced a new type called dynamic that avoids compile-time type checking. 

A dynamic type escapes type checking at compile-time; instead, it resolves type at run time.

Dynamic types change types at run-time based on the assigned value.

.. code-block:: c#
   :caption: Dynamic Type example

        dynamic MyDynamicVar = 100;
        Console.WriteLine("Value: {0}, Type: {1}", MyDynamicVar, MyDynamicVar.GetType());

        MyDynamicVar = "Hello World!!";
        Console.WriteLine("Value: {0}, Type: {1}", MyDynamicVar, MyDynamicVar.GetType());

***********
Collections
***********

C# includes specialized classes that *store series of values or objects* are called collections.

two types of collections available in C#: 

* non-generic collections  //System.Collections namespace

* generic collections  //System.Collections.Generic

Note : It is recommended to use the generic collections because they perform faster than non-generic collections and also minimize exceptions by giving compile-time errors.

**ArrayList**

ArrayList is a non-generic collection of objects whose size increases dynamically. It is the same as Array except that its size increases dynamically.

An ArrayList can be used to add unknown data where you don't know the types and the size of the data.

var arlist1 = new ArrayList();
arlist1.Add(1);
arlist1.Add("Bill");

//Access individual item using indexer
int firstElement = (int) arlist[0]; //returns 1
string secondElement = (string) arlist[1]; //returns "Bill"

Use the AddRange(ICollection c) method to add an entire Array, HashTable, SortedList, ArrayList, BitArray, Queue, and Stack in the ArrayList.


**List<T>**

The List<T> is a collection of strongly typed objects that can be accessed by index and having methods for sorting, searching, and modifying list.

**List<T> Characteristics**

* List<T> equivalent of the ArrayList, which implements IList<T>.

* List<T> can contain elements of the specified type. It provides compile-time type checking and **doesn't perform boxing-unboxing** because it is generic.

* Elements can be added using the Add(), AddRange() methods or collection-initializer syntax.

* Elements can be accessed by passing an index e.g. myList[0]. Indexes start from zero.

* List<T> performs faster and less error-prone than the ArrayList.


**SortedList<TKey, TValue>**

The SortedList<TKey, TValue>, and SortedList are collection classes that can store key-value pairs that are sorted by the keys based on the associated **IComparer** implementation. 

For example, if the keys are of primitive types, then sorted in ascending order of keys.

C# supports generic and non-generic SortedList. It is recommended to use generic SortedList<TKey, TValue> because it *performs faster and less error-prone* than the non-generic SortedList.

**SortedList Characteristics**

* It uses less memory than SortedDictionary<TKey,TValue>.

* It is *faster in the retrieval of data* once sorted, whereas SortedDictionary<TKey, TValue> is *faster in insertion and removing key-value pairs*.

**Dictionary<TKey, TValue>**

The Dictionary<TKey, TValue> is a generic collection that stores key-value pairs in no particular order.

IDictionary<int, string> numberNames = new Dictionary<int, string>();
numberNames.Add(1,"One"); //adding a key/value using the Add() method
numberNames.Add(2,"Two");

foreach(KeyValuePair<int, string> kvp in numberNames)
    Console.WriteLine("Key: {0}, Value: {1}", kvp.Key, kvp.Value);


*********
HashTable
*********

The Hashtable is a non-generic collection that stores key-value pairs, similar to generic Dictionary<TKey, TValue> collection. 

It optimizes lookups by computing the hash code of each key and stores it in a different bucket internally and then matches the hash code of the specified key 
at the time of accessing values.

.. code-block:: c#
   :caption: Hashtable example

        Hashtable numberNames = new Hashtable();
        numberNames.Add(1,"One"); //adding a key/value using the Add() method
        numberNames.Add(2,"Two");
        numberNames.Add(3,"Three");

        //The following throws run-time exception: key already added.
        //numberNames.Add(3, "Three"); 

        foreach(DictionaryEntry de in numberNames)
            Console.WriteLine("Key: {0}, Value: {1}", de.Key, de.Value);

        //Hashtable is a non-generic collection, so you must type cast values while retrieving it.

        string one = (string) numberNames[1]; //cast to string

Why Use Hashtable ?

* In a well-dimensioned hash table, the average cost for each lookup is independent of the number of elements stored in the table.

* In many situations, hash tables turn out to be more efficient than search trees or any other table lookup structure.

Disadvantage

* The hash tables are not effective when the number of entries is very small.

They are widely used in many kinds of computer software, particularly for associative arrays, database indexing, caches and sets.

*****
Stack
*****

Stack is a special type of collection that stores elements in LIFO style (*Last In First Out*). C# includes the generic Stack<T> and non-generic Stack collection classes. 
It is recommended to use the generic Stack<T> collection.

Stack is useful to store temporary data in LIFO style, and you might want to delete an element after retrieving its value.

**Stack<T> Characteristics**

* Stack<T> is Last In First Out collection.

* Stack<T> can contain elements of the specified type. It provides compile-time type checking and doesn't perform boxing-unboxing because it is generic.

* Elements can be added using the **Push()**(*Inserts an item at the top of the stack*) method.

* Elements can be retrieved using the **Pop()**(*Removes and returns items from the top of the stack*) and **Peek()**(*Returns the top item from the stack*) methods.
  It does not support an indexer.

*****
Queue
*****

Queue is a special type of collection that stores the elements in FIFO style (*First In First Out*), exactly opposite of the Stack<T> collection. 
It contains the elements in the order they were added. 

C# includes generic Queue<T> and non-generic Queue collection. It is recommended to use the generic Queue<T> collection.

**Queue<T> Characteristics**

* Queue<T> is FIFO (First In First Out) collection.

* Queue<T> can contain elements of the specified type. It provides compile-time type checking and doesn't perform boxing-unboxing because it is generic.

* Elements can be added using the Enqueue() method.

* Elements can be retrieved using the Dequeue() and the Peek() methods. It does not support an indexer.