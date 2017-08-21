The `MockUnitMonth` interface extends `MockUnit<Month>`.

```java
public interface MockUnitMonth extends MockUnit<Month>
```

That means that it inherits all the methods from `MockUnit<Month>`.

The easiest way to obtain a MockUnitDays is to call the [`months()`](MockNeat#months) method from `MockNeat`.

Methods that are particular to MockUnitDays:


| Method | Description |
|:-------|:------------|
| [`display()`](#display) | This generates a `MockUnitString` from `MockUnitMonth` by transforming the days of the week into localised strings. |

### `display()`

Example:

```java
String month = mock.months()
                    .before(JULY)
                    .display(TextStyle.FULL, Locale.CANADA)
                    .val();

//Possible Ouput: May
```