# Study Guide for Java

This is hopefully meant to help as an aid for Intro to CS 2, which focuses on a variety of different topics: 

1. Static methods and variables 
2. References, class parameters and memory diagrams 
3. Different ways of storing information (ASCII, binary, octal, hexadecimal)
4. Inheritance 
5. Poloymorphism and Abstract classes 
6. Copy constructors 
7. ArrayLists 
8. Event-driven programming 
9. Recursion 
10. Enum types

These are just a few topics covered, hopefully enough is provided to generate thinking and assistance when learning/studying for Java!

---
## Static Methods and Static Variables: 

- Static variables in Java apply to the class as whole rather than belonging to individual objects. Which is why they are also called _class variables_.
- Every object of a class has its own copy of each _instance variable_ but there is only one copy of each _static variable_, and it is shared among all objects of the class. 
- When the value of a static variable gets changed, since there is only one copy of it, the value of that variable changes for all objects. 
- On the other hand, when the value of an instance variable gets changes, that change only effects the value of that instance variable for the calling object. In other words, only the instance variable for `this` get modified. 

**Below is an example of this:**
- Note that this is not an ideal use, but meant to highlight everything above into words 

```
public class StaticVarDemo {

  private static int staticVariable = 10;
  private int instanceVariable;

  public StaticVarDemo() {
    instanceVariable = 20;
  }

  public void increment() {
    staticVariable++;
    instanceVariable++;
  }

  public String toString() {
	return ("static var: " + staticVariable
      + ", " 
      + "instance var: " + instanceVariable);
  }
}
```
**Here is a use of this class:**
```
public class TestStaticVersusInstance {
  public static void main(String[] args) {
    StaticVarDemo object1 = new StaticVarDemo();
    StaticVarDemo object2 = new StaticVarDemo();

    object1.increment();
    object1.increment();
    object2.increment();
    System.out.println("object1:" + object1);
    System.out.println("object2:" + object2);
  }
}
```
**static method key points:**
- Just as static variables, static methods belong to a whole class rather than belonging to individual objects 
- The syntax for invoking a static method is different than using an object to invoke a method 

Instead of: 

`objectName.methodName();`

we can say: 

`className.methodName();` 

- Examples of other static method calls are: 

`Math.random();`
`LocalTime.now();`

- Static methods are not assocated with any object, so using `this` cannot be used to access any instance variables. However, you may access all static varables. 

**Example of static method**
- If you have a single class that contains main, any utility methods will be static 

```
// This program declares and initializes the contents of an array. It then
// determines the maximum of the values in the array. The determination of the max
// is done in a static method called maximumValue
import java.util.Scanner;

public class MaxOfArray {

	public static void main(String[] args) {

		// declare the array and store some integers:
		int[] array = { 2, 3, 5, -1, 8, 2 };

		int max = maximumValue(array);
		System.out.println("The maximum in the array is " + max + ".");
	}

	// Return the minimum value of an array of ints
	public static int maximumValue(int[] a) {

                // initial value is the smallest value an int can be
		int max = Integer.MIN_VALUE;
		if (a == null) {
			return max;
		}
		for (int i = 0; i < a.length; i++) {
			if (a[i] > max) {
				max = a[i];
			}
		}
		return max;

	}
}
```
- With the exaple above, when the method `maximumValue` is invoked (called), we can eliminate the name of the class because that method belongs to the same class (same file) as the line invokes the method. 

--- 
## Array of objects

- We know that arrays must be declared **and** instantiated, indicating how many slots there are in the array. 
- Keep in mind that arrays of objects in that each element must **also** be instantiated! 

**Here's a example of code that does all of the above:**

