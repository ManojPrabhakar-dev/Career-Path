.. include:: include.rst

.. _csharp_concept_Level4:

*******************
Boxing and Unboxing
*******************

**Boxing** is the process of converting a value type to the object type(Reference Type). Boxing is implicit.

Example: Boxing Copy
int i = 10;
object o = i; //performs boxing

practical example.

ArrayList list = new ArrayList();
list.Add(10); // boxing
list.Add("Bill");

Above, ArrayList is a class in C#, and so it is a reference type. We add an int value 10 in it. 
So, .NET will perform the boxing process here to assign value type to reference type.

**Unboxing** is the reverse of boxing. It is the process of converting a reference type to value type.
Unboxing is explicit. It means we have to cast explicitly.

object o = 10;
int i = (int)o; //performs unboxing

A boxing conversion makes a copy of the value. So, changing the value of one variable will not impact others.

**Note :**

Boxing and unboxing degrade the performance. So, avoid using it.They also lack in type safety and increases runtime overhead.
Use generics to avoid boxing and unboxing. For example, use List instead of ArrayList.


***************
Named Parameter
***************

Named Parameters allow developers to pass a method arguments with parameter names. Prior to these this feature, the method parameters were passed using a sequence only.
Now, using named parameters in C#, we can put any parameter in any sequence as long as the name is there

public static int AddNumber(int firstNumber, int secondNumber)  
{  
    return firstNumber + secondNumber;  
} 

// Named arguments can be supplied for the parameters in either order.    
Console.WriteLine(AddNumber(firstNumber: 12, secondNumber: 13));    
Console.WriteLine(AddNumber(secondNumber: 13, firstNumber: 12));    
    
******************
Optional Parameter
******************

In C# 4.0, the concept of optional parameters was introduced that allows developers to declare parameters as optional. 
That means, if these arguments are not passed, they will be ommitted from the execution. Optional parameters are not mandatory.

Option 1: Using Default Values

public int AddNumber(int firstNumber, int secondNumber = 0)  {  }

Option 2: Using OptionalAttribute

public int AddNumber(int firstNumber, [Optional] int secondNumber) { }

Option 3: Using Method Overloading

Option 4: Using Params Keyword.

Using params, we can pass a number of parameters to a method and implement optional parameters concept.
 

******
Params
******

Params used as a parameter which can take the variable number of arguments.

It is useful when programmer don’t have any prior knowledge about the number of parameters to be used.

Parmas are also useful to write "clean code". Instead of using various overloaded methods to pass multiple values, we can simply create an array
and pass it as an arugment or a comma separated list of values.

//function using object type params  
public void result(params object[] array) {  //void result(params int[] array) ->int value type
    for (int i = 0; i < array.Length; i++)   
        Console.WriteLine(array[i]);  
    }      
}  

*********************
Shallow and Deep copy
*********************

**Shallow copy :** Creating a new object and then copying the value type fields of the current object to the new object. 
But when the data is reference type, then the only reference is copied but not the referred object itself.

*Value type will be copied (duplicated) and reference type will be point to same address in both original and newly created object*.
*So, Changes in value type will not affect other object but changes in ref will reflect to other object*.

**Deep Copy :** It is a process of creating a new object and then copying the fields of the current object to the newly created object
to make a complete copy of the internal reference types.

*Both Value type and reference type will be copied (duplicated) from original object to newly created object*.
*So, Changes in both value type and reference type will not affect other object*.

**Options to Clone objects**

* Using the System.Object.MemberwiseClone method to perform a shallow copy

* Using Reflection by taking advantage of the Activator.CreateInstance method

* Using Serialization

* By implementing the IClonable interface


*******************
Const and Read-only
*******************

A const is a compile-time constant whereas readonly allows a value to be calculated at run-time and set in the constructor or field initializer.
So, a 'const' is always constant but 'readonly' is read-only once it is assigned.

**const**

They can not be declared as static (they are implicitly static)

The value of constant is evaluated at compile time

constants are initialized at declaration only


**readonly**

They can be either instance-level or static

The value is evaluated at run time

readonly can be initialized in declaration or by code in the constructor

***************
Ref, In and Out
***************

**Ref**

The ref keyword is used to pass an argument as a reference. This means that when value of that parameter is changed in the method, 
it gets reflected in the calling method.

*An argument that is passed using a ref keyword must be initialized in the calling method before it is passed to the called method*.

Note : By default, a reference type passed into a method will have any changes made to its values reflected outside the method as well. 
If you assign the reference type to a new reference type inside the method, those changes will only be local to the method

**Out**

The out keyword is also used to pass an argument like ref keyword, but the argument can be passed without assigning any value to it.

*An argument that is passed using an out keyword must be initialized in the called method before it returns back to calling method*.

**In**
The in modifier is most often used for performance reasons and was introduced in C# 7.2. 

The motivation of in is to be used with a *struct to improve performance by declaring that the value will not be modified*.
When using with reference type, it only prevents you from assigning a new reference.

**ref** is used to state that the parameter passed may be modified by the method.

**in** is used to state that the parameter passed cannot be modified by the method.

**out** is used to state that the parameter passed must be modified by the method.

Both the ref and in require the parameter to have been initialized before being passed to a method.
The out modifier does not require this and is typically not initialized prior to being used in a method.


**********************
Early and Late Binding
**********************

The compiler performs a process called binding when an object is assigned to an object variable.

The early binding (static binding) refers to compile time binding and late binding (dynamic binding) refers to runtime binding.


In **Early Binding**, an object is assigned to a variable declared to be of a specific object type. 

Early binding objects are basically a strong type objects or static type objects. While Early Binding, methods, functions and properties
which are detected and checked during compile time and perform other optimizations before an application executes. 

The biggest advantage of using early binding is for performance and ease of development.


By contrast, in **Late binding** functions, methods, variables and properties are detected and checked only at the run-time. 

It implies that the compiler does not know what kind of object or actual type of an object or which methods or properties an object contains until run time.

The biggest advantages of Late binding is that the Objects of this type can hold references to any object, but lack many of the advantages of early-bound objects.

**Example**

Method Overloading happens at compile time (Early Binding) while Overriding happens at runtime (Late Binding).
