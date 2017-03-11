# MockUnitLong

The `MockUnitLong` interface extends `MockUnit<Long>`. 

```java
public interface `MockUnitLong` extends MockUnit<Long>
```

This means that it "inherits" all the methods from [`MockUnit<Long>`](MockUnit)

The easiest way to obtain a `MockUnitInt` is to call the [`longs()`](MockNeat#longs) method from `MockNeat` or to call the [`mapToLong()`](MockUnit#maptolong) method.

Methods that are particular to `MockUnitLong`:

| Method | Description |
|:-------|:------------|
| [`arrayPrimitive()`](#arrayprimitive) | Generates a `MockUnit<long[]>` from a `MockUnitLong`. |
| [`array()`](#array) | Generates a `MockUnit<Long[]>` from a `MockUnitLong`. |
| [`longStream()`](#longstream) | Generates a `MockUnit<LongStream>` from a `MockUnitLong`. |

## `array()`

The method is used to generate a `MockUnit<Long[]>` from a `MockUnitLong`.

Compared to the [`array()`](MockUnit#array) method from `MockUnit<T>` there's no reason to specify the type of the array. We know it's `Long[]`.

Example for creating an array of 100 random Longs, with values between [1000, 2000): 

```java
Long[] array = mock.longs()
                   .range(1000l, 2000l)
                   .array(100)
                   .val();
````

### `arrayPrimitive()`

This method is used to generate a `MockUnit<long[]>` from a `MockUnitLong`.

Example for creating a primitive array of 100 random longs, with values between [1000l, 2000l):

```java
long[] array = mock.longs()
                   .range(1000, 200)
                   .arrayPrimitive(100)
                   .val();
```

### `longStream()` 

Can be used to obtain a more specific `LongStream` instead of a `Stream<Long>`, which normally can be obtain with the [`stream()`](MockUnit#stream) from `MockUnit<Long>`.