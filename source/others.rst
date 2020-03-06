
// Interface 1 
interface GFG1 { 
	void languages(); 
} 

// Parent class 1 
class Geeks1 : GFG1 { 

	// Providing the implementation 
	// of languages() method 
	public void languages() 
	{ 
        //code here
	} 
} 

// Interface 2 
interface GFG2 { 
	void courses(); 
} 

// Parent class 2 
class Geeks2 : GFG2 { 

	// Providing the implementation 
	// of courses() method 
	public void courses() 
	{ 
        //code here
	} 
} 

// Child class 
class GeeksforGeeks : GFG1, GFG2 { 

	// Creating objects of Geeks1 and Geeks2 class 
	Geeks1 obj1 = new Geeks1(); 
	Geeks2 obj2 = new Geeks2(); 

	public void languages() 
	{ 
		obj1.languages(); 
	} 

	public void courses() 
	{ 
		obj2.courses(); 
	} 
} 

// Driver Class 
public class GFG { 
	// Main method 
	static public void Main() 
	{ 
		// Creating object of GeeksforGeeks class 
		GeeksforGeeks obj = new GeeksforGeeks(); 
		obj.languages(); 
		obj.courses(); 
	} 
} 

-----------------------------------------------------------

Design pattern Link :

1. https://dotnettutorials.net/lesson/creational-design-pattern/

