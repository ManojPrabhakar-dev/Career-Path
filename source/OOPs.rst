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

Polymorphism refers to the ability to present the same interface in different forms.

The types of polymorphism are,

* Compile-time polymorphism 

    * Function/Method overloading (parametric or Template)

    * Operator Overloading

* Runtime Polymorphism or Dynamic Polymorphism

    * Method overriding or inclusion polymorphism

**Compile Time Polymorphism** means defining multiple methods with the same name but with different parameters.
It can be achieved by using method overloading and it is also called **early binding** or **static binding**.

.. code-block:: c#
    :caption: Function Overloading code example.

        public class Calculate
        {
            public void AddNumbers(int a, int b)
            {
                Console.WriteLine("a + b = {0}", a + b);
            }

            public void AddNumbers(int a, int b, int c)
            {
                Console.WriteLine("a + b + c = {0}", a + b + c);
            }
        }


**RunTime or Dynamic Polymorphism** means overriding a base class method in the derived class by creating a similar function and 
this can be achieved by implementing **abstract** classes and **virtual** functions along with **inheritance** principle.

we can point to any derived class from the object of the base class at runtime that shows the ability of runtime binding.
Through the reference variable of a base class, the determination of the method to be called is based on the object being referred to by reference variable.

Compiler would not be aware whether the method is available for overriding the functionality or not. So compiler would not give any error at compile time. 
At runtime, it will be decided which method to call and if there is no method at runtime, it will give an error.

.. code-block:: c#
    :caption: Method Overriding code example with **virtual** functions.

        class Animal  // Base class (parent) 
        {
            public virtual void animalSound() 
            {
                Console.WriteLine("The animal makes a sound");
            }
        }

        class Pig : Animal  // Derived class (child) 
        {
            public override void animalSound() 
            {
                Console.WriteLine("The pig says: wee wee");
            }
        }

        class Dog : Animal  // Derived class (child) 
        {
            public override void animalSound() 
            {
                Console.WriteLine("The dog says: bow wow");
            }
        }

        class Program 
        {
            static void Main(string[] args) 
            {
                Animal myAnimal = new Animal();  // Create a Animal object
                Animal myPig = new Pig();  // Create a Pig object
                Animal myDog = new Dog();  // Create a Dog object

                myAnimal.animalSound();
                myPig.animalSound();
                myDog.animalSound();
            }
        }

        The output will be:

            The animal makes a sound
            The pig says: wee wee
            The dog says: bow wow


.. code-block:: c#
    :caption: Method Overriding code example with **Abstract** class.
       
        class Shape 
        {
            protected int width, height;
            
            public Shape( int a = 0, int b = 0) {
                width = a;
                height = b;
            }
            public virtual int area() {
                Console.WriteLine("Parent class area :");
                return 0;
            }
        }

        class Rectangle: Shape 
        {
            public Rectangle( int a = 0, int b = 0): base(a, b) {

            }
            public override int area () {
                Console.WriteLine("Rectangle class area :");
                return (width * height); 
            }
        }

        class Triangle: Shape 
        {
            public Triangle(int a = 0, int b = 0): base(a, b) {
            }
            public override int area() {
                Console.WriteLine("Triangle class area :");
                return (width * height / 2); 
            }
        }

        class Caller 
        {
            public void CallArea(Shape sh) {
                int a;
                a = sh.area();
                Console.WriteLine("Area: {0}", a);
            }
        }  

        class Tester 
        {
            static void Main(string[] args) 
            {
                Caller c = new Caller();
                Rectangle r = new Rectangle(10, 7);
                Triangle t = new Triangle(10, 5);
                
                c.CallArea(r);
                c.CallArea(t);
                Console.ReadKey();
            }
        }
        
