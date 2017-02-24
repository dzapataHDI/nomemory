The following sections contains code examples on how we can "play" with the library in order to random obtain integer values. 

* To generate a random int value in the interval ``(Integer.MIN_VALUE, Integer.MAX_VALUE)`` (note: The values can be both positive and negative):

```java
Integer randInt = mock.ints().val();
```

* To generate a random int value in an bounded interval [0, bound):

```java
Integer boundInt = mock.ints().bound(100).val();
```

* To generate a random integer bounded by 100 and then multiply it by 13

```java
Function<Integer, Integer> multiplyBy13 = (x) -> 13 * x;
Integer multiplied = mock.ints().bound(100).map(multiplyBy13).val();
```
Or in short:
```java
Integer multiplied2 = mock.ints().bound(100).map(x -> x * 13).val();
```

* To generate a random int value in certain range [lowerBound, upperBound):

```java
// Generates a random integer in the range [100, 200)
Integer rangeInt = mock.ints().range(100, 200).val();
```

* To generate a random int value from a pre-existing array:

```java
int[] array = {100, 200, 300, 400, 500};
Integer fromArray = mock.ints().from(array).val();
```