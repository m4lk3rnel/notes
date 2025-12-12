- **Callable** interface - similar to the Runnable interface but cannot throw checked exceptions, and cannot return results. (used for threads), `call()`
- **functional interface** - contains only one abstract method, can have multiple default or static methods. You can use the annotation `@FunctionalInterface`
	- **default methods**: declared using the `default` keyword. used for providing an implementation for a method. Before Java 8 you could only use **abstract** methods and provide the implementation in a separate class.
	- what is a static method in an interface?
- **inner class**: a class defined in another class.
- an **abstract method** cannot have a body.
- **casting**
- break and continue with labels
- **thread safety** ( all the classes from `java.time` are immutable therefore thread-safe. multiple classes can access the same immutable object)
- **interfaces** are always abstract. they can have **static** and **default** methods. cannot be final.
- `Locale` requires the `language` parameter. Used for formatting data
- `new List<>()` is invalid because `List` is an interface, it cannot be instantiated.
- when declaring an array, the size of the array is never specified in the **left hand side** 
- **services**
- in a module definition that uses a service: the `uses` and `requires` clauses are used.
- `String`, `StringBuffer` and `StringBuilder` are `final` classes.
- wrapper classes are also `final` (`Integer`, `Short`, `Float`, `Long`, etc..)
- a `final` class cannot be extended. you can't inherit a final class.
- if a class is `final` all its methods are implicitly `final` 
- `Stream`, `Function`?
- Java module system
	- `--module` or `-m`
	- `--module-path` or `-p`
	- `--module-source-path`, has not shortcut
	- `--list-modules`, has no shortcut
	- `--show-module-resolution`
	- `--describe-module`
- **steam pipeline**:
	- a **source** (data)
	- an **intermediate operation** (like filter)
	- **terminal operation**
- you use `implements` when you want to obtain access to an interface's methods, it kinda works like `extends`
- **deadlock**, **thrashing**, **starvation**
	- **starvation**: happens when low-priority threads don't have access to shared resources because of the high-priority ("greedy") threads.
	- **deadlock**: when two threads are blocked forever waiting on each other.
	- **livelock**
- **anonymous inner classes**: classes without names that are declared and instantiated at the same time?
	- a non static inner class may have static members (feature added in Java 16)
	- **anonymous inner classes** cannot have any `extends` or `implements` clause.
	- **anonymous inner classes** cannot be static.

```java

class Greeting {
    void sayHello() {
        System.out.println("Hello from Greeting class!");
    }
}

public class Main {
    public static void main(String[] args) {
        Greeting g = new Greeting() {
            @Override
            void sayHello() {
                System.out.println("Hello from Anonymous Inner Class!");
            }
        };
        g.sayHello();
    }
}

```

- an abstract class cannot be instantiated. it acts as a blueprint for other classes. Used for sharing code among classes. an interface is a **contract** that tells the class what to do.
- **default values** for primitive types: all numeric types including char get the value of 0 (or 0.0 for *float* and *double*), a blank space is printed for *char*'s default value tho.
- autoboxing is applicable only to **primitive types**. 
- **migrating an application to modules**

---

- `StringBuilder.setLength(10)` will append spaces if the string's length is less than 10.
- the default case in a `switch` block should be the last clause because otherwise it dominates all other clauses.
	- `default` does not cover `null`, to cover it, a `case null` must be provided
	- for a switch with pattern matching you can use `case null, default -> "invalid input"`
- `Arrays.compare(int[] arr1, int[] arr2)` returns -1 if the first array is smaller lexicographically compared to the second one, 0 if the arrays provided are the same, and 1 if the first array is greater lexicographically compared to the second one.
- `Arrays.mismatch(int[] arr1, int[] arr2)` returns the index of the first mismatch between the two arrays, otherwise -1 if no mismatch is found.
- when you use a resource with `try` `catch`, the resource must implement `java.lang.AutoCloseable` interface
- `forEach` expects a `Consumer` as an argument. Not a function

