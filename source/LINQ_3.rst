.. include:: include.rst

.. _LINQ_3:

**********************
SequenceEqual Operator
**********************

The SequenceEqual Operator in Linq is used to check whether two sequences are equal or not. If two sequences are equal then it returns true else it returns false.

Two sequences are considered to be equal when both the sequences have the same number of elements as well as the same values should be present in the same order.

.. code-block:: c#
   :caption: SequenceEqual example
        List<string> cityList1 = new List<string> { "Delhi", "Mumbai", "Hyderabad" };
        List<string> cityList2 = new List<string> { "Delhi", "Mumbai", "Hyderabad" };

        bool IsEqual = cityList1.SequenceEqual(cityList2); //Overload 1
        bool IsEqual = cityList1.SequenceEqual(cityList2, 
                        StringComparer.OrdinalIgnoreCase); //Overload 2

*******************
Partioning Operator
*******************

The Partitioning Operations in Linq are used to divide a data source into two parts and then return one of them as output without changing the positions of the elements.

**Take Operator**

The Take Operator in Linq is used to fetch the first “n” number of elements from the data source where “n” is an integer which is passed as a parameter to the Take method.

.. code-block:: c#
   :caption: Take example
        List<int> numbers = new List<int>() {1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        List<int> ResultMS = numbers.Take(4).ToList(); //1,2,3,4
        //With Filter
        int[] ResultMS = numbers.Where(num => num > 3).Take(4).ToArray();

**TakeWhile Operator**

The TakeWhile Method in Linq is used to fetch all the elements from a data source or sequence until a specified condition is true.
Once the condition is failed, then the TakeWhile method will not check the rest of the elements presents in the data source even though the condition is true for the remaining elements.

.. code-block:: c#
   :caption: TakeWhile example
        List<int> numbers = new List<int>() {1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
            
        List<int> ResultMS = numbers.TakeWhile(num => num < 6).ToList();

**Skip Operator**

The Skip Method in Linq is used to skip or bypass the first “n” number of elements from a data source or sequence and then returns the remaining elements from the data source as output.

.. code-block:: c#
   :caption: Skip example
        List<int> numbers = new List<int>() { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        List<int> ResultMS = numbers.Skip(4).ToList();

**SkipWhile Operator**

The SkipWhile Method in Linq is used to skip or bypass all the elements from a data source or sequence as long as the given the condition specified by the predicate is true and
then returns the remaining element from the sequence as an output.

.. code-block:: c#
   :caption: SkipWhile example
        List<int> numbers = new List<int>() { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        List<int> ResultMS = numbers.SkipWhile(num => num < 5).ToList();

        //Overload 2
        List<string> namesResult = names.SkipWhile((name, index) => name.Length > index).ToList();

**Paging**

Paging is nothing but a process in which we will divide a large data source into multiple pages. At one page we need to display a certain number of records.
And next records can be visible with next – previous buttons or scroll or using any other techniques.

**Advantage**

* Faster data transfer. This is because fewer data will be traveled in the network.

* Improve memory management. This is because we are not showing all the data in a view.

* Better performance.

**Drawbacks**

In a client-server architecture, the number of requests between the client and server is increased.
In such cases, you may get the data at once and store it locally and then implement the paging at the client-side.

**How to Implement Paging**

We can implement the paging using the Linq Skip and Take method. Here we need to understand two things one is PageNumber and the other one is the number of records per page.

Let say Page Number = PN and Number Of Records Per Page = NRP, then you need to use the following formula

Result = DataSource.Skip((PN – 1) * NRP).Take(NRP)

*******************
Generation Operator
*******************

The Enumerable class also provides three non-extension methods are as follows.

* Range()

* Repeat<T>()

* Empty<T>()


.. code-block:: c#
   :caption: Generation example
        IEnumerable<int> numberSequence = Enumerable.Range(1, 10);
        
        //Range with Filter
        IEnumerable<int> EvenNumbers = Enumerable.Range(10, 30).Where(x => x%2 == 0);

        //Repeat
        IEnumerable<string> repeatStrings = Enumerable.Repeat("Repeat Example Tutorials", 10);

        //Empty
        //using NULL-COALESCING operator (??)
        IEnumerable<int> integerSequence = GetData() ?? Enumerable.Empty<int>();

        //Without Empty
        if(integerSequence != null) //Need to check before using the result

        private static IEnumerable<int> GetData()
        {
            return null;
        }











