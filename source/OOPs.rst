.. include:: include.rst

.. _oop-basics:

####################################
Object Oriented Programming Concepts
####################################

************
Abstraction
************

Abstraction is a process of providing only *essential* information and hiding the implementation details.

e.g., TV remote -> user only knows to change the channels or volumes using keys without knowing how it actually works in background.

*How to achieve data abstraction?*

    - Interface (java,c#)
    - Abstract class
    - Abstract method

*************
Encapsulation
*************

Encapsulation is the idea of bundling data and methods that work on that data within one unit.

e.g., Class

In encapsulation, the variables of a class will be hidden from other classes, and can be accessed only through the methods of their current class. 
Therefore, it is also known as *data hiding*.

*How to achieve data Encapsulation?*

    - Declare the variables of a class as private.

    - Provide public setter and getter methods to modify and view the variables values.


***********
Inheritance
***********

Inheritance is a concept of inheriting the properties of base class to derived class.
It hepls us to reuse existing functionality and extend the functionality based on new requirement.

**Types of Inheritance**
    - Single -> one base class and one derived class
    - Hierarchical -> multiple classes derived from one base class
    - Multi Level -> one class is derived from another derived class
    - Multiple (Interface) -> C# does not support multiple inheritances of classes. To overcome this problem we use interfaces.

Note : If don't want other classes to inherit from a class, use the sealed keyword

*************
Polymorphism
*************

Polymorphism

