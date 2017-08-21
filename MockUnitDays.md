# MockUnitDays

The `MockUnitDays` interface extends `MockUnit<DayOfWeek>`. 

```java
public interface MockUnitDays extends MockUnit<DayOfWeek> 
```

This means that it "inherits" all the methods from [`MockUnit<DayOfWeek>`](MockUnit)

The easiest way to obtain a `MockUnitDays` is to call the [`days()`](MockNeat#ints) method from `MockNeat<T>`.

Methods that are particular to `MockUnitDays`:


| Method | Description |
|:-------|:------------|
| [`display()`](#display) | This generates a `MockUnitString` from `MockUnitDays` by transforming the days of the week into localised strings. |

### `display()` 

This method generates a `MockUnitString` from a `MockUnitDays` by transforming the days of the week into localised strings. 

Examples of generating a String that represents a random day of the week-end in French:

```java
String day = m.days()
              .after(FRIDAY)
              .display(TextStyle.FULL_STANDALONE, Locale.FRANCE)
              .val();
// Possible Output: "dimanche"
```

The method signatures are:

```java
default MockUnitString display(TextStyle textStyle, Locale locale)
```

When we want to use the default locale:
```java
default MockUnitString display(TextStyle textStyle)
```

When we want to use the default locale and the `textStyle` as `FULL_STANDALONE`:
```java
default MockUnitString display()
```