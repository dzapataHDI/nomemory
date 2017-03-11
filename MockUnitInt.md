# [MockUnitInt](#mockunitint)

The `MockUnitInt` interface extends `MockUnit<Integer>`. 

```java
public interface `MockUnitInt` extends MockUnit<Integer>
```

This means that it "inherits" all the methods from `MockUnit<Integer>`.

The easiest way to obtain a `MockUnitInt` is to call the `ints()` method from `MockNeat` or to call the `mapToInt()` method documented above on an existing `MockUnit<T>`.

**Example:** 
```java
MockUnitInt mockUnitInt = mock.ints();
```

Additionally it has some methods of it's own.

## The `intStream()` method

Returns an `IntStream` instead of a `Stream<Integer>`.

## `arrayPrimitve()`

Can be used to create an primitive array of `int[]`.

For example, to create an array of random integers, with values between [1000, 2000), a with a length of 100:

```java
int[] array = mock.ints()
                  .range(1000, 200)
                  .arrayPrimitive(100)
                  .val();
```

## `array()`

Can be used to create an array of `Integer[]`.

To create an array of random Integers, with values between [1000, 2000), with a length of 100: 

```java
Integer[] array = mock.ints()
                      .range(1000, 2000)
                      .array(100)
                      .val();
````