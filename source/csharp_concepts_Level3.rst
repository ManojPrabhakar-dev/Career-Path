.. include:: include.rst

.. _csharp_concept_Level3:

##################
C# Concepts Level3
##################

*****************************
Covariance and Contravariance
*****************************

**Covariance** enables you to pass a derived type where a base type is expected. Co-variance is like variance of the same kind.

The base class and other derived classes are considered to be the same kind of class that adds extra functionalities to the base type. 

Covariance in delegates allows flexiblity in the return type of delegate methods.


**Contravariane** is applied to parameters. Cotravariance allows a method with the *parameter of a base class* to be assigned to a delegate 
that expects the *parameter of a derived class*.


.. code-block:: c#
   :caption: Covariance and Contravariance example

        delegate Small covarDel(Big mc);

        class Program
        {
            static Big Method4(Small sml)
            {        
                return new Big();
            }

            static void Main(string[] args)
            {
                covarDel del = Method4;    
                Small sm = del(new Big());
            }
        }


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

********
Generics
********

In C#, generic means not specific to a particular data type.

C# allows you to define generic classes, interfaces, abstract classes, fields, methods, static methods, 
properties, events, delegates, and operators using the **type parameter** and without the specific data type.

A type parameter is a placeholder for a particular type specified when creating an instance of the generic type.

e.g. TypeName<T> where T is a type parameter.

class DataStore<T>
{
    public T Data { get; set; }   //Generic fields

    private T[] _data = new T[10];   //Generic Array
    
    //Generic Methods

    public void AddOrUpdate(int index, T item)
    {
        if(index >= 0 && index < 10)
            _data[index] = item;
    }

    public T GetData(int index)
    {
        if(index >= 0 && index < 10)
            return _data[index];
        else 
            return default(T);
    }
}

//Instantiating Generic class
//String Data Type
DataStore<string> strStore = new DataStore<string>(); 
strStore.Data = "Hello World!";

//Int Data Type
DataStore<int> intStore = new DataStore<int>();
intStore.Data = 100;

class KeyValuePair<TKey, TValue> //Multiple Type parameter
{
    public TKey Key { get; set; }
    public TValue Value { get; set; }
}

A generic class increases the reusability. The more type parameters mean more reusable it becomes.

Example: Generic Method in Non-generic Class

class Printer
{
    public void Print<T>(T data)
    {
        Console.WriteLine(data);
    }
}

Printer printer = new Printer();
printer.Print<int>(100);
printer.Print(200); // type infer from the specified value
printer.Print<string>("Hello");
printer.Print("World!"); // type infer from the specified value

**Advantages of Generics**

* Generics increase the reusability of the code. You don't need to write code to handle different data types.

* Generics are type-safe. You get compile-time errors if you try to use a different data type than the one specified in the definition.

* Generic has a performance advantage because it removes the possibilities of boxing and unboxing.


*******************
Generic Constraints
*******************

C# allows you to use constraints to restrict client code to specify certain types while instantiating generic types.

GenericTypeName<T> where T  : contraint1, constraint2

**where T : class**

// generic class with a constraint to reference types
// can pass reference types such as class, interface, delegate, or array type
class DataStore<T> where T : class
{
    public T Data { get; set; }
}

**where T : struct**

struct constraint that restricts type argument to be non-nullable value type only.

class DataStore<T> where T : struct
{
    public T Data { get; set; }
}

**where T : new()**

new() type argument must be a reference type which has a public parameterless constructor

class DataStore<T> where T : class, new()
{
    public T Data { get; set; }
}


**where T : baseclass**

Base class constraint that restricts type argument to be a derived class of the specified class, abstract class, or an interface.

class DataStore<T> where T : IEnumerable
{
    public T Data { get; set; }
}

DataStore<ArrayList> store = new DataStore<ArrayList>(); // valid
DataStore<List> store = new DataStore<List>(); //valid

****************
Extension Method
****************

Extension method concept allows you to *add new methods in the existing class or structure without modifying the source code* of the original type.

Not require any kind of special permission from the original type and there is no need to re-compile the original type as well.

.. code-block:: c#
   :caption: Extension Method example
        namespace ExtensionMethod {
            // Here Geek class contains three methods 
            // Now we want to add two more new methods in it  
            // Without re-compiling this class 
            class Geek {  
            public void M1()  
            { 
                Console.WriteLine("Method Name: M1"); 
            }      
            public void M2() 
            { 
                Console.WriteLine("Method Name: M2"); 
            }     
            public void M3() 
            { 
                Console.WriteLine("Method Name: M3"); 
            }     
        }

        namespace ExtensionMethod {
            // This class contains M4 and M5 method 
            // Which we want to add in Geek class, so we use the binding parameter (**this**) to bind these methods with Geek class
            // NewMethodClass is a static class 
            static class NewMethodClass {   
                public static void M4(this Geek g) 
                { 
                    Console.WriteLine("Method Name: M4"); 
                }   
                public static void M5(this Geek g, string str) 
                { 
                    Console.WriteLine(str); 
                } 
        } 

        // Now we create a new class in which 
        // Geek class access all the five methods 
        public class GFG { 
            public static void Main(string[] args) 
            { 
                Geek g = new Geek(); 
                g.M1(); 
                g.M2(); 
                g.M3(); 
                g.M4(); 
                g.M5("Method Name: M5"); 
            } 
        }  

**Advantages:**

* Extension method is to add new methods in the existing class **without** using **inheritance**.

* You can add new methods in the existing class without modifying the source code of the existing class.

* It can also work with **sealed** class.

**Limitation :**

* Extension method does not support method overriding.

* It cannot apply to fields, properties, or events.

