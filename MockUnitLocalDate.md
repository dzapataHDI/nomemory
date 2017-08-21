# MockUnitLocalDate

The `MockUnitLocalDate` interface extends `MockUnit<LocalDate>`. 

```java
public interface `MockUnitLocalDate` extends MockUnit<LocaleDate>
```

This means that it "inherits" all the methods from [`MockUnit<LocaleDate>`](MockUnit)

The easiest way to obtain a `MockUnitLocalDate` is to call the [`localDates()`](MockNeat#localdates) method from `MockNeat`.

Methods that are particular to `MockUnitInt`:

| Method | Description |
|:-------|:------------|
| [`toUtilDate()`](#arrayprimitive) | Generates a `MockUnit<java.util.Date>` from a `MockUnitLocalDate`. |

### `toUtilDate()`

Translates the existing `MockUnitLocalDate` into a `MockUnit<java.util.Date>`.

Example:
```java
Date date = mock.localDates()
                  .between(
                     of(2000, 10, 10),
                     of(2020, 10, 10)
                  )
                  .toUtilDate()
                  .val();
```