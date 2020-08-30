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