.. include:: include.rst

.. _LINQ_2:

****************
Aggregate Method
****************

The Linq aggregate functions are used to group together the values of multiple rows as the input and then return the output as a single value.
So, simple word, we can say that the aggregate function in C# is always going to return a single value. 

**When to use the Aggregate Functions in C#?**

Whenever you want to perform some mathematical operations such as Sum, Count, Max, Min, Average, and Aggregate on the numeric property of a collection
then you need to use the Linq Aggregate Functions.

**Sum**

.. code-block:: c#
   :caption: Sum example
        int[] intNumbers = new int[] { 10, 30, 50, 40, 60, 20, 70, 90, 80, 100 };
        //Using Method Syntax
        int MSTotal = intNumbers.Sum();
        //Using Query Syntax
        int QSTotal = (from num in intNumbers
                        select num).Sum();

        //With Filtering
        //Using Method Syntax
        int MSTotal = intNumbers.Where(num => num > 50).Sum();
        //Using Query Syntax
        int QSTotal = (from num in intNumbers
                        where num > 50
                        select num).Sum();

        //Using Method Syntax with a Predicate
        int MSTotal = intNumbers.Sum(num => {
            if (num > 50)
                return num;
            else
                return 0;
        });

        //---------Complex Types----------------
        //Using Method Syntax
        var TotalSalaryMS = Employee.GetAllEmployees()
                            .Sum(emp => emp.Salary);
        //Using Query Syntax
        var TotalSalaryQS = (from emp in Employee.GetAllEmployees()
                                select emp).Sum(e => e.Salary);
        

**Max**

The Linq Max in C# is used to returns the largest numeric value from the collection on which it is applied.

.. code-block:: c#
   :caption: Max example

        int[] intNumbers = new int[] { 10, 80, 50, 90, 60, 30, 70, 40, 20, 100 };
        //Using Method Syntax
        int MSLergestNumber = intNumbers.Max();
        //Using Query Syntax
        int QSLergestNumber = (from num in intNumbers
                        select num).Max();
        //With Filter,Predicate and complex type refer above 

**Min**

The Linq Min method is used to returns the lowest numeric value from the collection on which it is applied.

.. code-block:: c#
   :caption: Min example

        int[] intNumbers = new int[] { 60, 80, 50, 90, 10, 30, 70, 40, 20, 100 };
        //Using Method Syntax
        int MSLowestNumber = intNumbers.Min();
        //Using Query Syntax
        int QSLowestNumber = (from num in intNumbers
                        select num).Min();

**Average**

The Linq Average method is used to calculate the average of numeric values from the collection on which it is applied.
This Average method can return nullable or non-nullable decimal, float or double value.

.. code-block:: c#
   :caption: Average example
        int[] intNumbers = new int[] { 60, 80, 50, 90, 10, 30, 70, 40, 20, 100 };
        //Using Method Syntax
        var MSAverageValue = intNumbers.Average();
        //Using Query Syntax
        var QSAverageValue = (from num in intNumbers
                                select num).Average();

**Count**

The Linq Count Method used to return the number of elements present in the collection or the number of elements that have satisfied a given condition.

.. code-block:: c#
   :caption: Count example
        int[] intNumbers = new int[] { 60, 80, 50, 90, 10, 30, 70, 40, 20, 100 };
        //Using Method Syntax
        int MSCount = intNumbers.Count();
        //Using Query Syntax
        var QSCount = (from num in intNumbers
                                select num).Count();

**Aggregate**

The Linq Aggregate extension method performs an accumulative operation.
There are three overloaded versions of this method is available in System.Linq namespace.

.. image:: images/Linq_Aggregate_Overload.png
   :width: 400

.. code-block:: c#
   :caption: Aggregate example
      //Overload 1
      string[] skills = { "C#.NET", "MVC", "WCF", "SQL", "LINQ", "ASP.NET" };
      string result = skills.Aggregate((s1, s2) => s1 + ", " + s2);

      int[] intNumbers = { 3, 5, 7, 9 };
      int result = intNumbers.Aggregate((n1, n2) => n1 * n2);

      //Overload 2 - Aggregate method takes the first parameter as the seed value to accumulate.
      int[] intNumbers = { 3, 5, 7, 9 };
      int result = intNumbers.Aggregate(2, (n1, n2) => n1 * n2);

      //Complex type
      int Salary = Employee.GetAllEmployees()
                            .Aggregate<Employee, int>(0,
                            (TotalSalary, emp) => TotalSalary += emp.Salary);

      //Overload 3 - Aggregate with Result selector
      string CommaSeparatedEmployeeNames = Employee.GetAllEmployees().Aggregate<Employee, string, string>(
                                        "Employee Names : ",  // seed value
                                        (employeeNames, employee) => employeeNames = employeeNames + employee.Name + ",",
                                        employeeNames => employeeNames.Substring(0, employeeNames.Length - 1));

*******************
Quantifier Operator
*******************

LINQ Quantifier Operators on a data source when we want to check if some or all of the elements of that data source satisfy a condition or not.

