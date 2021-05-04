# Java Intricacies

- If there is atleast one abstract method in a class, the class has to be marked as abstract.
- If a class extends an abstract super class, it must implement all the methods that are marked as abstract. Or this class to has to be marked as abstract.
## What did Java 8 add?.
- Lambda expressions
- Method references
- Stream API
- https://www.tutorialspoint.com/java8/java8_overview.htm#:~:text=JAVA%208%20is%20a%20major,%2C%20new%20streaming%20API%2C%20etc.

## How to make a class as immutable in Java, just like String class?.
- Dont provide any setter methods
- Mark all the variables are private and final.
- To prevent subclasses from overriding any methods, mark the class as final.
- If there are any reference properties in the class, dont provide setter methods for them.
- Also provide private constructors with static factory methods.
- https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html

## What is the use of declaring a class as final in Java?.
- Final prevents a class from getting subclasses.
- If you take String, it is an immutable class. Using final to a class helps achieving this.
- Typically when designing frameworks, always start with final classes and then expose as needed.
- This is because if the users subclass and override public methods, how it works may be complex to understand in early stages.

## Class modifer visibility in Java
![image](https://user-images.githubusercontent.com/42272776/116924446-82c48380-ac75-11eb-8182-246b6f28fac6.png)

## Time complexity of String concatenation in Java?.

## How to use Stringbuffer class
```
StringBuffer buf = new StringBuffer();
buf.append("Pavan");
buf.getCharAt(index);
buf.setChartAt(index, 'a' );
buf.toString();
```

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

## Generics in Java

## HashMap vs HashTable
- HashTable is thread safe as it is synchronized. HashMap is not.
- HashTable is consequently slower. Also it is an old data structure.
- We can synchronize the methods that put and get data from HashMap which typically perform better than HashTable.
- Or you can use Collections.synchronizedMap(), which returns a Map.
- HashTable does not allow null key. HashMap allows on key with null value.

## When are static variables initialized?.
- Static variables are initialized only once when the class is loaded by the class loader.
- Static methods with immutable variables are thread safe.

# Design Patterns in Java

## Singleton Design Pattern
- When a single instance of the class is supposed to exist.
- Examples are loggers, registries etc.
- The below implementation is not thread safe. This means that more than one instance may actually get created.
```class Singleton
{
	private static Singleton instance;
	private Singleton()
	{
		System.out.println("Constructing object...");
	}
	
	public static Singleton getInstance()
	{
		if( instance = null )
			instance = new Singleton();
		System.out.println("Serving object...");
		return instance;
	}
}
```
- There are two possibilities here to address this.
- First approach: Using Synchornized key word to the getInstance method.
- By adding this key word, we force every theread to wait its turn before it can enter the method.
- In other words, no two threads can enter this method at the same time.
```class Singleton
{
	private static Singleton instance;
	private Singleton()
	{
		System.out.println("Constructing object...");
	}
	
	public static synchronized Singleton getInstance()
	{
		if( instance = null )
			instance = new Singleton();
		System.out.println("Serving object...");
		return instance;
	}
}
```
- Second approach: Using JVM for managing the object creation.
- Key thing here is that the object is created in the class definition as a static instance directly.
- Since static will ensure that the initialization happens only once, this method works out.
- This is early initialization compared to the lazy initialization described above.
```
class Singleton
{
	private static Singleton instance = new Singleton();
	private Singleton()
	{
		System.out.println("Constructing object...");
	}
	
	public static Singleton getInstance()
	{
		System.out.println("Serving object...");
		return instance;
	}	
}
```

## Explain the template method design pattern.
- Abstract class Lucene defines a template method called search.
- Search has some methods which are defined in this class itself and also some methods which are expected to be overridden in the subclass implementations.
- These methods are marked as abstract and so the base class itself is an Abstract base class.
- Subclasses can have methods which override these and provide their own implementations.
- Key thing to note here is that the reference used is of type of Super class.
```
abstract class Lucene
{
	public final void search()
	{
		buildIndex();
		treeTraversal();
		collectFrequencies();
		displayResults();
	}

	private void displayResults() {
		System.out.println("Displaying Search Results");
	}

	abstract void collectFrequencies();
	abstract void treeTraversal();
	abstract void buildIndex();
}

class Solr extends Lucene
{
	@Override
	void collectFrequencies() {
		System.out.println("Collecting Inverse Term Frequencies");
	}

	@Override
	void treeTraversal() {
		System.out.println("Applying BFS Traversal");
	}

	@Override
	void buildIndex() {
		System.out.println("Building Index Top Down");
	}
}

class ElasticSearch extends Lucene
{
	@Override
	void collectFrequencies() {
		System.out.println("Collecting Term Frequencies");
	}

	@Override
	void treeTraversal() {
		System.out.println("Applying DFS Traversal");
	}

	@Override
	void buildIndex() {
		System.out.println("Building Index Bottom Up");
	}
}
```