```
import java.awt.Color;

public class ArrayInstantiationExample {

	public static void main(String[] args) {
		int red, green, blue;
		// the next line declares the array of Colors
		Color[] twentyColors;
		// the next line instantiates the array to hold 20 references to Colors
		twentyColors = new Color[20];
		for (int i = 0; i < 20; i++) {
			// find a random value from 0-255 for each of red, green and blue components
			red = (int) (Math.random()*256);
			green = (int) (Math.random()*256);
			blue = (int) (Math.random()*256);
			// the next line instantiates each Color object itself
			twentyColors[i] = new Color(red, green, blue);
		}

	}

}
```
**key takeaways that you must remember**
- declare the array 
- instantiate the array
- instantiate each object within the array
---
# 2D Arrays: 
- Example of declaring: `double[][] numbers; `
- Example of instantiating (allocating): `numbers = new double[10][3]; `
- The number of rows given by `numbers.length`; 
- The number of columns in row is given by `numbers[i].length;`
- If the array is rectangular (not jagged), we can just use `numbers[0].length` for the number of columns 
- Use a nested for-loop to traverse all elements of the array 
- Use a single for-loop to traverse individual columns or individual rows 
- 2D arrays can be declared, instantiated, and the contents initialized in one line, for example: `int[][] intArray = {{0, 1, 2}, {1, 2, 3}, {2, 3, 4}. {3, 4, 5}};`

---
# Shallow vs Deep Copies: 
**Shallow Copy**

- Often not what you want to do in your code. 

**Example of Shallow Copy:**
```
Student student1 = new Student("Finn", 2187);
Student student2 = student1; // student2 is a shallow copy of student1
```

- Both `student1 and student2` refer to same object once the code above is executed because they are assigned to a variable. 
- Value stored in `student1` is a reference to an object we created of type Student. 
- Any changes to these two variables will cuse the same object to change. 

**Deep Copy**
- Usually our intent when copying an object is to create an entirely new object with the same information. 
- Shallow copy does not do this, rather copies reference to the same object. 
- To perform a deep copy, we need to create something called a **copy constructor** defined in the object we are copying. 

**Copy Constructor:**
- Really concept of overloading, applies to any methods tht share th same name but different number or ordering of parameters. 
- Constructors are methods, but more special ones because they can only be called using **new** and specifically used to create an instance of an object. 
- **copy constructor** refers to a constructor that takes in an instance of the class as a parameter and deep copies the instance variables from other object into this one. 