All the methods in quantifier operations are always going to return a Boolean value. That means if the all or some of the elements in the data source satisfy the given condition then it is going to return true else it is going to return false.

The following three methods belong to the Quantifier Operations category.

**All:** This specifies whether all the elements of a data source satisfy a given condition or not.

.. code-block:: c#
   :caption: All example
      int[] IntArray = { 11, 22, 33, 44, 55 };
      var Result = IntArray.All(x => x > 10);

      //Using Method Syntax - pass predicate using All for filtering
      var MSResult = Student.GetAllStudnets()
                        .Where(std => std.Subjects.All(x => x.Marks > 80)).ToList();

**Any:** This specifies whether at least one of the elements of a data source satisfies the condition or not.

.. code-block:: c#
   :caption: Any example
      int[] IntArray = { 11, 22, 33, 44, 55 };
      //Using Method Syntax
      var ResultMS = IntArray.Any();
      //Using Query Syntax
      var ResultQS = (from num in IntArray
                        select num).Any();
      Console.WriteLine("Is there any element in the collection : " + ResultMS);

      int[] IntArray = { 11, 22, 33, 44, 55 };
      var Result = IntArray.Any(x => x > 10);

      //fetch all the student details whose mark on any subject is greater than 90
      var MSResult = Student.GetAllStudnets()
                            .Where(std => std.Subjects.Any(x => x.Marks > 90)).ToList();


**Contains:** This method is used to check whether the data source contains a specified element or not.

.. code-block:: c#
   :caption: Contains example
      int[] IntArray = { 11, 22, 33, 44, 55 };      
      var IsExistsMS = IntArray.Contains(33);

      List<string> namesList = new List<string>(){ "James", "Sachin", "Sourav", "Pam", "Sara" };
      //Using Method Syntax
      //This method belongs to System.Collections.Generic namespace
      var IsExistsMS1 = namesList.Contains("Anurag");
      //This method belongs to System.Linq namespace
      var IsExistsMS2 = namesList.AsEnumerable().Contains("Anurag");

Note: if you want to check the values rather than the reference then you need to create a class and need to implement the IEqualityComparere interface.
Then you need to use the overloaded version of the Contains method which takes IEqualityComparere as a parameter.

.. code-block:: c#
   :caption: Contains example
      public class StudentComparer : IEqualityComparer<Student>
      {
        public bool Equals(Student x, Student y)
        {
            //If both object refernces are equal then return true
            if(object.ReferenceEquals(x, y))
            {
                return true;
            }
            //If one of the object refernce is null then return false
            if (x is null || y is null)
            {
                return false;
            }
            return x.ID == y.ID && x.Name == y.Name && x.TotalMarks == y.TotalMarks;
        }
      }

      //Creating Student Comparer Instance
      StudentComparer studentComparer = new StudentComparer();
      //Using Method Syntax
      var IsExistsMS = students.Contains(new Student() { ID = 101, Name = "Priyanka", TotalMarks = 275 }, studentComparer);

****************
GroupBy Operator
****************

This method takes a flat sequence of elements and then organizes the elements into groups (i.e. IGrouping<TKey, TSource>) based on a given key.

.. code-block:: c#
   :caption: GroupBy example
      //Using Method Syntax
      var GroupByMS = Student.GetStudents().GroupBy(s => s.Barnch);
      //Using Query Syntax
      IEnumerable<IGrouping<string, Student>> GroupByQS = (from std in Student.GetStudents()
                        group std by std.Barnch);
      //It will iterate through each groups
      foreach(var group in GroupByMS)
      {
            Console.WriteLine(group.Key +" : " + group.Count());
            //Iterate through each student of a group
            foreach(var student in group)
            {
               Console.WriteLine("  Name :" + student.Name + ", Age: " + student.Age + ", Gender :" + student.Gender);
            }
      }

Note: Each group has a key and you can access the key-value by using the key property. Along the same line, you can use the count property to check how many elements are there in that group.

********************
GroupBy Multiple Key
********************

In real-time applications, we need to group the data based on multiple keys.

Note that when you are using multiple keys in Group By operator then the data returned is an anonymous type.

.. code-block:: c#
   :caption: GroupBy Multiple key example

      var GroupByMultipleKeysMS = Student.GetStudents()
                                        .GroupBy(x => new { x.Barnch, x.Gender })
                                        .OrderByDescending(g => g.Key.Barnch).ThenBy(g => g.Key.Gender)
                                        .Select(g => new
                                        {
                                            Branch = g.Key.Barnch,
                                            Gender = g.Key.Gender,
                                            Students = g.OrderBy(x => x.Name)
                                        });

      //It will iterate through each group
      foreach (var group in GroupByMultipleKeysQS)
      {
            Console.WriteLine($"Barnch : {group.Branch} Gender: {group.Gender} No of Students = {group.Students.Count()}");
            //It will iterate through each item of a group
            foreach (var student in group.Students)
            {
               Console.WriteLine($"  ID: {student.ID}, Name: {student.Name}, Age: {student.Age} ");
            }
            Console.WriteLine();
      }






















