[TOC]

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

### Interface and abstract classes

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





