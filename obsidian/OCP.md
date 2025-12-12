### Building Blocks

```java
double annoyingButLegal = 1_00_0.0_0;  // Ugly, but compiles
```

- primitive types:
	- `boolean`
	- `byte`
	- `short`
	- `int`
	- `long`
	- `float`
	- `double`
	- `char`
- reference types example:
	- `String`

- a reference points to an object located in the heap.
- each primitive has a wrapper class.

```java
Integer.valueOf(1)
```

- `int value = null //DOES NOT COMPILE`
- if you don't know the value of `int` you can use its wrapper class, `Integer`
- wrapper classes have helper methods like: `min(int num1, int num2)`

---
### Operators
- **Operator Precedence** - the order in which the operators are evaluated.
- casting is an unary operation where one data type is interpreted as another data type.
- casting is required if you need to go from a larger numerical data type to a smaller numerical data type.
- `instanceof` operator can be used to check if an instance is a member of a particular class or interface. `System.out.println(t instanceof Testus)`
- logical operators:
	- `a & b`
	- `c | d`
	- `e ^ f`
- conditional operators:
	- `a && b`
	- `c || d`
- **Unperformed Side Effect** - https://en.wikipedia.org/wiki/Short-circuit_evaluation
```java
int rabbit = 6;
boolean bunny = (rabbit >= 6) || (++rabbit <= 7);
System.out.println(rabbit); // "6" will get printed
// the increment operator of the expression is never evaluated
```

```java
int rabbit = 6;
boolean bunny = (rabbit > 6) || (++rabbit <= 7);
System.out.println(rabbit); // "7" will get printed
// the increment operator of the expression will be evaluated
```

- **ternary operator** example
```java
int owl = 5;
int food = owl < 2 ? 3 : 4;
System.out.println(food); // 4
```

- ternary expression can also contain **unperformed side effect**.
```java
int sheep = 1;
int zzz = 1;
int sleep = zzz<10 ? sheep++ : zzz++ ;
System.out.println(sheep + "," + zzz); // 2, 1
```

---
### Making decisions
- boilerplate code is code that tends to be duplicated throughout a section of code over and over again in a similar manner.
- **Pattern matching** can help reduce *boilerplate code*.
```java
void compareIntegers(Number number) {
	if(number instanceof Integer) {
		Integer data = (Integer)number;
		System.out.print(data.compareTo(5));	
	}
}
```

```java
void compareIntegers(Number number) {
	if(number instanceof Integer data) { // Pattern matching, "data" is a pattern variable
		System.out.print(data.compareTo(5));	
	}
}
```

```java
if(tickets instanceof Double d && d < 10)
```

