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


