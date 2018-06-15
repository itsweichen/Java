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
