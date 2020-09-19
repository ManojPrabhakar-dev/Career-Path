.. include:: include.rst

.. _Linq:

####
LINQ
####

LINQ stands for **Language-Integrated Query** and it is a powerful query language that was introduced with .Net 3.5 & Visual Studio 2008.

LINQ provides us common query syntax which allows us to query the data from various data sources.

That means using a single query we can get or set the data from various data sources such as SQL Server database, XML documents, ADO.NET Datasets, and any other in-memory objects such as Collections, Generics, etc.

//Add image (Linq_DataSources.png)

**************
Why Use Linq ?
**************

LINQ provides a uniform programming model (i.e. common query syntax) which allows us to work with different data sources but *using a standard or unified coding style*.
As a result, we don’t require to learn different syntaxes to query different data sources.

****************
How Linq Works ?
****************

//Add image for Linq_Architecture

The LINQ provider is a software component that lies between the LINQ queries and the actual data source. 
The Linq provider will convert the LINQ queries into a format that can be understood by the underlying data source. 
For example, LINQ to SQL provider will convert the LINQ queries to SQL statements which can be understood by the SQL Server database. 

**What are LINQ Providers?**

A LINQ provider is software that implements the **IQueryProvider** and **IQueryable** interface for a particular data source.
In other words, it allows us to write LINQ queries against that data source. If you want to create your custom LINQ provider then it must implement IQueryProvider and IQueryable interface.

**What is a Query?**

A query is nothing but a set of instructions which is applied on a data source (i.e. in-memory objects, SQL, XML, etc.) to perform certain operations (i.e. CRUD operations) and then tells the shape of the output from that query. 

Each query is a combination of three things.

* Initialization (to work with a particular data source)

* Condition (where, filter, sorting condition)

* Selection (single selection, group selection or joining)

We can write the LINQ query in three different ways. They are as follows

* Query Syntax

* Method Syntax

* Mixed Syntax (Query + Method)


**LINQ Query Syntax:**

This is one of the easy ways to write complex LINQ queries in an easy and readable format. The syntax for this type of query is very much similar to SQL Query.

//Add image of linq query syntax


**LINQ Method Syntax:**

It uses a lambda expression to define the condition for the query. Method syntaxes are easy to write simple queries to perform read-write operations on a particular data source.
But for complex queries Method syntaxes are a little hard to write as compared to query syntax.

//Add image of  linqMethodquery

**LINQ Mixed Syntax:**

This is the combination of both Query and Method syntax.

//Add image of LinqMixedSyntax


.. code-block:: c#
   :caption: Linq Syntax example

        //-----------Data Source--------------
        List<int> integerList = new List<int>()
        {
            1, 2, 3, 4, 5, 6, 7, 8, 9, 10
        };

        //------------Query-----------------------
        //LINQ Query using Query Syntax
        var QuerySyntax = from obj in integerList
                            where obj > 5
                            select obj;

        //LINQ Query using Method Syntax
        var MethodSyntax = integerList.Where(obj => obj > 5).ToList();

        //LINQ Query using Mixed Syntax
        var Mixedyntax = (from obj in integerList
                                where obj > 5
                                select obj).Sum();
        
        //--------------Execution---------------------        
        foreach(var item in ____Syntax)
        {
            Console.Write(item + " ");
        }

