[TOC]

## Strategy Pattern

We have some ducks.

```java
Duck
  quack()
  swim()
  display()
  
MallardDuck extends Duck
  display() // each duck subtype has its own display way

RedheadDuck extends Duck
  display()
```

Now we need the ducks to fly.

* Option 1: add fly() to the supertype `Duck`
  * Good: code reuse
  * Bad: some ducks don't fly (like RubberDuck)! Although you can override the method, it's very confusing the have this in the parent class.
* Option 2: make `Flyable` an interface, so that the ducks that can fly can implement that interface
  * Good: solve the problem above
  * Bad: code reuse — if you want to make a change, you need to make changes to every class (think about 48 flying ducks). *The one constant in software development is CHANGE!*

> **Design Principle: Identify the aspects of your application that vary and separate them from what stays the same.** — take the parts that vary and encapsulate them, so that later you can alter or extend the parts that vary without affecting those that don't.

So we will pull out those that change and make separate classes like `FlyBehaviors` and `QuackBehaviors`.

* We want to *assign* behaviors to the instance of `Duck`.
* Even better, we want to change the behavior dynamically (at runtime) — meaning we should have setters.

> **Design Principle: Program to an interface, not an implementation.**

```java
interface FlyBehavior
  fly()

class FlyWithWings implements FlyBehavior
  fly()
  
class FlyNoWay implements FlyBehavior
  fly()
```

In `Duck` classes, we have instance variables that declared as interface type, and have setter like `setFlyBehavir(FlyBehavior fb)` to set the behavior dynamically, and of course have `performFly()` in each class that does the job.

> **Design Principle: Favor composition over inheritance.** — composition meaning putting classes together instead of using inheritance. It gives more flexibility by encapsulating a family of methods into its own set of classes, but allows you to change behavior at runtime.

Other Notes

* You should think about what things might change we you design it, but these principles apply to any stage of the development cycle.

---

**The Strategy Pattern** defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

---



##Observer Pattern 

We have a weather station and want to display the data to three specific displays (current conditions display, statistics display, forecast display).

```java
WeatherData
  getTemperature()
  getHumidity()
  getPressure()
  measurementsChanged() // called any time new data is available.
```

An easy solution would be

```java
public class WeatherData {
  public void measurementsChanged(){
    float temp = getTemperanture();
    float humidity = getHumidity();
    float pressure = getPressure();
    
    currentConditionDisplay.update(temp, humidity, pressure);
    statisticsDisplay.update(temp, humidity, pressure);
    forecastDisplay.update(temp, humidity, pressure);
  }
}
```

Problems

* We might want to add more displays; this will make us change `measurementsChanged` method — We should encapsulated the part that changes.
  * And we have no way to add displays *at run time*.
* We might want to add more measures to the display; this means we'll need to change every invocation of the `update` method. — We should be coding to interfaces, not concrete implementations.
* The good part is that the displays seem to implement a common interface.

---

**The Observer Pattern** defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically.

```
   <<interface>>				<<interface>>
	  Subject					  Observer
 registerObserver()				  update()
 removeObserver()
 notifyObservers()
```

---

 Subject object is decoupled from the observer objects.

* The only thing the subject knows about observer is that it implements a certain interface.
* We can add new observers anytime.
* We never need to modify the subject to add new types of observers.
* Change to either subject or observers will not affect the other.

> **Design Principle: Stive for loosely coupled designs between objects that interact.**

==Question: is `update()` a good place to call `display()`==

Java's built-in Observer Pattern

* `java.util.Observer` and `java.util.Observable`

```
   Observable			      Observer
 addObserver()				  update()
 deleteObserver()
 notifyObservers()
 setChange()
```

* Need to first call `setChange()` and then call `notifyObservers()` or `notifyObservers(Object arg)`
  * `setChange()` allows you to optimize the notifications.
* Two modes with two notify methods
  * pull mode (call subject's method to fetch the data when notified)
  * push mode
* Never depend on the order of evaluation of the Observer notifications.
* Dark side of java util
  * Observerable is a class
    * You can't add on the Observable behavior if the class has already extends another superclass.
    * ​









