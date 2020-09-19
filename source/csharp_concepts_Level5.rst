.. include:: include.rst

.. _csharp_concept_Level5:


###################
C# 7.X New features
###################

*************
Out Variables
*************

We can declare the variable directly in parameter of function call without the need of creating prior to function call.
since  the out parameter doesn't need to be initialized before using.

GetEmployeeDetails(out **string** EmployeeName, out **string** Gender);

****************
Pattern Matching
****************

* Can perform pattern matching on any data type, even your own, whereas with if/else, you always need primitives to match.

* Pattern matching can extract values from your expression.

* C# 7.0 introduces pattern matching in two cases, the **is** expression and the **switch** statement.

.. code-block:: c#
   :caption: Before pattern Matching example

        public static void DisplayArea(Shape shape)
        {
             if (shape is Circle)
            {
                Circle c = (Circle)shape;
                Console.WriteLine("Area of Circle is : " + c.Radius * c.Radius * Shape.PI);
            }
            else if (shape is Rectangle)
            {
                Rectangle r = (Rectangle)shape;
                Console.WriteLine("Area of Rectangle is : " + r.Length * r.Height);
            }
        }

.. code-block:: c#
   :caption:  pattern Matching using **is** example

        public static void DisplayArea(Shape shape)
        {
            if (shape is Circle c)
            {
                Console.WriteLine("Area of Circle is : " + c.Radius * c.Radius * Shape.PI);
            }
            else if (shape is Rectangle r)
            {
                Console.WriteLine("Area of Rectangle is : " + r.Length * r.Height);
            }
        }

.. code-block:: c#
   :caption:  pattern Matching using **switch** example

        public static void DisplayArea(Shape shape)
        {
            switch (shape)
            {
                case Circle c:
                    Console.WriteLine("Area of Circle is : " + c.Radius * c.Radius * Shape.PI);
                    break;
                case Rectangle r:
                    Console.WriteLine("Area of Rectangle is : " + r.Length * r.Height);
                    break;
            }
        }

***************
Digit Separator
***************

In reality, it’s very difficult to read a very large number. To overcome this problem, C# 7 comes with a new feature called digit separators “_”.
Now, it is possible to use one or more Underscore (_) character as digit separators.

var bigNumber = 123456789012345678;
var bigNumberSplit = 123_456_789_012_345_678;
//Both var print same value - Digit separator only for visual understanding
Console.WriteLine("bigNumber : {0}, bigNumberSplit : {1}", bigNumber, bigNumberSplit);

***************
Local Functions
***************

The Local Functions are the special kind of inner function or you can say sub-function or function within a function that can be declared and defined by the parent function.
These methods or functions are the private methods for their containing type and are only called by their parent method. 

* Small helper functions to be used several times within the main or parent method.

* Parameter validation functions for any iterators or asynchronous methods.

* An alternate to recursive functions as local function comparatively takes less memory due to the reduced call stack.


*************************
Ref Local and Ref Returns
*************************

The **Ref local** in C# is a new variable type that is used to store the references. It is mostly used in conjunction with Ref returns to store the reference in a local variable.
That means Local variables now can also be declared with the ref modifier.

int no1 = 1;
ref int no2 = ref no1;
no2 = 2;
Console.WriteLine($"local variable {nameof(no1)} after the change: {no1}");

**Ref Returns**

Before C# 7.0, the ref was only used to be passed as a parameter in a method, however, there was no provision to return it and use it later.
With C# 7.0, this constraint has been waived off and now you can return references from a method as well. 

public **ref** int GetFirstOddNumber(int[] numbers);

ref int oddNum = ref GetFirstOddNumber(x); //storing as reference  

************************
Generalized Async Return
************************

The generalized async returns types in C# mean you can return a lightweight value type instead of a reference type to avoid additional memory allocations.
From C# 7, there is an inbuilt value type **ValueTask <T>** which can be used instead of Task<T>.

**********
AutoMapper
**********

The AutoMapper in C# is a mapper between two objects. That is AutoMapper is an object-object mapper.
It maps the properties of two different objects by transforming the input object of one type to the output object of another type.

            //Initialize the mapper
            var config = new MapperConfiguration(cfg =>
                    cfg.CreateMap<Employee, EmployeeDTO>()
                );
            //Creating the source object
            Employee emp = new Employee
            {
                Name = "James",
                Salary = 20000,
                Address = "London",
                Department = "IT"
            };
            //Using automapper
            var mapper = new Mapper(config);
            var empDTO = mapper.Map<EmployeeDTO>(emp);
            //OR
            //var empDTO2 = mapper.Map<Employee, EmployeeDTO>(emp);
            Console.WriteLine("Name:" + empDTO.Name + ", Salary:" + empDTO.Salary + ", Address:" + empDTO.Address + ", Department:" + empDTO.Department);
            Console.ReadLine();


//When Souce property and destination property name are different, use ForMember method like below,

            //Initialize the mapper
            var config = new MapperConfiguration(cfg =>
                    cfg.CreateMap<Employee, EmployeeDTO>()
                    .ForMember(dest => dest.FullName, act => act.MapFrom(src => src.Name))
                    .ForMember(dest => dest.Dept, act => act.MapFrom(src => src.Department))
                );

