Table of Contents
=================

   * [Basics](#basics)
      * [General](#general)
      * [Data Structure](#data-structure)
      * [Polymorphism](#polymorphism)
         * [Abstract classes](#abstract-classes)
         * [Interface](#interface)
         * [Know when to use them](#know-when-to-use-them)
      * [Constructors and garbage collection](#constructors-and-garbage-collection)
      * [Numbers and statics](#numbers-and-statics)
         * [Static and Final](#static-and-final)
         * [Autoboxing](#autoboxing)
         * [Format Specifiler](#format-specifiler)
         * [Date](#date)
      * [Exceptions](#exceptions)
      * [File I/O](#file-io)
         * [Deserialization](#deserialization)
         * [Streams](#streams)
         * [java.io.File Class](#javaiofile-class)
            * [Writing a file](#writing-a-file)
            * [Buffers](#buffers)
            * [Reading a file](#reading-a-file)
      * [Threads](#threads)
         * [Concurrency](#concurrency)
      * [Generics](#generics)
         * [Collections](#collections)
   * [Ten topics](#ten-topics)

# Basics

## General

Java program has a `main` method to start the program.

>  This may seems no brainer, but when the project is huge, you may find it hard to find the `main` method as this may lie in another dependency package. Finding the `main` method is important if you want to know the details about how a service starts up.



Arguments vs. Parameters

* Arguments are what you passed to a method's parameters



Java: always pass by value (i.e. pass by copy)



instance & local variables

* just instance variable: inside a class
* local variable: also inside a method



`==` vs. `.equals()`



`&&` and `||` are called short circuit operation, while for `&` and `|`, they are not and they are forced to check both sides when used as boolean check.



Test-driven

* Start by writing a little unit test => write implementation that you need to pass the test => write a little more test code => … 
* At each iteration, you run all previous-written tests.



>  Learn to find answers by reading java doc/api. At least come up with your own solution before asking Google.



## Data Structure

Array vs. ArrayList

* Array
  * Need fixed size
  * Has special syntax ( i.e. `[]`)
* ArrayList
  * Has methods you can call
  * Update size dynamically
  * Primitives cannot be put into ArrayList 
    * Why? — think about how you implement ArrayList. What the parameter type would be?
  * Use parameterized types (e.g. `ArrayList<String>`)



## Polymorphism

Inheritance

* Access level controls who can see what.
* Private members are *not* inherited.

Polymorphism

* Reference type can be a super class.
* You can make polymorphic arrays, arguments, and return types.


In what condition will a class not be able to be subclassed?

1. There is no `private` class, but you can make it non-public (if you don't declare `public`). Only the class in the same package can subclass this non-public class.
2. `final` stops a class from being subclassed.
3. If a class has only `private` constructors, it can't be subclassed.

Override vs. Overload

* An overloaded method is just a different method that happens to have the same method name.

### Abstract classes

Abstract class and abstract method

* *Abstract class*: Some classes are needed for inheritance and polymorphism, but are not meant to be initialized (like Animal class). They are the classes that must be extended.
* An *abstract method* means the method must be overriden. It has no body.
* An abstract method cannot be in non-abstract class.
* You *must* implement all abstract methods in a concret class. (Remember, the abstract methods have no body!)


```java
// Example 1
private Animal[] animals = new Animals[5]; // Making an *array* object that holds Animal

// Example 2
ArrayList<Object> myDogArrayList = new ArrayList<Object>();
Dog aDog = new Dog();
myDogArrayList.add(aDog);

Dog d = myDogArrayList.get(0); // This won't compile! It comes out as what you declared.

// but you can cast it back
if (d instanceof Dog) {
    Dog dog = (Dog) d;
}
```



> *Start from a question. And you'll appreciate the design.*
>
> Question: You have an Animal inheritence tree. What if you want some subclasses (like dogs, cats) to have "pet" behaviors?

Why multiple inheritance is not a good thing?

```
      +---------------+
      |DigitalRecorder|
      +---------------+
      | int i         |
      | burn()        |
      +-+----------+--+          CDBurner and DVDBurner both
        |          |             inherit from DigitalRecorder.
        |          |             They both override the method
+-------v-+      +-v--------+    and inherit the i instance.
|CDBurner |      |DVDBurner |
+---------+      +----------+    Which one should ComboDrive use?
| burn()  |      | burn()   |
+----+----+      +-----+----+
     |                 |
     +--------+--------+
              |
        +-----v-----+
        |ComboDrive |
        +-----------+
        |           |
        +-----------+
```

* Deadly Diamond of Death.
  * You need extra rule to specify these special case.

### Interface

* Interface made all the methods abstract (avoided DDD confusion problem).
* Interface methods are all `public` and `abstract` by default (you can type them optionally)
* A class can implement multiple interfaces.
* Interface is not tied to inheritance tree.

### Know when to use them

* Make a class that doesn't extend anything when your new class doesn't pass the IS-A test for any other type.
* Make a subclass only when you need to make a *more specific* version of a class and need to override or add new behaviors
* Use an abstract class when you want to define a *template* for a group of subclasses, and you have at least *some* implementation code that all subclasses could use. Make the class abstract when you want to guarantee that nobody can make objects of that type.
* Use an interface when you want to define a *role* that other classes can play, regardless of where those classes are in the inheritance tree.

## Constructors and garbage collection

Stack and Heap

* Objects live in the heap.
* Method invocations and local variables live in the stack.

Constructors

* Constructors are not inherited.
* Constructors have no return type.
* If you write your own constructor, write the no-arg one if possible (but you don't have to).
* Instance variables are assigned a default value.


* All the constructos in an object's *inheritance tree* must run when you make a new object. 
  * Each subclass constructor immediately invokes its own superclass constructor. The subclass may depend on things from the super class. (You can't have a child before the parents exist.)
  * You can manually call using `super();`. And it must be the first statement in each constructor.
* What does `this()` mean? Can you have both `this()` and `super()` in the same constructor?

Object lifespan

* *life* and *scope*. Difference — think about a call stack `doStuff()` calls `go()`. How do you describe the local variable in `doStuff()`.


## Numbers and statics

### Static and Final

**Static method**

* Static method can't use non-static variables or methods.
* It's not recommended to call static method using a reference variable

**Static variable**

* Shared as a single copy among all instances of the object.
* All static variables in a class are initialized before any object of that class can be created.
* Static final variables are contants — they should be all caps.

Static initializer

* A block of code that runs when a class is loaded, before any other code can use the class.
  * It's a great place to initialize a static final variable.

```java
class Foo {
  final static int x;
  static {
      x = 42;
  }
}
```

**Final**

* Final *variable* — you can't *change* its value.
* Final *method* — you can't *override* the method.
* Final *class* — you can't *extend* the class.


### Autoboxing

* Autoboxing works almost everywhere, even `i++`  — `i` can be either `int` or `Integer`

```java
// What will happen for this code
public class TestBox {
    
    Integer i;
    int j;
    
    public static void main(String args[]) {
        TestBox t = new TestBox();
        t.go();
    }
    
    public void go() {
        j = i;
        System.out.println(j);
        System.out.println(i);
    }
}
```

* Utility methods

```java
Integer.parseInt("2");
Double.parseDouble("2.3");
new Boolean("true").booleanValue();
```

### Format Specifiler

```
%[argument number][flags][width][.precision]type

format("%,6.1f", 42.000);

* %d - decimal
* %f - floating point
* %x - hexadecimal
* %c - charater (eg. format("%c", 42) => *)

* %tc/%tr... for printing date
```

> Why `String.format()` can accept unspecified number of arguments? 
>
> => vargas (variable argument list). It's said that you won't need this in a well designed system.

### Date

* For a timestamp of now, use `Date`
* Everything else use `Calender` (new way)


## Exceptions

Hierarchy

* Exception
  * IOException
  * Interruputed Exception
  * RuntimeException
    * NullPointerExceptino
    * ClassCastException
    * ...

**Checked Exceptions** - *Compiler checked Exception*. Exceptions that are not subclass of RuntimeException are checked for by the compiler.

> Most RuntimeExceptions come from a problem in your code logic, rather than a condition that fails at runtime in ways that you cannot predict or prevent.

**Finally block**

* finally will still run even if the try or catch block has a return statement.

## File I/O

### Serialization

* Serialization saves the entire object graph.
  * All objects in the object graph must be serializable (or `NotSerializableException`).

```java
FileOutputStream fileStream = new FIleOutputStream("MyGame.ser");

ObjectOutputStream os = new ObjectOutputStream(fileStream);

os.writeObject(obj1);
os.writeObject(obj2);

os.close(); // FileOutputStream and the file will close automatically
```

* Implement `Serialization` if you want your class to be serializable
  * It's known as a *marker* or *tag* interface (it doesn't have any methods to implement)
  * The subclass is automatically serializable (even without declaring) if any superclass is serializable. — This is how interfaces always work.
* Mark an instance variable as `transient` if it can't (or shouldn't be saved)
  * It will be `null` after deserialization and defaults for primitives.
* Static variables, of course, are not serialized. They will have whaterver static variable its class *currently* has.
* `serailVersionUID` is needed if your class might evolve. 

### Deserialization

```java
FileInputStream fileStream = new FileInputStream("MyGrame.ser");

ObjectInputStream os = new ObjectInputStream(fileStream);

Object one = os.readObject();
Object two = os.readObject();

GameCharacter elf = (GameCharacter) one;
GameCharacter troll = (GameCharacter) two;

os.close();
```

* What happens when you have a serializable subclass of non-serializable superclass?
  * The superclass constructor will run just as though a new object of that type were being created.
* What JVM will do: read object from stream => determine class type from the object => find and load object's class (JVM will throw an exception if it can't) => A new object is given space on the heap => constructor for non-serializable class will run => instance variables are given the value from the serialized state (and transient variables...).

### Streams

**Connection streams** represent a connection to a source or destination (file, socket, etc). 

**Chain streams** can't connect on their own and must be chained to a connection stream.

* Often it takes at least two streams hooked together to do something useful — because *connection* streams are usually too low-level. e.g. `FileInputStreams` just writes bytes to a file.
  * Why can't we make an easier way? Think good OO.
  * Each class does *one* thing well. And it gives us flexibility.

###Output a String

```java
FileWriter writer = new FileWriter("Foo.txt");
writer.write("hello foo!");
writer.close();
```

### `java.io.File` Class

#### Writing a file

```java
// Make a File object representing an existing file (it does not represent the data in the file)
File f = new File("MyCode.txt");

// Make a new directory
File dir = new File("Chapter7");
dir.mkdir();

// List the contents of a directory
if (dir.isDirectory()) {
  String[] dirContents = dir.list();
  for (int i = 0; i < dirContents.length; i++) {
    System.out.println(dirContents[i];
  }
}
                       
// Get the absolute path of a file or directory
System.out.println(dir.getAbsolutePath());

// Delete a file or dir
boolean isDeleted = f.delete();
```

#### Buffers

```java
BufferedWriter writer = new BufferedWriter(new FileWriter(aFile));
```

* Only when buffer is full will the FileWriter write to the file on disk. — saving a lot of overhead of I/O to disk.
* Calling `write.flush()` will let it send data immediately.

#### Reading a file

```java
File myFile = new File("MyText.txt");
FileReader fileReader = new FileReader(myFile);

BufferedReader reader = new BufferedReader(fileReader);

String line = null;

while ((line = reader.readLine()) != null) {
  System.out.println(line);
}
reader.close();
```

## Threads

> I'm skipping notes for networking part now.

```java
public class MyRunnable implements Runnable {
  public void run(){
    do();
  }
  public void do(){
    //...
  }
}

class ThreadTester {
  public static void main(String[] args) {
    Runnable threadJob = new MyRunnable();
    Thread myThread = new Thread(threadJob);
    
    myThread.start();
  }
}
```

* Every thread in Java has its own call stack.
* A thread needs a job to do stuff — a job is an instance of some class that implements `Runnable` interface.
* The `Runnable` interface has only one `run()` method. It goes to the bottom of the new call stack (called first).
* The call stack is created when you call `start()` method. The thread moves from NEW state to RUNNABLE state.
* The thread is RUNNING when JVM's thread scheduler has selected it to be the currently-running thread. (#threads = #processors)
* The thread also be BLOCKED when waiting or sleeping.
* How thread scheduling works is not guaranteed. You can *help* influence it by letting threads to sleep periodically.
* You can name a thread using `setName()` (helpful for debugging!)

### Concurrency

Mark your method `synchronized`.

* Every object has a single lock with a single key.
* If an object has synchronized methods, only one thread are allowed to access one of the object's synchronized methods.

`synchronized` doesn't come for free

1. It has a certain amount of overhead.
2. It can slow your program down because it restricts concurrency.
3. It can lead to deadlock — synchronize only the bare minimum that should be synchronized.

How about static method? — Each loaed class has lock.

## Generics

Why — Generics means more type-safety. It will catch the problem in compile-time instead of runtime.

Three Basics

```java
new ArrayList<Song>() // creating instances
  
List<Song> songList = new ArrayList<Song>() // declaring and assigning variables
  
void foo(List<Song> list) // declaring and invoking methods
x.foo(songList)
```

**Generic Classes**

```java
public class ArrayList<E> extends AbstractList<E> implememts List<E> ... {
  public boolean add(E o)
  ...
}
```

(It's convention to use `T` unless you're specifically writing a collection class.)

**Generic Methods**

```java
// T declared in the method declaration
public <T extends Animal> void takeThing(ArrayList<T> list)
        ^                                          ^
        
// same as
public void takeThing(ArrayList<? extends Animal> list)
```

* Note that it's NOT the same as

  ```java
  public void takeThing(ArrayList<Animal> list)
  ```

  And this is not the same as

  ```java
  public void takeThing(Animal[] list)
  ```

### Collections

`Sort` method declaration

```java
public static <T extends Comparable<? super T>> void sort(List<T> list)
```

* In generics, "extends"  means “extends” or "implements" — meaning "is-a"
  * This means that whatever class `T` that you want sort must implement Comparable, or you use another sort method and pass a Comparator.

**Equality**

* References are equal if they refer to the same object.
  * Calling `hashCode()` will be the same.
  * `==` compares the bits in variable. The bits will be the same if two references point to the same object.
* Objects are equal if the `hashCode()` and `equals()` are overriden and returns the same.
* The defualt `equals` is to do `==`.
* How *HashSet* checks for duplicate
  * First check its hashcode.
  * Then check `equals()` for the objects that have the same hashcode.
* `.equals()` => `hashCode()` same, but not the other way!! 
  * That is, same hashcode != equal. It's because the hashcode "bucket" may overlap.

> Skipping release part and rmi part for now.





# Ten topics

**#10 Bit Manipulation**

**#9 Immutability**

* String - JVM puts strings into a specific part of memory called "String Pool", and gabage collector won't go there.
* Wrapper - there is no setter method for a wrapper object.

**#8 Assertions**

```java
assert (height > 0);
```

* run with `-ea` will run the code with assertion, otherwise it won't run.

**#7 Block Scope**

**#6 Linked Invocations**

e.g. `sb.delete(3,6).insert(2 "hmmm").deleteCharAt(1);`

More common and useful example is to invoke an instance method of the main class without keeping a renferece to the instance of the class.

```java
class Foo{
  public static void main(String[] args) {
    new Foo().go();
  }
  void go() {
    // ...
  }
}
```

**#5 Anonymous and Static Nested Classes**

Static nested classes

```java
public class FooOuter {
  static class BarInner {
  	void sayIt() {
    	System.out.println("method of a static inner class.");
  	}
  }
}

class Test {
  public static void main (String[] args) {
    FooOuter.BarInner foo = new FooOuter.BarInner();
    // Just use the name of the class
  }
}
```

* Static nested class is just like a regular non-nested class, but since it's a member of the outer class, it can access any private members, but *only the static ones*.

Anoynmous Inner classes

* Non-static nested classes are often referred to as *inner class*. (Nested classes contain inner class and non-static nested classes.)

```java
button.addctionListner(new ActionListener() {
  public void actionPerformed(ActionEvent ev) {
    System.exist(0);
  }
});
```

**#4 Access Levels and Access Modifier**

* public
* protected - just like default (code in the same package has access), EXCEPT it also allows subclasses *outside* the package to inherit the protected thing.
* default - only code within the same *package* as the class with the default thing can access the default thing
* private - only code within the same *class* can access the private thing. Note that it means private to the class, not private to the object. One Dog can see another Dog object's private stuff, for example.

**#3 String and StringBuffer/StringBuilder Methods**

**#2 Multidimensional Arrays**

**#1 Enumerations**

Create a set of constant values to represent the only valid values for a variable.

```java
// Without Enum
public static final int JERRY = 1;
public static final int BOBBY = 2;
public static final int PHIL = 3;

...
if (seletedBandMember == JERRY) {
  ...
}
```

* The problem of this is that `seletedBandMember` can be any number so it's easily broken.

```java
// Go with Enum
public enum Members {JERRY, BOBBY, PHIL};
public Members selectedBandMember; // Note that the type is `Members`

...
if (selectedBandMember == Members.JERRY) {
  ...
}
```

* It's actually just a special class, which implicitly extends `java.lang.Enum`

More complicated usage

```java
public class HfjEnum {
  enum Names {
    JERRY("lead guitar") {public String sings() {
      					return "plaintively";}
    	}, /* with argument passed to the constructor and "constant-specific class bodies which will override the basic enum method"*/
     
    BOBBY("rhythm guitar") {public String sings() {
      					return "hoarsely"; }
    	},
   	PHIL("bass") ;
    
    private String instrument;
    
    Names(String instrument) {
      this.instrument = instrument;
    }
    public String getInstrument() {
      return this.instrument;
    }
    public String sings() {
      return "occasionally";
    } 
  }
  
  public static void main(String[] args) {
    for (Names n: Names.values()) { // every enum comes with built-in values() method
      System.out.print(n);
      System.out.print(", instrument: " + n.getInstrument());
      System.out.print(", sings: " + n.sings());
    }
  }
}
```









