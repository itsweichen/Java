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

FlyWithWings implements FlyBehavior
  fly()
  
FlyNoWay implements FlyBehavior
  fly()
```

In `Duck` classes, we have instance variables that declared as interface type, and have setter like `setFlyBehavir(FlyBehavior fb)` to set the behavior dynamically, and of course have `performFly()` in each class that does the job.

> **Design Principle: Favor composition over inheritance.** — composition meaning putting classes together instead of using inheritance. It gives more flexibility by encapsulating a family of methods into its own set of classes, but allows you to change behavior at runtime.

Other Notes

* You should think about what things might change we you design it, but these principles apply to any stage of the development cycle.

---

**The Strategy Pattern** defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

---

