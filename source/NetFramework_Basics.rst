.. include:: include.rst

.. _NetFramwork_Basics:

*********************
What .NET Represents?
*********************

NET stands for Network Enabled Technology. In .NET, dot (.) refers to object-oriented and NET refers to the internet.
So the complete .NET means *through object-oriented we can implement internet-based applications*.

**What is a Framework?**

A framework is a software. Or you can say a framework is a collection of many small technologies integrated together to develop applications that can be executed anywhere.

What does the DOTNET Framework provide?

The DOTNET Framework provides two things are as follows

* BCL (Base Class Libraries)

Base Class Libraries (BCL) is designed by Microsoft. Without BCL we can’t write any code in .NET. So, BCL is also known as the Building block of .NET Programs.
These are installed into the machine when we installed the .NET framework. BCL contains pre-defined classes and these classes are used for the purpose of application development.

The physical location of BCL is *C:\Windows\assembly*


* CLR (Common Language Runtime)

CLR stands for Common Language Runtime and it is the core component under the .NET framework which is responsible for *converting the MSIL (Microsoft Intermediate Language) code into native code* and then execution. 

//TODO : Add image (Compile_Execute_Flow.png)

In the .NET framework, the code is compiled twice.

In the 1st compilation, the source code is compiled by respective language compiler and generates the intermediate code which is known as MSIL (Microsoft Intermediate Language) or IL (Intermediate language code) Or Managed Code.

In the 2nd compilation, MSIL is converted into Native code (native code means code specific to Operating system so that the code is executed by the Operating System ) using CLR.

What is JIT?

JIT stands for the Just-in-Time compiler. It is the component of CLR which is responsible for converting MSIL code into Native Code. This Native code is directly understandable by the operating system.

The .net framework is available in three different flavors

**DOTNET Framework:** This is the general version required to run .NET applications on Windows OS only.

**.NET mono Framework:** This is required if we want to run DOT NET applications on other OS like Unix, Linux, MAC OS, etc.

**DOT NET Compact Framework:** This is required to run .NET applications on other devices like mobile phones and smartphones.

NOTE: MSIL is only CPU dependent and will run only on Windows OS only using .NET Framework because .NET Framework is designed for Windows OS only.

There is another company known as “NOVEL” designed separate framework known as “MONO Framework”. Using this framework we can run MSIL on different OS Like Linux, UNIX, Mac, BSD, OSX, etc.


***********************
Common Language Runtime
***********************

//TODO : Add image (Compilation_Workflow.png)

CLR is the heart of .NET Framework and it contains the following components.

* Security Manager

* JIT Compiler

* Memory Manager

* Garbage Collector

* Exception Manager

*Common Language Specification (CLS)

* Common Type System (CTS)

The codes which run under the complete control of CLR are called as **Managed Code** in .NET. 

**What is an Assembly in .NET?**

Assembly is a precompiled .NET Code that can be run by CLR (Common Language Runtime).

In the .NET Framework, there are two types of assemblies.

* EXE (Executable)

* DLL (Dynamic Link Library)


********************************************
.Net Standard Vs .Net Core Vs .Net Framework
********************************************

A .NET Core Class Library is built upon the .NET Standard. If you want to implement a library that is portable to the .NET Framework, .NET Core and Xamarin, choose a .NET Standard Library

.NET Core will ultimately implement .NET Standard 2 (as will Xamarin and .NET Framework)

.NET Core, Xamarin and .NET Framework can, therefore, be identified as flavours of .NET Standard

//TODO : Add image (NetStandard.png)


.NET Core is an open-source and cross-platform implementation of .NET. 

We should use the .NET Core:

* While developing applications for cross-platform

* For the development of microservices

* When we want to use Docker containers

* To develop high-performance and scalable systems


Use the .NET Framework when:

* We want to target only Windows

* Our application uses some third party packages which are not supported by .NET Core

* The application uses .NET technologies that are not available for .NET Core


.NET Standard should be the choice when:

* We want to share our common code across different .NET implementations



https://medium.com/@mohamedabdeen/now-we-have-net-standard-net-framework-net-core-f9dda9dacf65

https://www.infoworld.com/article/3394865/net-5-what-the-merger-of-net-framework-and-net-core-means.html