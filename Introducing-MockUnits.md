# Introducing MockUnits

Each main `MockNeat` object is composed by `MockUnit<T>`, `MockUnitInt`, `MockUnitLong`, `MockUnitDouble`, `MockUnitString`, `MockUnitDays`, `MockUnitMonths`, `MockUnitLocalDate` objects that are used to generate sets of data in a very generic and flexible way.

The `MockUnit<T>` is the generic interface from which all the MockUnits inherit their methods. 

The rest of the units are particular implementations that are enhancing the general interface with methods that are specific to their enclosed types (`Integer`, `String`, etc.)

## [MockUnit<T>](#mockunit-t)

This is the generic interface that contains useful methods to manipulate data. It allows us to generate not only  individual values but also collections (list, sets), maps, streams and arrays. 

To better understand the power the MockUnits let's follow a simple example. Let's say we want to generate `Boolean` values with a 35.25% probability of obtaining `true`.

For this we will create first a `MockUnit<Boolean>` object by calling the `bools()`:

```java
// Re-using a pre-existent ThreadLocl MockNeat Object
MockNeat mock = MockNeat.threadLocal();

// Creating our first MockUnit<Boolean>
MockUnit<Boolean> boolUnit = mock.bools();
```

Plain and simple. But doing only this the probability of generating a `true` is 50.00%. 

We will want to go further and put a constraint on our `MockUnit<Boolean>` object. 

We will add a new condition on our object by calling the `probability(double)` method. This methods "recursively" returns a new instance of `MockUnit<Boolean>` but internally the boolean generating method will behave accordingly to our constraint.

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
```

At this point we have created the "generation unit" of Booleans and all we need to do is make use of it.


### The `val()` method

If we want to obtain a Boolean we will call the val() method on the `MockUnit` instance:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
Boolean val = boolUnit.val();
```

We can write all of the above simpler without keeping the reference:

```java
Boolean val = mock.bools().probability(35.50).val();
```

Think of the `val()` method as the final step we doing before obtaining our values. This represents the "EXIT POINT" of a MockUnit chain. 

### The `list` method

Let's say we want to re-use the same generator but this generate `List<Boolean>` values, while keeping the same constraints. The List implementation we want to use is `LinkedList`, and it's size is `100`.

All we need to is to make use of the `list` method of the `RandUnit<T>` interface:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
List<Boolean> list = boolUnit.list(LinkedList.class, 100).val();
``` 

The `list` method will return a new `RandUnit` but this type the returning type will be `RandUnit<List<Boolean>>`. It might look a little bit confusing but we can re-write the above code like this:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
MockUnit<List<Boolean>> listBoolUnit = boolUnit.list(LinkedList.class, 100);
List<Boolean> list = listBoolUnit.val();
```

We stretch our minds and go even further to generate a `List<List<Boolean>>` by creating the associated `MockUnit<List<List<Boolean>>>`. 

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
MockUnit<List<Boolean>> listBoolUnit = boolUnit.list(LinkedList.class, 100);
MockUnit<List<List<Boolean>>> listListBoolUnit = listBoolUnit.list(LinkedList.class, 100);
List<List<Boolean>> list = listListBoolUnit.val();
```

And we can go like this forever... well not exactly because after a few thousand `List<List<List....<Boolean>..>`  we will eventually receive a `StackOverFlow` exception.

For making the code look more clean, there's no reason why we should keep references of `MockUnit`s all the time.  
We can simply start chaining our methods starting with `MockNeat` instance:

```java
List<Boolean> list = mock.bools().probability(35.50).list(100).val();
List<List<Boolean>> listList = mock.bools().probability(35.50).list(100).list(100).val();
List<List<List<Boolean>>> listListList = mock.bools().probability(35.50).list(100).list(100).list(100).val();
// Show can go on (if there's a good reason...)
```

_Note: If we don't specify the implementing List type, by default we will return an `ArrayList.class`_

### The `set()` method

This method works like the `list` one, with one exception: given the nature of `Set` we cannot guarantee a fixed size. We will generate `N` elements and add them into the `Set`, but if the elements are not unique, the `Set` will only keep what's possible. Duplicates won't be permitted.

Let's do a small exercise for fun. 

> We will generate 3 Booleans with a probability of returning `true` being 5%, and afterwards we put all the values a HashSet. 

> Let's see how many iterations we are going to have before we obtaining a Set with both `true` and `false` in it.

```java
Set<Boolean> set;
for(int i = 0 ;; i++) {
    set = mock.bools().probability(5.0).set(3).val();
    if (set.size()==2) {
        System.out.println("Number of iterations: " + i);
        break;
    }
}
```

After running the snippet I've obtained: `9`.

### The `collection()` method

The `collection` method works similar with the `list` method and the `set` method:

```java
Collection<Boolean> vector = mock.bools()
                                 .probability(35.50)
                                 .collection(Vector.class, 100)
                                 .val();
