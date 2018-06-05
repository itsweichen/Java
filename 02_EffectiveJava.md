[TOC]

## Creating and Destroying Objects

### Item 1: Consider static factory methods instead of constructors

An example would be:

```java
public static Boolean valueOf(booean b) {
  return b ? Boolean.TRUE : Boolean.FALSE;
}
```

> Need more understanding

### Item2: Consider a builder when faced with many (optional) constructor parameters

Java does not support optional parameters! Normally you would...

1. List many constructors — does not scale at all!
2. Have setters — may be in an inconsistent state part way through; also you cannot make the class immutable.

Let's meet the *Builder* pattern. My attempt of writing a class.

```java
package com.company;

public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public NutritionFacts(final int servingSize, final int servings, final int calories, final int fat, final int sodium, final int carbohydrate) {
        this.servingSize = servingSize;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }

    // NutritionFacts nf = new Builder(servingSize, servings).setFat().build();

    public class Builder {
        private int servingSize = 0;
        private int servings = 0;
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(final int servingSize, final int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public void setCalories(final int calories) {
            this.calories = calories;
        }

        public void setFat(final int fat) {
            this.fat = fat;
        }

        public void setSodium(final int sodium) {
            this.sodium = sodium;
        }

        public void setCarbohydrate(final int carbohydrate) {
            this.carbohydrate = carbohydrate;
        }

        public NutritionFacts build() {
            return new NutritionFacts(servingSize, servings, calories, fat, sodium, carbohydrate);
        }
    }

}
```

There are many problems with this implementation.

1. First of all, it would not run as what you expected. Always examine your code before submitting.
2. How to construct the object is actually not correct. A correct way is

```java
NutritionFacts coke = new NutritionFacts.Builder(servingSize, servings).setFat(fat).build();
// Notice that you need to invoke Builder class from inside the real class.
```

2. Builder class
   1. The Builder class should be `static`, so that you can invoke it using the real class name.
   2. You should assign `final` to the required fields and only initilize the optional fields with default values.
   3. The set methods inside the builder should return `Builder`, so that they can be invoked as a chain.
   4. You don't need to write every parameters when you return the object. Simply `return new NutritionFacts(this);` to pass the builder to the constructor.
3. Consctuctor should be `private` and the arguments should only have builder `private NutritionFacts(Builder builder){ servingSize = builder.servingSize; ... }`

### Item3: Enforce the singleton property with a private constructor or an enum type