**Example of Copy Constructor:**
```
public class Student {
	private String name;
	private int id;
	
	public Student(String name, int id) {
		this.name = name;
		this.id = id;
	}

	// Copy constructor
	public Student(Student other) {
		if (other != null) {
			this.name = other.name;
			this.id = other.id;
		}
	}
	// .... rest of class defined below
}
```
- Critical aspect of copy constructors is keeping in mind that `other` could very well be null (despite programmer's best ability to prevention). 
- This is an issue that if attempt to access sanything of the form `other.<something>` such as `other.name` or `other.getID()`, a NullPointerExceptionError will get thrown.  
- To avoid, ensure `other` is not null before accessing anything inside `other`. 

- With copy constructor define for Student, we can crate a deeop copy over in main method: 

```
Student student1 = new Student("Finn", 1287);
Student student2 = new Student(student1); // deep copy
```
# Copy Construtor with Arrays 

- Arrays themselves are also objects, while tempting to type something like `this.array = other.array` this is simply a shallow copy. 
- Propr deep copy creating deep copies of each instance variable - simple task for primitive types. 
- If object being copies has instance variables that are also objects, we must also create deep copies of each of those objects. 

**For example:**
```int[] numbers1 = {1, 2, 3, 4, 5}; // example array
// First create an array (a new one!) of the same length
int[] numbers2 = new int[numbers1.length];
// Then copy in the values from the original into the copy
for (int i = 0; i < numbers1.length; i++) {
	numbers2[i] = numbers1[i];
}
```
- If student class had an array of `ints` as an instance variable, the copy constructor would have code much like this to copy the other student's array into this one's. 
- Same logic applies to copying an array of objects by first instantiating an array of the same length as one being copied. 
- Then copy over each element from original into new one, twist here is each copy constructor should be used for each element to creat deep copies of each object in the array. 

**For example:**
```
public class Student {
	private String name;
	private int id;
	private Grade[] grades;
	
	public Student(String name, int id) {
		this.name = name;
		this.id = id;
		this.grades = new Grade[4];
	}
	
	// Copy constructor
	public Student(Student other) {
		if (other != null) {
			this.name = other.name;
			this.id = other.id;
			// First create a new array of the same length as other.grades
			if (other.grades != null) {
				this.grades = new Grade[other.grades.length];
				for (int i = 0; i < this.grades.length; i++) {
					// Use the copy constructor for Grade on each element
					if (other.grades[i] != null) {
						this.grades[i] = new Grade(other.grades[i]);
					}
				}
			}
		}
	}
	// .....
}
```
---
# Inheritance 

- Inheritance allows to extend a class to create a derived class. 

**Consider the following:**
```
public class Pet {
	
	private String name;
	
	public Pet() {
		this.name = "No Name";
	}
	
	public Pet(String name) {
		this.name = name;
	}
	
	// copy constructor
	public Pet(Pet other) {
		if (other != null) {
			this.name = other.name;
		}
	}
		
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String toString() {
		return "Pet name: " + this.name;
	}
	
	public boolean equals(Pet anotherPet) {	
		//Avoid accidentally accessing a null pointer
		if (anotherPet==null) {
			return false;
		}
		// If "this" and anotherPet actually refer to the
		// identical object, then of course they are equals
		if (this == anotherPet) {
			return true;
		}		
		// If the names are the same, we count them as equals	
		return this.name.equals(anotherPet.name);
	}
	
}
```
# Dervied classes 
- Also called subclasses or child classes extend base classes (also called superclasses or parent classes) using `extends` operator. 

**Example:**
```
public class Dog extends Pet {
	private String breed;
	
	public String getBreed() {
		return breed;
	}
	
	public void setBreed(String breed) {
		this.breed = breed;
	}
}
```
- Most important part is class header - `Dog extends Pet` which dog class inherits all instance variables and methods of the class `Pet` 
- Means any instance of `Dog` can invoke the getName() and setName() methods, despite the fact these methods not being defined inside `Dog` class - essentially free since they are inherited. 
- `Private` keyword still stands within dervied classes despite `Dog` extending `Pet` in this case, `Dog` cannot directly access instance variable `name` in `Pet` because its a private instance variable. 


- No constructor for `Dog` exists, within each constructor, derived classes can and should invoke the constructor of their base class by using `super`. 
- Handy when the derived class needs to set the instance variable declared in parent class - keep in mind that `super` must be the first line inside the construtor of the subclass. 

**Example:**
```
	public Dog()
	{
		super();
		this.breed = "Unknown";
	}
	
	public Dog(String name, String breed) {
		super(name);
		this.breed = breed;
	}
	
        // copy constructor
	public Dog(Dog other) {
		super(other);
		if (other != null) {
			this.breed = other.breed;
		}
	}
```
- First is a no-argument constructor - though Java will by default call `super()` as the first line of any no-argument constructor. 
- Second constructor takes a name and a breed as parameters - `super(name)`, which invokes the constructor in `Pet` taking a name as a parameter and stores it in its instance variable `name`. 
- Third constructor is a copy constructor, natrually taking a `Dog` object as a parameter - first line, it invokes the copy constrcutor for the `Pet` class in which the breed is then set, after checking that the passed parameter is not null. 

**Example of code testing these classes:**
```
Dog myDog = new Dog("Obi", "Tibetan Terrier");
System.out.println(myDog);
``` 
- Creating `Dog` object called `myDog`, initialized with name "Obi" and breed "Tibetan Terrier", then output the `Dog` object. 
- The `toString` method is so far not defined inside `Dog`, but it is defined inside `Pet` so the `toString` method is inherited by the `Dog` class. 

# Overriding Methods

- We prefer if our `toString` method in the `Dog` class also output the breed of the dog. 
- To accomplish, an override method for `toString` that `Dog` inherited from `Pet` must be written. 
```
public String toString() {
		return super.toString() + "\tBreed: " + this.breed;
	}
```
- Notice here thaht `toString` method in `Dog` class is allowed to invoke the `toString` method already implemented in the `Pet` class by using the expression `super.toString()`. 
- Note that the code above must go in the Dog class. 

# The `equals` method: 
- Implement `equals` method in the `Dog` class, for it to do its job, this method is allowed to invoke the `equals` method of its base class with the expression `super.equals(otherDog)`. 
```
public boolean equals(Dog anotherDog) {
		// Avoid accidentally accessing a null pointer
		if (anotherDog == null) {
			return false;
		}		
		// If this object and anotherDog actually refer to the
		// identical object, then of course they are equal
		if (this == anotherDog) {
			return true;
		}		
		// Check if all instance variables match. Use the equals method
		// of the Pet class to check instance variables owned by the parent
		return super.equals(anotherDog) && this.breed.equals(anotherDog.breed);		
	}
```
- Note that to check if Strings are equal, we need to use the equals method in the String class (using == will check if the two String reference the same object in memory, not if they contain the same characters). 
---
# Upcasting

**Consider the following code:**
```
Pet pet = new Dog();
```
- Code is valid, compiles and runs just fine - this tells us that the `Pet` variable is being assigned the value of Dog object, **but is being used as if it were of type Pet**. 
- Known as upcasting - we are effectively casting an object of type Dog to be of type Pet which is fine because Dog is a subclass of Pet. 
- Note the following is not valid: 
```
Dog dog = new Pet(); // invalid code
``` 
- Here an attempt to upcast Pet to Type Dog is tried. However, upcasting only works in the upward direction in the inheritance hierarchy. 
- Dog inherits all instance variables and methds from Pet, so an instnace of Dog could be used as if it were a Pet - however, pet does not have all the instance variable and methods a Dog has, so reverse cannot be done. 
- Since `pet` is of type Pet, only methods defined inside of Pet on `pet` can be used because Dog inherits all these methods, this is completely valid. 

# Polymorphism
- Known as late binding, refers to the idea that a method with the same name can be defined in multiple classes within the same hiearchy. 
- Method definition that Java ultimately invokes is always the lowest in the hierachy - allows us to implement many version of a single method, and the correct one will be detected and used at run-time. 
- Polymorphism comes into play when a dervied class (subclass) overrides a method in its base class (superclass). The getName() method call above is not an example of polymorphism because getName() is only defined in Pet, not in Dog. 

```
// in class Pet
public void makeNoise() {
    System.out.println("Pet makes a noise!");
}
```
- Suppose Dog overrides the method above with the following method definition: 

```
// in class Dog
public void makeNoise() {
    System.out.println("Woof woof!");
}
```
- If makeNoise is called on `pet`, the definition makeNoise() that is invoked is determined by polymorphism. 
- In other words, dervied class Dogs's makeNoise() is ultimately invoked instead of the superclass Pet's makeNoise() because `pet` was instantiated to be an instance of Dog. 
- Most common form of upcasting in the sense of how functionality we can utilize with the objcet despite restrictions is via method calls. 

**For instance:**
```
public static void dinnerIsReady(Pet[] pets) {
    if (pets != null) {
        for (int i = 0; i < pets.length; i++) {
            if (pets[i] != null) {
                pets[i].makeNoise();
            }
        }
    }
}
```
- Taking advantage of inheritance and polymorphism, passing in an array of Pet objects that are instantiated with instances of Dog, Cat, GuineaPig, Horse or whatever else pet defined. 
- Since Pet has makeNoise() defined, the call to makeNoise above will always work. 
- If subclasses we define override makeNoise (make own definition of makeNoise) those defintions of makeNoise will be used. 
- Convienient since this means objects will always work as we expect them to so long as when overridden the methods are specific to subclassess. 

--- 
# Abstract Classes 
- Suppose there was a desire to set up code so that when creating Dog, Cat, and Horse objects, but no Pet objects unless they are also instances of Dog, Cat or Horse - this can be done with abstract classes. 

**Example of implementation of the pet class:**
```
public abstract class Pet {
	private String name;
	
	public Pet() {
		this.setName("No name"); // placeholder
	}
	
	public Pet(String name) {
		this.setName(name);
	}
	
	/* copy constructor */
	 public Pet(Pet p)
	 {
		 this.name = p.name;
	 }
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String toString() {
		return "Pet name: " + this.name;
	}
	
	
	public boolean equals(Object other) {
		if (other == null || other.getClass() != this.getClass()) {
			return false;
		} else {
			Pet otherPet = (Pet) other;
			return this.name.equals(otherPet.name);
		}
	}
	
	public abstract void makeNoise();
}
```
- When a class is defined as an abstract objects, not allowed to instantiate objects of that class.
- It may be referenced to the class, but not create objects 


---


## References:

**Consider the following:** 

`int number;`

`Counter counter;` 

Number in this is of primitive type `int`

Counter refers to an object of type **Counter**

Within Java `int number` allocates memory storage to hold **number** whereas, 
`Counter counter` just simple stores a reference (or address) of Counter object which
has not been created 

The variable **counter** is intended to refer to an object, but since it has yet to be instantiated an object using **new** it will refer to nothing (**null**)

--- 

### Example of Memory Diagrams:

Consider the following block of code: 

`int number = 7;`

`Counter counter = new Counter();`

`System.out.println("number = " + number);` 

`System.out.println("counter = " + counter);`

The output of thise program will depend on whether or not the user implemented **toString** method within the **Counter** class - within in Java, if the user prints an object and the **toString** metho is not defind, the memory location of the object will print to the console. 

If you remove the **toString** method from the Counter class, then it should becoe obvious from the output that **counter** is a reference (memory address). 

**Below is an example of a memory diagram:**


![counter](https://user-images.githubusercontent.com/83374820/157770295-4dedc7fb-3342-4f3b-8264-a4023145923a.jpg)

---
# ArrayLists 

**Creating and instantiaitng an ArrayList**

- Arrays are useful data structures for grouping and representing data conveniently. 
- Limited though since increasing and decreasing length is not possible.
- ArrayList class addresses this issue, regardless of initial length of an ArrayList since it will not cause an index out of bounds exception. 
- ArrayList will dynamically increase its size if the ArrayList is already full and trying ot add another element to it. 

**An example:**
```
ArrayList<String> avengers = new ArrayList<String>(); 
avengers.add("Iron Man"); 
avengers.add("Captain America"); 
avengers.add("Thor"):
avengers.add("Black Window"): 
avengers.add("Ant-man"): 
```
- Syntax different here than arrays, an _array_ of strings would need to be decalted and instantiated using square brackets (`String[] avengers = new String[10];`). 
- When indexing an array to get or add elements utilize square brackets (`avengers[0] = "Iron Man";)`. 
- Sytax more closely represents typical objects in Java and other methods that use parenthesis. 
- New things are the greater-than and less-than symbols which follow the ArrayList variable type and the type of object of the elements in the ArrayList go beteween them.
- add() method, given a single parameter, will append that object to the end of the ArrayList. 

# Itering over an ArrayList(for-loops and for-each loops): 

- Since arrayLists increase dynamically, the concepts of _capacity vs size_ are at play here. 
- "Capacity" refers to the actual length of the ArrayList in the background - which in Java we do not really care how its being handled. 
- "Size" refers to how many elements there are in the ArrayList - important for us since size() method is useful to iterate over contents in ArrayList. 
```
for (int i = 0; i < avengers.size(); i++) {
	System.out.println(avengers.get(i));
}
```
- ArrayLists are sytactically like most objects in Java, calling a method like get() on ArrayList to get element at the specified index. 
- Above loop is analogus to iterating i from 0 to the length of an array and indexing into the array using square brackets. 
- Useful thing about ArrayList is a simplified version of the typical for loop for iterating all over all elements. 
- Instead of creating an integer(called _i_), initalized to the value of 0, incremeneting it up to the size of the list, siimplified syntax below: 

```
for (String s : avengers) {
	System.out.println(s);
}
```
- For each loop above, the first thing to go in parentheses, type of element in the ArrayList, then a variable to use as the name to reference each element in the list in turn, then a colon, and finally name of the ArrayList. 
- 