```

### The `mapKeys()` methods

This allows us to build `Map<T,R>`s, mapping our MockUnit generated values with a Set of keys.

The method allow us to:
- specify the number of elements in the map;
- specify the map implementation;

The keys can be obtained from:
- Another `MockUnit`;
- From a `Supplier<K>`;
- From an `Iterable<K>`;
- From an generic array `K[]`;
- From primitives arrays: `int[]`, `double[]`, `long[]`;

**Examples**:

**Example 1.** Obtaining a `Map<Integer, Boolean>` from an array of `int[]`:

```java
int[] keys = { 100, 200, 300, 400, 500, 600 };
Map<Integer, Boolean> map = mock.bools().mapKeys(keys).val();
```

Possible output:
```
{400=true, 100=false, 500=false, 200=true, 600=true, 300=true}
```

**Example 2.** Obtaining a `Map<Double, Boolean>` from an `Iterable<Double>`:

This time we want the map implementation to be a `LinkedHashMap`. 

```java
Iterable<Double> iterable = unmodifiableList(asList(0.1, 0.2, 0.3, 0.4, 0.5));
Map<Double, Boolean> map = mock.bools().mapKeys(LinkedHashMap.class, iterable).val();
```

Possible output:
```
{0.1=true, 0.2=true, 0.3=false, 0.4=false, 0.5=true}
```

### The `mapVals()` method

This allows us to build `Map<T,R>`s, mapping our MockUnit generated as keys for a given set of values.

The method allow us to:
- specify the number of elements in the map;
- specify the map implementation;

The values can be obtained from:
- Another `MockUnit`;
- From a `Supplier<K>`;
- From an `Iterable<K>`;
- From an generic array `K[]`;
- From primitives arrays: `int[]`, `double[]`, `long[]`;

**Example 1** Obtaining a `Map<Boolean, Integer>` from an array of `int[]`:

```java
int[] values = { 100, 200, 300, 400, 500, 600 };
Map<Boolean, Integer> map = mock.bools().mapKeys(keys).val();
``` 

Possible output:
```
{false=500, true=600}
```

PS: It's normal to have only two keys `false`/`true`.

**Example 2** Obtaining a `Map<Boolean, Long>` from another `Supplier<Long>`

```java
Supplier<Long> longSupplier = () -> System.currentTimeMillis();
Map<Boolean, Long> map = mock.bools().mapVals(10, longSupplier).val();
```

Possible output:
```
{false=1487951761873, true=1487951761873}
```

### The `stream()` method

Instead of obtaining a `Collection`, `List`, `Set` or a `Map` we can use the `MockUnit<Boolean>` to generate a `Stream<Boolean>` values by simply calling the `stream()` method.

```java
Stream<Boolean> stream = mock.bools().stream().val();
```

### The `map()` method

This method allows us to do pre-processing on the values before they are actually generated.

The input parameter is `Function<T, R>`, thus allowing us to actually change the returned type and to actually translate our "mocking" chain into a `MockUnit<R>`.

For example, if we want to transform our `MockUnit<Boolean>` into a `MockUnit<String>` that holds the `String` values `"true"` and `"false"`, we can do like this:

```java
List<String> list = mock.bools().map((b) -> b.toString()).list(10).val();
```

There is also a shortcut function that is recommended to be used in this case that help us "translate" our MockUnit<Boolean> into a MockUnitString. 

```java
List<String> list = mock.bools().mapToString().list(10).val();
```

Let's take another example. We will want to make our `MockUnit<Boolean>` generator translates into a `MockUnitInt` that returns 0s and 1s.

```java
MockUnitInt mockInt = mock.bools().mapToInt(b -> (b) ? 1 : 0);
List<Integer> listInt = mockInt.list(10).val();
```

A possible output:
```
[1, 0, 0, 1, 0, 0, 0, 0, 0, 0]
```

