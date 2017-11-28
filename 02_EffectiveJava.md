[TOC]

## Creating and Destroying Objects

### Item 1: Consider static factory methods instead of constructors

An example would be:

```java
public static Boolean valueOf(booean b) {
  return b ? Boolean.TRUE : Boolean.FALSE;
}
```

> Not the same as the *Factory Method* pattern.

There are both advantages and disadvantages compared to constructor.

**Adv1. They have names.**

This is good because you know what the parameters are for. For a method that returns a BigInteger that is probably a prime, prefer `BigInteger.probablePrime` to `BigInteger(int, int, Random)`.

**Adv2. You are not required to create a new object each time.**

This way, you can return preconstructed instances (Item 15), or cache instance. You can also return one instance repeatedly — allow classes to maintain strict control; it's called *instance-controlled* class. 

**Adv3. They can return an object of any subtype of their return type.**

> ??? Later.

### Item2: Consider a builder when faced with many (optional) constructor parameters

Java does not support optional parameters! Normally you would...

1. List many constructors — does not scale at all!
2. Have setters — may be in an inconsistent state part way through; also you cannot make the class immutable.

Let's meet the *Builder* pattern.

```java
public class Car {
  private final String make;
  private final String model;
  private final int year;
  private String color;
  private String hasBlindSpotDetection;
  private String hasBackUpCamera;
  
  public static class Builder {
    private final String make;
    private final String model;
    private final int year;

    private String color = "black";
    private String hasBlindSpotDetection = false;
    private String hasBackUpCamera = false;

    public Builder(String make, String model, int year) {
      this.make = make;
      this.model = model;
      this.year = year;
    }
    
    public Builder setColor(String val) { color = val; return this; }
  	public Builder addBlindSpotDetection() { hasBlindSpotDetection = true; return this; }
  	public Builder addBackUpCamera() { hasBackUpCamera = true; return this; }
  
  	public Car build() {
      return new Car(this);
  	}
  }
  
  private Car(Builder builder) {
    make = builder.make;
    model = builder.model;
    year = builder.year;
    color = builder.color;
    hasBlindSpotDetection = builder.hasBlindSpotDetection;
    hasBackUpCamera = builder.hasBackUpCamera;
  }
}
```

How to use it:

```java
Car myFirstCar = new Car.Builder("Acura", "TLX", 2018).setColor("white").addBlindSpotDetection().addBackUpCamera().build();
```

### Item3: Enforce the singleton property with a private constructor or an enum type



