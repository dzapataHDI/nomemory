# MockUnitInt

The `MockUnitInt` interface extends `MockUnit<Integer>`. 

```java
public interface `MockUnitInt` extends MockUnit<Integer>
```

This means that it "inherits" all the methods from [`MockUnit<Integer>`](MockUnit)

The easiest way to obtain a `MockUnitInt` is to call the [`ints()`](MockNeat#ints) method from `MockNeat` or to call the [`mapToInt()`](MockUnit#maptoint) method.

Methods that are particular to `MockUnitInt`:

| Method | Description |
|:-------|:------------|
| [`arrayPrimitive()`](#arrayprimitive) | Generates a `MockUnit<int[]>` from a `MockUnitInt`. |
| [`array()`](#array) | Generates a `MockUnit<Integer[]>` from a `MockUnitInt`. |
| [`intStream()`](#intstream) | Generates a `MockUnit<IntStream>` from a `MockUnitInt`. |

## `array()`

The method is used to generate a `MockUnit<Integer[]>` from a `MockUnitInt`.

Compared to the [`array()`](MockUnit#array) method from `MockUnit<T>` there's no reason to specify the type of the array. We know it's `Integer[]`.

Example for creating an array of 100 random Integers, with values between [1000, 2000): 

```java
Integer[] array = mock.ints()
                      .range(1000, 2000)
                      .array(100)
                      .val();
````

### `arrayPrimitive()`

This method is used to generate a `MockUnit<int[]>` from a `MockUnitInt`.

Example for creating a primitive array of 100 random integers, with values between [1000, 2000):

```java
int[] array = mock.ints()
                  .range(1000, 200)
                  .arrayPrimitive(100)
                  .val();
```

### `intStream()` 

Can be used to obtain a more specific `IntStream` instead of a `Stream<Integer>`, which normally can be obtain with the [`stream()`](MockUnit#stream) from `MockUnit<Integer>`.