```java
char[] myCharArr = { 'g', 'o', 'o', 'd'};  
String newStr = null;  
for (char ch : myCharArr) {  
    newStr += ch;  
}
System.out.println(newStr); // "nullgood" will get printed
```

- **static** methods can never be **abstract** (neither in an interface nor in a class)
- an **interface** can have a static method but the method must have a body

```java
Statement stmt = c.createStatement();
try(stmt){ //VALID
...
} 
```

```java
try(Statement stmt = c.createStatement()){ //VALID
...
} 
```

```java
Statement stmt = null;
try(stmt = c.createStatement()){ //INVALID, reassignment?
...
} 
```

![[arrays_pass_by_value.png]]
- `StringBuffer` and `StringBuilder` do not extend `String`.
- `ArrayList` extends `java.util.AbstractList`
- `ArrayList` implements `RandomAccess`
- the result **division** or **multiplication** between two integers will be itself an **integer**.
- **widening conversion** (small -> wide), done implictly by the compiler
- **narrowing conversion** (wide -> small), needs explicit **casting**.
- **lower-bounded wildcard**, e.g. *? super E*, `public void sort(Comparator<? super E> c)`
- `&&` and `||` are considered short-circuiting operators, they do not evaluate all the operands.
- all mathematical operators like `%` evualuate all the operands.
- **fields** in an interface are implicitly `public`, `static`, and `final`!!!!
- `List<? extends String> list2 = new ArrayList<>()` 
	- "`list2` is a list of some unknown type that _extends_ `String` (or is `String` itself)."
- `protected` is less restrictive than `package`
- the default accessibility is more restrictive than `protected` and less restrictive than `private`
- a `sealed` class cannot be final, a `sealed` class has always one or more subclasses
- only classes and interfaces can be marked as `sealed`.
- records are implictly `final`!! they cannot have subclasses.
- classes names are not considered keywords. `String String = "name"` is valid.
- substring(beginIndex - inclusive, endIndex - exclusive)
- `System.out` is of type `java.io.PrintStream`
- `Serializable` is a "marker" interface and does **NOT** declare any methods.
- `final` methods cannot be overriden.
- `private` methods are not inherited so they cannot be overriden.
- since Java 16 you can have **static** fields in an inner class (aka, nested class)
- of all the collection classes in `java.util` only `Vector` and `Hashtable` are thread-safe.

```java
Set s = Collections.synchronizedSet(new HashSet());
```

- `ConcurentMap/ConcurrentHashMap`
- the `=` operator has the least **precedence**
 
- inhanced for loop example: `for (var el: arr) {}`
- constructor of a super class should be called as the FIRST STATEMENT in the subclass's constructor
- max is a *reduction* method in the Stream interface (combines multiple values to produce one value)
- a module name cannot start with a number.
```java
com.amazing.movierentals //valid
com.amazing.$movierentals //valid
com.amazing.1movierentals //invalid
no1movierentals //valid
```

- to get the number of elements in an int array, use `length`
```java
int[] intArr = {2, 3, 8};
System.out.println(intArr.length);
```

- an overriding method is allowed to make the overridden method more accessible.
- `ToDoubleFunction` returns a double (primitive type). There several versions of `ToXXXFunction`. They are primitive returning verions of Function. For example, `ToIntFunction` returns an `int`.
- Interfaces cannot have protected methods.
- Interfaces can have static methods, public as well as private.
- non-abstract classes cannot have abstract methods.
- abstract methods cannot be private (it makes sense)
- the compiler will provide the default constructor only if ANY consructor is NOT declared explicitly.
- a record can have at most one vararg argument
- `~` is the bitwise negation operator that flips bits of a value (it changes a 0 to a 1 and a 1 to a 0)
- **integral types** means byte, short, int, long and char.
- an overriding method is allowed to change the return type of the overriden method ([Covariant return type - Wikipedia](https://en.wikipedia.org/wiki/Covariant_return_type))
---