# MockUnitDouble

The `MockUnitDouble` interface extends `MockUnit<Double>`. 

```java
public interface `MockUnitDouble` extends MockUnit<Double>
```

This means that it "inherits" all the methods from [`MockUnit<Double>`](MockUnit)

The easiest way to obtain a `MockUnitDouble` is to call the [`doubles()`](MockNeat#doubles) method from `MockNeat` or to call the [`mapToDouble()`](MockUnit#maptodouble) method.

Methods that are particular to `MockUnitDouble`:

| Method | Description |
|:-------|:------------|
| [`arrayPrimitive()`](#arrayprimitive) | Generates a `MockUnit<double[]>` from a `MockUnitDouble`. |
| [`array()`](#array) | Generates a `MockUnit<Double[]>` from a `MockUnitDouble`. |
| [`doubleStream()`](#doublestream) | Generates a `MockUnit<DoubleStream>` from a `MockUnitDouble`. |

## `array()`

The method is used to generate a `MockUnit<Double[]>` from a `MockUnitDouble`.

Compared to the [`array()`](MockUnit#array) method from `MockUnit<T>` there's no reason to specify the type of the array. We know it's `Double[]`.

Example for creating an array of 100 random Doubles, with values between [1000.0, 2000.0): 

```java
Double[] array = mock.doubles()
                      .range(1000.0, 2000.0)
                      .array(100)
                      .val();
````

### `arrayPrimitive()`

This method is used to generate a `MockUnit<double[]>` from a `MockUnitDouble`.

Example for creating a primitive array of 100 random doubles, with values between [1000.0, 2000.0):

```java
double[] array = mock.doubles()
                     .range(1000, 200)
                     .arrayPrimitive(100)
                     .val();
```

### `doubleStream()` 

Can be used to obtain a more specific `DoubleStream` instead of a `Stream<Double>`, which normally can be obtain with the [`stream()`](MockUnit#stream) from `MockUnit<Double>`.