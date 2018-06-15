# Nice Java Snippets

## Optional

### Block 1

```java
Response response;
IndexPage paginatedPage = getFromSomeWhere();

// Before
if (paginatedPage.getIndexPage != null) {
    response.setPageMarker(SerializationUtil.toJson(paginatedPage.getIndexPage));
}

// After
Optional.ofNullable( paginatedPage.getIndexPage )
        .ifPresent( pm -> response.setPageMarker(SerializationUtil.toJson(pm)) );
```
## Stream

### Block 1

Before
* `Iterator.remove` is the only safe way to modify a collection during iteration. See [here](https://docs.oracle.com/javase/tutorial/collections/interfaces/collection.html).
```java
public List<Apple> filterOutBadApple(@NonNull List<Apple> apples){
    Iterator<Apple> i = apples.iterator();
    while (i.hasNext()) {
        if (i.next().isBad()) {
            i.remove();
        }
    }
}
```

After
* Read more about [Collectors](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Collectors.html)
```java
public List<Apple> filterOutBadApple(@NonNull List<Apple> apples){
    return apples.stream()
                 .filter(a -> !a.isBad())
                 .collect(Collectors.toList());
}
```
