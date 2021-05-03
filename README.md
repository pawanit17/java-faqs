# Java Intricacies

## What did Java 8 add?.
- Lambda expressions
- Method references
- Stream API
- https://www.tutorialspoint.com/java8/java8_overview.htm#:~:text=JAVA%208%20is%20a%20major,%2C%20new%20streaming%20API%2C%20etc.
## What is the use of declaring a class as final in Java?.

## Time complexity of String concatenation in Java?.

## How is StringBuffer class implemented in Java?.
- StringBuffer is backed by a char array.
- The size of the char array doubles if it overflows.
- So sometimes append can be slow.
- When you do an append operation, the content gets persisted onto the char array. If the size of new content is more than the array capacity, a new larger
  char array shall be used instead.
- https://stackoverflow.com/questions/8011338/how-is-stringbuffer-implementing-append-function-without-creating-two-objects

## What are String constant pools in Java?.
- A part of Java HEAP to store string literals. This serves as a cache.
- This reduces the number of strings that are created by the JVM.
![image](https://user-images.githubusercontent.com/42272776/116812197-165d5d80-ab6b-11eb-9a1d-a2c25ebcab0f.png)
- Note that Strings created with *new* dont get cached here.
![image](https://user-images.githubusercontent.com/42272776/116812216-2bd28780-ab6b-11eb-9347-4babe59cffbb.png)

## What is intern method in String
- Intern method is used to add a String to the String literal pool.
![image](https://user-images.githubusercontent.com/42272776/116812298-bdda9000-ab6b-11eb-8b9b-8ccaf2860f39.png)
```
    String str = new String("Axar");
    String str3 = str.intern();
    String str1 = "Axar";
    String str2 = "Axar";
    System.out.println( (str1 == str2) + " " + (str == str1) + " " + (str3 == str1) );
```
- The above code returns *true false true*.
- Note that the *intern* method does not change state of the String, because String is immutable.
- Instead, it will place the entry in the String literal constant pool and returns a reference to.
- This is the same entry that other String literals would point to later.

## Why are Strings immutable in Java?.
- They help in Thread-safe implementations because they are immutable - no race conditions.
- They make String literal constant pool implementation possible - strings can be cached because of the promise that they cannot changed under the water.
- Caching hashcode values. HashCode value helps in locating keys in data structures like HashSets, HashMaps. Hash code value of an object is built based on its internal
  state. So if the internal state does not change, then its hash code too does not change. That is why having immutable as keys to data structure is beneficial.

## How can you write a hook like Arrays.sort() comparator in Java?.

## Override toString method in Java
- toString method lets a class override so that it can have its own version of it.
- By default the *toString* method on Object class returns the *hashcode* information like - *Employee@7a81197d*
- Ex, this method is overridden at String class.
```
public class Experiments 
{
	public static void main( String args[] )
	{
		Employee e1 = new Employee();
		e1.id = 1;
		e1.name = "Pavan";		
		
		Employee e2 = new Employee();
		e2.id = 2;
		e2.name = "Kumar";
		
		System.out.println( e1.toString() );
		System.out.println( e2.toString() );
	}
}

class Employee
{
	String name;
	int id;
	
	Employee()
	{
		
	}
	
  @override
	public String toString()
	{
		return name + id;		
	}
}
```
**
Employee@7a81197d
Employee@5ca881b5

vs

Pavan1
Kumar2**


## hashCode and equals methods on Object
- hashCode and equals methods are used when we try to add objects to hash based containers like HashMap or HashSet.
- To see if an object exists as a KEY in the container, it needs to look use these methods.
- If you have a custom object and two such objects are treated equal based on one of their properties, then you need to override equals obviously.
  - Ex: User usr1 = new User( "pdittaka", 89999999999 ); and User usr2 = new User( "pdittaka", 9999999999 ) are logically equal but have different memory addresses as
    they are two different instances.
  - So you override equals such that if the userid is same, you consider them to be equal.
  - In such cases, you also need to update hashCode method. The reason is when you added the first usr1 to the container and if you try to add the second usr2, the hash that
    gets computed should be same to prevent inconsistent results. By default, the hashCode method from Object will not contain processing for your userid attribute and will
    give two different hashes.
  - That is why, if you override equals, you should also override hashCode.
  - The otherway is not true ofcourse. If you override hascode, it means you are changing the hashing algorithm. This is not related to the working of equals method.
- https://www.tutorialspoint.com/java8/java8_overview.htm#:~:text=JAVA%208%20is%20a%20major,%2C%20new%20streaming%20API%2C%20etc.
## Interfaces vs Abstract Classes

## Lambdas in Java

## Threads in Java

