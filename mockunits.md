# Introducing MockUnits

`MockUnit<T>` is a `@FunctionalInterface` that has exactly one non-default abstract method:

```java
@FunctionalInterface
@SuppressWarnings("unchecked")
public interface MockUnit<T> {

    // Functional Method
    Supplier<T> supplier();

// ...

}
```

It's important to note that almost every method of `MockNeat` class returns a `MockUnit<T>` or an instance of its implementations. 

```java
MockNeat m = MockNeat.threadLocal();

// A mockUnit that can generate integers in [0, 100) interval
MockUnit<Integer> intMockUnit = m.ints().bound(100);

// Generating an integer with the mockUnit
int integer = intMockUnit.val();

// Generating another integer with the mockUnit
int integer2 = intMockUnit.val()
```

In the same time each `MockUnit<T>` is "smart" enough to generate "children" MockUnits that inherit the behaviour of their parent, but adding some particular behaviour or their own. 

With the `intMockUnit` from the previous example we can "generate" another `MockUnit` that has the ability to generate lists of integers (`List<Integer>`) in the same interval [0, 100):

```java
// A mockUnit that can generate list of integers
MockUnit<List<Integer>> listMockUnit = intMockUnit.list(100);

// Generating a list of integers
List<Integer> list = listMockUnit.val();

// Generating another list of integers
List<Integer> list2 = listMockUnit.val();
```

Without keeping the intermediary references we can write "incremental chains" of MockUnits that are passing their behaviour further and further. 

Usually (with some exceptions) a chain like this is "lazy", and no value is generated until calling a "closing" method, like `val()`:

```java
List<Integer[]> listArray = m.ints() 
                             .range(0, 100)
                             .array(100)
                             .list(ArrayList.class, 100)
                             .val();
```

Breaking the previous example:

| Step | Description |
|:-------- |:------- |
| `ints()` | Returns a "dumb" `MockUnit<Integer>` that has the ability to generate integers (with no constraint). |
|`range()` | Returns a newer & smarter `MockUnit`, that just like the previous works with Integers. The only difference is that this time the generated values are in the interval `[0, 100)` |
|`array()` | Returns a `MockUnit<Integer[]>` that will be able to generate arrays of integers in the interval `[0, 100)` |
|`list()` | Returns a `MockUnit<List<Integer[]>>` that will be able to generate lists of arrays of integers in the interval `[0, 100)` |
|`val()` | Closes the cycle and gets the actual value, which is a `List<Integer[]>` |