- it is a bad practice to reassign a pattern variable. Reassignment can be prevented by using `final`
```java
if(number instanceof final Integer data) { 
	data = 10; // DOES NOT COMPILE
}
```
- [cool article about pattern matching](https://www.freecodecamp.org/news/write-better-java-code-pattern-matching-sealed-classes/)
- **flow scoping**, maybe read this: https://www.objectos.com.br/blog/pattern-matching-and-flow-scoping-in-java.html
- **switch expression** (not the same as **switch statement**):
```java
String getAnimalBest(int type) {
	return switch (type) {
		case 0    -> "Lion";
		case 1    -> "Elephant";
		case 2, 3 -> "Alligator";
		case 4    -> "Crane";
		default   -> "Unknown";	
	};
}
```

- **switch** can be used with the assignment operator.
- for an **exhaustive switch**: 
	- add a `default` clause
	- if the `switch` takes an enum, add `case` for each possible enum value
	- cover all possible types of the `switch` variable with **pattern matching**
- it may be a good idea to always add a default clause even if you cover every type.
- if you want to return a value from a switch expression in case clause use the `yield` statement. if you use `return` it will exit the method itself.
- **pattern matching** in a **switch expression**: 
```java
String message = switch (height) {
	case Integer i -> "Rounded: " + i;
	case Double d -> "Precise: " + d;
	case Number n -> "Unknown: " + n;
	
	//you can use
	case Integer i when i > 10 -> "Rounded: " + i;
};
```

- you get a compiler error if the compiler detects *unreachable code*
- handle `null` when working with object types when using switch: `case null -> "test"` (whenever a switch uses `case null` it is considered that switch statement uses *pattern matching*)
- `do/while` loops guarantee that the statement or block will be executed at least *once*.

---
### Core APIs
- `String` is **immutable**.
- get substrings with the method `substring()`
- `toUpperCase()`, `toLowerCase()`
- to check if two strings objects contain the exactly same characters in the same order use `equals()`, `equalsIgnoreCase()`, don't use `==`, `==` is used only to check if two references point to the same object when working with reference types.
- remove whitespaces with `trim()`, `strip()`, `stripLeading()`, `stripTrailing()`
- `isEmpty()`, `isBlank()`
```java
System.out.println(" ".isEmpty()); //false
System.out.println("".isEmpty());  //true
System.out.println(" ".isBlank()); //true
System.out.println("".isBlank());  //true
```
- `format(), formatted()`
- **method chaining**
- `StringBuilder` is **not** immutable.
- `public StringBuilder reverse()`
- **string pool**
- `int[] moreNumbers = {42, 55, 99};`
- `Arrays.sort()`, `Arrays.binarySearch(numbers,2)`, `Arrays.equals(new int[] {1}, new int[] {1})`, `Arrays.mismatch()`, `Arrays.compare()`
- **varargs** (variable arguments): `public static void main(String.. args)`
- `Math.min()`, `Math.round()`, `Math.ceil()`, `Math.floor()`, `Math.pow()`, `Math.random()`
- `BigInteger`, `BigDecimal`
- `LocalDate.now()`, `LocalDate.of()`, `LocalTime.of()`, `date.plusYears(5);`

### Methods
- **Effectively final variables**
- access modifiers: `public`, `private`, `package access`, `protected`
- when `static` is applied to a variable, method, or class, it belongs to the class rather than a specific instance of the class.
- optional modifiers:
	- `static`
	- `abstract`
	- `final`
	- `default`
	- `synchronized`
	- `native`
	- `strictfp`
- local variable modifiers:
	- `final`
- instance variable modifiers:
	- `public`
	- `protected`
	- `private`
- optional instance variables:
	- `final`
	- `volatile`
	- `transient`
- Java is a "**pass-by-value**" language, a copy of the variable passed as a paramenter is created and the method receives that copy.
- **autoboxing** and **unboxing** variables.
	- **autoboxing** is the process of converting primitive types to the object of their corresponding wrapper class.
	- **unboxing** is the reverse process of **autoboxing**
```java
int quack = 5;
Integer quackquack = quack ;       //autoboxing
int quackquackquack = quackquack ; //unboxing
```
- **method overloading**
- **autoboxing** applies to **method overloading** as well:
```java
public class Kiwi {
	public void fly(int numMiles) {}
	public void fly(Integer numMiles) {}
}
```

### Class Design
- `final` class: the class may not be **extended**.
- `abstract` class: the class cannot be **instantiated**, it may contain **non-abstract** and **abstract** methods (**concrete methods**).
- `sealed` class: may only be extended by a specific list of classes specified by the programmer. 
```java
sealed class Vehicle permits Car, Bike {}
```
- `non-sealed` class: can be applied to a subclass of a `sealed` class. ?
- **single inheritance**, **multiple inheritance**
- **constructor overloading**
- **private constructors** prevent instantiation. useful when a class has only static methods.
- you can't declare a top level class nor **private** or **protected** (compiler error)
### Beyond Classes

### Exceptions and Localization
- Exceptions are used when "something goes wrong"
![[categories_of_exception.png]]
- **checked exceptions**: should be declared and handled by the application (example: `FileNotFoundException`), all exceptions that inherit `Exception` but not `RuntimeException` are **checked exceptions**
- **unchecked exceptions**: does not need to be declared and handled by the application. they are often considered runtime exceptions. **unchecked exceptions** include any class that inherits the `RuntimeException` or `Error` class.
- `ArrayIndexOutOfBoundsException`
- `Exception` is an `Object` , you can store it in a variable.

```java
throw new RuntimeException() // correct way of throwing an exception
throw RuntimeException() // DOES NOT COMPILE!
```

```java
try {
	throw new RuntimeException();
	throw new ArrayIndexOutOfBoundsException(); // DOES NOT COMPILE, UNREACHABLE CODE
} catch (Exception e) {}
```
- `javac -g:vars Frog.java`
  `java Frog`
- you always need the braces in a try catch finally statement
- `NumberFormatException` is a subclass of `IllegalArgumentException`
- *multi-catch block*

```java
public static void main(String[] args) {
	try {
		System.out.println(Integer.parseInt(args[1]))
	} catch (ArrayIndexOutOfBoundsException | NumberFormatException e) {
		System.out.println("Missing or invalid input");
	}
}

catch(Exception1 e | Exception2 e | Exception3 e)    // DOES NOT COMPILE
catch(Exception1 e1 | Exception2 e2 | Exception3 e3) // DOES NOT COMPILE
catch(Exception1 | Exception2 | Exception3 e)        // COMPILES

```

```java
var f = DateTimeFormatter.ofPattern(" MMMM dd, yyyy 'a' hh::mm");
// escape " ' " with ''
System.out.println(dt.format(f));  // October 20, 2025 at 06:15
```
- return codes should generally be avoided. use Java's exception framework.
- only classes that implement the `AutoClosable` interface can be used in a try-with-resources statement.
- external data sources are referred to as *resources*. (for example, *files*, *databases*, and various connection objects)
- *resource leak*
- the `catch` block is optionally when `finally` is used.
- the `finally` block always executes whether or not the an exception occurs.
 ![[try_catch_finally.png]]
- *Internationalization (I18N)* provides support for various countries, various languages and various currencies automatically without performing any change in the application.

```java
var loc = Locale.getDefault();
System.out.println(loc); // "en_US"
System.out.println(Locale.GERMAN);
System.out.println(Locale.GERMANY);
System.out.println(Locale.of("fr")); // fr factory method "of()"
System.out.println(Locale.of("hi", "IN"));

Locale l1 = new Locale.Builder()
	.setLanguage("en")
	.setRegion("US")
	.build()
	
Locale l2 = new Locale.Builder()
	.setRegion("US")
	.setLanguage("en")
	.build()

Locale locale = Locale.of("fr");
Locale.setDefault(locale);
```

- a *resource bundle* is stored in the *properties file* which is a text file in a specific format with key/value pairs.
```
Zoo_en.properties
hello=Hello
open=The zoo is open

Zoo_fr.properties
hello=Bonjour
open=Le zoo est ouvert
```

```java
var locale = Locale.of("fr", "FR");
var rb = ResourceBundle.getBundle("Zoo", locale);
System.out.println(rb.getString("hello") + ", " + rb.getString("open")); // Bonjour, Le zoo est ouvert

rb.keySet().stream()
	.map(k -> k + ": " + rb.getString(k))
	.forEach(System.out::println)
// hello: Hello
// open: The zoo is open
```

- `MissingResourceException` is thrown if no locale or default bundle is available.
### Lambdas and functional interfaces
```java
a -> { return a.canHop(); }
(Animal a) -> a.canHop()
```

- while writing a lambda, parantheses are *optional* only when you have only *one* parameter.
- a *functional interface* is an interface that has only *one* abstract method.
- you can use the `@FunctionalInterface` annotation.
- if a functional interface includes an abstract method with the same signature as a public method found in object, *those methods do not count tward the single abstract method test*. (like `public String toString()`)
- declaring an abstract method like `int toString()` in an interface would not compile.
```java
public interface Dive { //VALID FUNCTIONAL INTERFACE
	String toString();
	public boolean equals(Object o);
	public abstract int hashCode();
	public void dive(); // dive is the single abstract method.
}
```

```java
public interface Hibernate { // this is not a functional interface
	String toString();
	public boolean equals(Hibernate o); // this counts as an abstract method because it does not match Object's "equals(Object o)"
	public abstract int hashCode();
	public void rest(); // abstract method
}
```
- functional interfaces can be used with lambda expressions and method references.
- **Method reference** example: `LearnToSpeak learner = System.out::println;`
- `::` is like a lambda.
- you can pretend the compiler turns **method references** to **lambda** for you.
- *ambiguous type error*
- more examples of **method references**:
```java
interface Converter {
	long round(double num);
}

Converter methodRef = Math::round;       // METHOD REFERENCE
Converter lambda = x -> Math.round(x);   // LAMBDA
System.out.println(methodRef.round(100.1));  // 100
```

```java
interface StringStart {
	boolean beginningCheck(String prefix);
}

var str = "zoo";
StringStart methodRef = str::startsWith;     // METHOD REFERENCE
StringStart lambda = s -> str.startsWith(s); // LAMBDA

System.out.println(methodRef.beginningCheck("A"));
```

```java
interface StringChecker {
	boolean check();
}

var str = "";
StringChecker methodRef = str::isEmpty();
StringChecker lambda = () -> str.isEmpty();

System.out.println(methodRef.check());
```

```java
list.forEach(s -> System.out.println(s)) // lambda
list.forEach(System.out::println);       //method reference
```
![[common_general_purpose_functional_interfaces.png]]

- you can use `Supplier<T>` for lazy generation. 
	- You create the object only when you need it.
	- memory wise: the object is stored ONLY when needed.
- **deferred computation**: "don't compute the result now, compute it later, when (and if) it's actually needed."
- `Consumer<T>`: takes a value and doesn't return anything.

```java
Consumer<String> printer = System.out::println;
printer.accept("sus"); //prints "sus"
```

- `BiConsumer<T, U>`: takes two values and doesn't return anything.

```java
HashMap<String, Integer> map = new HashMap<>();
BiConsumer<String, Integer> b1 = map::put;

b1.accept("sus1", 1);
b1.accept("sus2", 2);

System.out.println(map.toString())    // prints "{sus1=1, sus2=2}"
```

- `Predicate<T>`: you can use a *predicate* to test a condition. "you use `Predicate` when you want to treat a condition as a variable or argument - not just evaluate it immediately."
- `BiPredicate<T,U>`: same as `Predicate<T>` but takes two values. 
```java
BiPredicate<String, String> p1 = String::startsWith;
Predicate<String> p2 = String::isEmpty;

System.out.println(p2.test("")); // true, string is empty.
System.out.println(p1.test("chicken", "chick"));
```

- also notice the method reference in each example.
- `Function<T, U>`: takes a value of type `T` and returns a value of type `U`.
- `Function<T, R>` lets you pass behaviour as data."
```java
Function<String, Integer> f = String::length;

var length = f.apply("wow");
System.out.println(length); //prints "3"
```

- take two strings and produce another string:
```java
BiFunction<String, String, String> bf = String::concat;
System.out.println(bf.apply("baby ", "chick")); // baby chick
```

- `UnaryOperator` require all type parameters to be the same type. it transform its value into one of the same type.
- `BinaryOperator` merges two values into one of the same type.
- `UnaryOperator` and `BinaryOperator` extend `Function`.

```java
UnaryOperator<String> u1 = String::toUpperCase;
System.out.println(u1.apply("chirp")); // CHIRP
```

```java
BinaryOperator<String> b1 = String::concat;
System.out.println(b1.apply("baby ", "chick")); // baby chick
```

![[general_purpose_functional_interfaces_convenience_methods.png]]

```java
Predicate<String> egg = s -> s.contains("egg");
Predicate<String> brown = s -> s.contains("brown");

Predicate<String> brownEggs = egg.and(brown);
Predicate<String> brownEggs = egg.and(brown.negate());
```
- you can add modifiers and annotations to lambda parameters.
- all lambda parameters **HAVE TO** be in the same format.
```java
(a, b) -> {int a = 0; return 5;} // DOES NOT COMPILE, "a" redeclaration
```
- lambdas **cannot** access variables that are **NOT** **final** or **effectively final** (variables that get reassigned, in case of an effectively final variable.)

---
### Collections and Generics
- `Map` doesn't implement the `Collection` interface.
![[collection_interface.png]]
- the *diamond operator* (*<>*)
```java
List<Integer> al1 = new ArrayList<Integer>(); //COMPILES
List<> al2 = new ArrayList<Integer>(); // DOES NOT COMPILE
List<Integer> al3 = new ArrayList<>(); // COMPILES

var al4 = new ArrayList<Integer>();
```
- don't specify the generic type of both sides of the `=`, that's redundant
```java
var map = new HashMap<>(); //COMPILES, the inffered type is Object
```

- `public boolean add(E element)` adds an ellement to the collection, and it returns whether it was successful.
- if you try to add the same element twice in a `Set`, `add(E element)` will return false.
- `Set` does not allow duplicates. `ArrayList` does.
- `public boolean remove(E element)`
- `public void clear()`
- `public boolean contains(Object object)`
- `public boolean removeIf(Predicate<? super E> filter)`
- `boolean equals(Object object)`
- all `List` implementations are *ordered* (the elements keep the same order in which you added them)
- `List` allow duplicates.
- `ArrayList` : constant time when you're trying to access an element. It is slower when adding or removing an element than accessing an element.
- a `LinkedList` implements both `List` and `Deque` . It has all the methods of `List` and it also has additional methods for adding/removing elements from the beginning/end.
- `list.toArray()`, convert a list to an array.
- a `HashSet` uses a hash table. a hash for the key and an `Object` for the value. It uses `hashCode()` to retrieve them more efficiently.
- `LinkedHashSet` is basically: `HashSet` + `LinkedList`. it also has methods for adding/removing elements from the front or back of the set. ordered.
- `TreeSet` stores the elements in a natural sorted order (alphabetically or numerically).
![[hashset_linkedhashset_treeset.png]]
- `Set<Character> letters = Set.of('c', 'a', 't')`
- `Set<Character> copy = Set.copyOf(letters)`
- `HashSet`: *arbitrary order*, it can happen that the elements are not ordered or sorted.
- `hashCode()` is used to know which "bucket" to look in and `equals()` is used to determine equality between objects.
- `Deque` (interface) works like `Queue` but you can add/remove elements from the front (head) and the back (tail).
- **FIFO** (first in first out) - works like a line of people, you get on in the back and off in the front.
- **LIFO** (last in first out) - works like a stack of plates. you put the plate on the top and take it off from the top.
- **double-ended queue** - uses both ends
- `LinkedHashMap` is ordered. `TreeMap` is sorted.

```java
Map.of("key1", "value1", "key2", "value2");
```

- `Collections.sort();` returns void.
![[sequencedcollection_methods.png]]

![[sequencedmap_methods.png]]
- `Collections.unmodifiableMap`, `Collections.unmodifiableCollection`, `Collections.unmodifiableList`, `Collections.unmodifiableSet`
- trying to modify and unmodifiable view throws `UnsupportedOperationException` at **runtime**
- types of bounds:
	- `unbounded wildcard` : `List<?> a = new ArrayList<String>();`
	- `wildcard with upper bound`: `List<? extends Exception> a = new ArrayList<RuntimeException>();` 
		- any type that is a subtype of `T` (including `T` itself).
		- subtypes of `Number`: `Integer`, `Double`, `Float`,...
	- `wildcare with lower bound`: `List<? super Exception> a = new ArrayList<Object>();`
		- any type that is a supertype of `T` (including `T` itself).
		- supertypes of `Integer`: `Integer`, `Number`, `Object`


---

### Concurrency
- a thread is the smallest unit of execution.
- a *process* is a group of associated threads.

- Thread creation:
```java
class MyThread extends Thread { 
	@Override
	public void run() {
		System.out.println("sussy baka!");
	}
}

public class Example {
	public static void main(String[] args) {
		var t1 = new MyThread();
		t1.start(); // start() creates and automatically calls run()
	}
}
```

- Thread creation using the Runnable interface:
```java
class MyRunnable implements Runnable {
	@Override
	public void run() {
		System.out.println("sussy baka!");
	}
}

public class Example {
	public static void main(String[] args) {
		var t1 = new Thread(MyRunnable);
	}
}
```

- Thread creation using a **lambda expression**:
```java
Thread t1 = new Thread(() -> System.out.println("sus"));
t1.start();
```

```java
void sing() {
	synchronized(this) {
		System.out.println("La la la!");
	}
}

synchronized void sing() {
	System.out.println("La la la!");
}
```

- `synchronized` acquires and releases the lock automatically.
-  the `Lock` interface provides explicit control. You have to manually acquire and release the lock.

```java
var object = new Object();
synchronized(object) {
	// protected code
}

var myLock = new ReentrantLock();
try {
	myLock.lock();
	// protected code
} finally {
	myLock.unlock();
}
```

- you use `ReentrantReadWriteLock` when dealing mostly with reading data.
	- a **read lock** - allows multiple readers to access the resource simultaneously (as long as no write lock is writing)
	- a **write lock** - allows only one writer, and it blocks all readers and other writers until it's done.
	
![[why_does_ReentrantReadWriteLock_exist.png]]

- if you `lock.readLock().lock()` before `lock.writeLock().lock()` the program will wait on forever.
- if you want to let threads continue execution only after all threads have reached a "barrier", use `CyclicBarrier`

```java
var hm = new HashMap<String, Integer>();

hm.put("elephant", 1);
hm.put("raccoon", 2);

for (var key : hm.keySet()) {
	hm.remove(key); //ConcurrentModificationException!!!!
}

// use ConcurrentHashMap instead!!
```

- if you work with immutable collections and immutable object there is no need for synchronization since their state can't be modified by threads. (no memory consistency error)
-  `CopyOnWrite` classes, save a copy of the collection when a reference is added, removed, or changed and then update the original collection reference to point to the copy.
- *Liveness* issues:
	- `Deadlock`: occurs when two or more threads are blocked forever, each waiting on each other.
	- `Starvation`: occurs when a single thread can't access a shared resource (denied access)
	- `Livelock` : occurs when multiple thread are blocked forever, even though they are each still active and trying to complete their task. "Livelock is often a result of two or more threads trying to resolve a *deadlock*"
- a *race condition* occurs when two tasks that should be completed sequentially are completed at the same time and the result is unpredictable.

- generate random values in a **multithreaded** program:

```java
ThreadLocalRandom.current()
	.ints()
	.limit(5)
	.forEach(System.out::println);
```

- `stream()` works sequentially, `parallelStream()` work in parallel (multiple threads), faster.
- use `.forEachOrdered()` when working with `parallelStream()` if you care about the elements being ordered. (the order in which they are defined)

```java
IntStream.range(1, 100).parallel().sorted().forEach(System.out::println)
// again, use forEachOrdered() if you need to guarantee ordering on a parallel stream
```

- it is recommended to use the three-argument version of `reduce()` when working with parallel streams !!! (problematic accumulator)
- *stateful lambda* : the output depends on *mutable external state*

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
List<Integer> results = new ArrayList<>();

numbers.parallelStream()
	.map(n -> {
		results.add(n * 2);  // modifies shared state!!!!
		return n * 2;
	})
	.forEach(System.out::println);
```

- *stateless (safe) lambda*

```java
List<Integer> numbers = List.of(1,2,3,4,5);
List<Integer> results = numbers.parallelStream()
							.map(n -> n*2) // no external mutation
							.collect(Collectors.toList()); 
```

- 



### static block vs instance initializer block
 - In Java, **a static block executes code before the object initialization** (once during the class loading). A static block is a block of code with a `static` keyword.
 - the **instance initializer** block initializes the instance data during **instance creation**.
 
### static variables
 - **static variables** are shared among all objects, they are like global variables.
### Serialization and Deserialization
- serialization is the process of converting an in memory object to a format that can be easily transmitted. byte stream.
- use the `transient` keyword for objects or data that you don't want to serialize like a password.
- how to make a class serializable:
	- the class must be marked `Serializable`
	- every instance member of the class must serializable, marked `transient` or have a `null` value.

### Records
- a **record** is a class that automatically provides:
	- immutable fields (`final` by default)
	- a contructor
	- getters (accessors)
	- `equals()`, `hashCode()`, `toString()`
	- generated by the compiler automatically

### Logger
- usually used for logging and debugging instead of System.out....
```java
private Logger logger = Logger.getLogger("info");
logger.log(Level.INFO, "wowies");
```


