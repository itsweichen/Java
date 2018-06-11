# Java notes

## Upcasting and downcasting

By casting you are not actually changing the object itself, you are just labeling it differently. 

> For example, if you create a Cat and upcast it to Animal, then the object doesn't stop from being a Cat. It's still a Cat, but it's just treated as any other Animal and it's Cat properties are hidden until it's downcasted to a Cat again. 

## Compare Objects

- Think over to use `==` or `.equal()`

### Compare `Long`
- use `.equals()`
  - Use `x == null && y == null || x.equals(y)`
  - Long will cache the smaller value.

```java
public class MyClass {
    public static void main(String args[]) {
        Long x=10L;
        Long y=10L;
        
        Long a=10000000000L;
        Long b=10000000000L;

        System.out.println(x == y); // true
        System.out.println(x.equals(y)); // true
        
        System.out.println(a == b); // false
        System.out.println(a.equals(b)); // true
    }
}
```

## Others

- try/catch have different scope.
