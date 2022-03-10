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
- Keep in mind thaht arrays of objects in that each element must **also** be instantiated! 

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




