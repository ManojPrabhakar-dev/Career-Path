.. include:: include.rst

.. _csharp_concept_Level1:

##################
C# Concepts Level1
##################

C# was developed by Microsoft and it can be used to develop software for various platforms including Windows, Web, and Mobile.
C# supports modern object-oriented programming language features including Abstraction, Encapsulation, Polymorphism, and Inheritance. 

C# is a strongly typed language and it supports concepts of classes and objects. Classes have members such as fields, properties, events, and methods.

Since its inception, C# language has gone through various upgrades. The latest version of C# is v8.0.

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

******
Tuple
******

The Tuple<T> class was introduced in .NET Framework 4.0. A tuple is a data structure that contains a sequence of elements of different data types. 
It can be used where you want to have a data structure to hold an object with properties, but you don't want to create a separate type for it.

Tuple<T1, T2, T3, T4, T5, T6, T7, TRest> //tuple can only include a maximum of eight elements

Tuple<int, string, string> person = 
                        new Tuple <int, string, string>(1, "Steve", "Jobs");
                        
var person = Tuple.Create(1, "Steve", "Jobs");  //static helper class Tuple, which returns an instance of the Tuple<T> without specifying each element's type

//Accessing Tuple elements - Item<elementNumber>

person.Item1; // returns 1
person.Item2; // returns "Steve"
person.Item3; // returns "Jobs"

//Nested Tuples

var numbers = Tuple.Create(1, 2, 3, 4, 5, 6, 7, Tuple.Create(8, 9, 10, 11, 12, 13));
numbers.Item1; // returns 1
numbers.Rest.Item1; //returns (8, 9, 10, 11, 12, 13)
numbers.Rest.Item1.Item1; //returns 8

**Usage of Tuple**

* When you want to return multiple values from a method without using **ref** or **out** parameters.

* When you want to pass multiple values to a method through a single parameter.

* When you want to hold a database record or some values temporarily without creating a separate class.

The Tuple elements can be accessed using properties with a name pattern Item<elementNumber>, which does not make sense.

C# 7.0 (.NetFramework 4.7) includes **ValueTuple** to overcome Tuple's limitations and makes it even easier to work with Tuple.

***********
Value Tuple
***********

Value Tuple can be created and initialized using parentheses () and specifying the values in it.

var person = (1, "Bill", "Gates");
    
//equivalent Tuple
//var person = Tuple.Create(1, "Bill", "Gates");

ValueTuple<int, string, string> person = (1, "Bill", "Gates"); //can be created by specifying specific typeto each element
person.Item1;  // returns 1

Shorthand : (int, string, string) person = (1, "Bill", "Gates");

ValueTuple can include more than eight values.

**Named Menbers**

We can assign names to the ValueTuple properties instead of having the default property names like Item1, Item2 and so on.

(int Id, string FirstName, string LastName) person = (1, "Bill", "Gates");
person.Id;   // returns 1

We can also assign member names on the right side with values, as below.

var person = (Id:1, FirstName:"Bill", LastName: "Gates");

**Deconstruction**

Individual members of a ValueTuple can be retrieved by deconstructing it.

A deconstructing declaration syntax splits a ValueTuple into its parts and assigns those parts individually to fresh variables.

// change property names
(int PersonId, string FName, string LName) = GetPerson();

// use var as datatype - We can also use var instead of explicit data type names.
(var PersonId, var FName, var LName) person = GetPerson();

static (int, string, string) GetPerson() 
{
    return (Id:1, FirstName: "Bill", LastName: "Gates");
}

ValueTuple also allows "discards" in deconstruction for the members you are not going to use.

// use discard _ for the unused member LName
(var id, var FName, _) = GetPerson(); 

********
Indexers
********

An indexer is a special type of property that allows a class or a structure to be accessed like an array for its internal collection.

An indexer can be defined the same way as property with **this** keyword and square brackets **[]**.

.. code-block:: c#
   :caption: Indexer example
        class StringDataStore
        {
            private string[] strArr = new string[10]; // internal data storage

            public string this[int index]
            {
                get => strArr[index];    
                set => strArr[index] = value;             
            }
        }

        //Accessing the indexer
        StringDataStore strStore = new StringDataStore();
        strStore[0] = "One";

**Generic Indexer**

Generic indexer can be used with any data type

.. code-block:: c#
   :caption: Generic Indexer example

        class DataStore<T>
        {
            private T[] store; 

            public DataStore()
            {
                store = new T[10];
            }

            public DataStore(int length)
            {
                store = new T[length];
            }

            public T this[int index]
            {
                get => store[index];    
                set => store[index] = value; 
            }

            public int Length
            {
                get=> store.Length;        
            }
        }

**Overload Indexer**

It can be overloaded with the different data types for index.

.. code-block:: c#
   :caption: Overload Indexer example

        private string[] strArr = new string[10];

        public string this[int index]
        {
            get => strArr[index];    
            set => strArr[index] = value;             
        }

        public string this[string name]
        {
            get
            {
                foreach (string str in strArr){
                    if(str.ToLower() == name.ToLower())        
                        return str;
                    }
                        
                return null;
            }
        }

