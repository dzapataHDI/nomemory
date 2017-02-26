# Introducing MockUnits

Each main `MockNeat` object is composed by different implementations of [[MockUnit<T>|mockunits#mockunit-t]], `MockUnitInt`, `MockUnitLong`, `MockUnitDouble`, `MockUnitString`, `MockUnitDays`, `MockUnitMonths`, `MockUnitLocalDate` interfaces.

Those implementations that are used to generate sets of data in a very generic and flexible way.

The `MockUnit<T>` is the generic interface from which all the other MockUnits inherit their methods. 

The rest of the interfaces are extending `MockUnit<T>` by enhancing it with methods that are very specific to their enclosed types (`Integer`, `String`, etc.)

## [MockUnit<T>](#mockunit-t)

This is the generic interface that contains useful methods to manipulate data. It allows us to generate not only  individual values but also collections (list, sets), maps, streams and arrays. 

To better understand the power the MockUnits let's follow a simple example in which we want to generate `Boolean` values with a 35.25% probability of obtaining `true`.

For this we will create first a `MockUnit<Boolean>` object by calling the `bools()` method on `MockNeat`:

```java
// Re-using a pre-existent ThreadLocl MockNeat Object
MockNeat mock = MockNeat.threadLocal();
// Creating our first MockUnit<Boolean>
MockUnit<Boolean> boolUnit = mock.bools();
```

We will want to go further and put a constraint on our `MockUnit<Boolean>` object. 

We will add a new condition on our object by calling the `probability(double)` method. 

This methods "recursively" returns a new instance of `MockUnit<Boolean>` but internally the boolean generating method will behave accordingly to our constraint.

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
```

At this point we have created the "generation unit" of Booleans and all we need to do is make use of it.


### [`val()`](#mockunit-t-val)

If we want to obtain a Boolean we will call the val() method on the `MockUnit` instance:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
Boolean val = boolUnit.val();
```

We can write all of the above simpler without keeping the reference:

```java
Boolean val = mock.bools().probability(35.50).val();
```

Think of the `val()` method as the final step we doing before obtaining our values. This represents the "EXIT POINT" of a `MockUnit` chain. 

### [`valStr()`](#mockunit-t-valStr)

This method works exactly like `val()` but instead of returning directly it's value, it first calls `toString` on the object. If the generated object is `null`, it should return an empty String.

### [`list()`](#mockunit-t-valStr)

Let's say we want to re-use the same MockUnit<Boolean> but this time we generate a `List<Boolean>` values, while keeping the same constraints. The List implementation is `LinkedList`, and it's size is `100`:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
List<Boolean> list = boolUnit.list(LinkedList.class, 100).val();
``` 

The `list` method will return a new `RandUnit` but this time the returning type will be `RandUnit<List<Boolean>>`. 
It might look a little bit confusing but we can re-write the above code like this:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
MockUnit<List<Boolean>> listBoolUnit = boolUnit.list(LinkedList.class, 100);
List<Boolean> list = listBoolUnit.val();
```

If we stretch our minds even further we can generate a `List<List<Boolean>>` by creating the associated `MockUnit<List<List<Boolean>>>`. 

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
MockUnit<List<Boolean>> listBoolUnit = boolUnit.list(LinkedList.class, 100);
MockUnit<List<List<Boolean>>> listListBoolUnit = listBoolUnit.list(LinkedList.class, 100);
List<List<Boolean>> list = listListBoolUnit.val();
```

And we can go like this forever... well not exactly because after a few thousand `List<List<List....<Boolean>..>`  we will eventually receive a `StackOverFlow` exception.

For making the code look more clean, there's no reason why we should keep references of `MockUnit`s all the time.  
We can simply start chaining our methods.

```java
List<Boolean> list = mock.bools().probability(35.50).list(100).val();
List<List<Boolean>> listList = mock.bools().probability(35.50).list(100).list(100).val();
List<List<List<Boolean>>> listListList = mock.bools().probability(35.50).list(100).list(100).list(100).val();
// Show can go on (if there's a good reason...)
```

_Note: If we don't specify the implementing List type, by default we will return an `ArrayList.class`_

### [`set()`](#mockunit-t-set)

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

After running the snippet the value obtained was `9`.

### [`collection()`](#mockunit-t-collection)

```java
Collection<Boolean> vector = mock.bools()
                                 .probability(35.50)
                                 .collection(Vector.class, 100)
                                 .val();
```

### [`mapKeys()`](#mockunit-t-mapKeys)

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

### [`mapVals()`](#mockunit-t-mapVals)

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

### [`stream()`](#mockunit-t-stream)

Instead of obtaining a `Collection`, `List`, `Set` or a `Map` we can use the `MockUnit<Boolean>` to generate a `Stream<Boolean>`:

```java
Stream<Boolean> stream = mock.bools().stream().val();
```

The Stream is infinite. 

### [`map()`](#mockunit-t-map)

This method allows us to do pre-processing on the values before they are actually generated.

The input parameter is a `Function<T, R>`. As you can see in the definition it is possible to change the return type of the next MockUnit in the chain.

For example, if we want to transform our `MockUnit<Boolean>` into a `MockUnit<String>` that holds the `String` values `"true"` and `"false"`, we can do like this:

```java
List<String> list = mock.bools().map((b) -> b.toString()).list(10).val();
```

`mapToString` a shortcut function that can to be used to "translate" the `MockUnit<Boolean>` into a `MockUnitString`, instead of `MockUnit<String>`.

```java
List<String> list = mock.bools().mapToString().list(10).val();
```

If we want to translate the `MockUnit<Boolean>` into a `MockUnitInt` that returns 0s and 1s, we can use the shortcut method: `mapToInt`. 

```java
MockUnitInt mockInt = mock.bools().mapToInt(b -> (b) ? 1 : 0);
List<Integer> listInt = mockInt.list(10).val();
```

Or the shorter version of it (without keeping the intermediary reference):
```java
List<Integer> listInt = mock.bools()
                            .mapToInt(b -> b ? 1 : 0)
                            .list(10)
                            .val();
```

A possible output:
```
[1, 0, 0, 1, 0, 0, 0, 0, 0, 0]
```

Similar shortcut methods exists for translating the MockUnit<Boolean> into a `MockUnitDouble`  or a `MockUnitLong`.

## [MockUnitInt](#mockunitint)

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

### The `intStream()` method

Returns an `IntStream` instead of a `Stream<Integer>`.

### The `arrayPrimitve()` method

Can be used to create an primitive array of `int[]`.

For example, to create an array of random integers, with values between [1000, 2000), a with a length of 100:

```java
int[] array = mock.ints()
                  .range(1000, 200)
                  .arrayPrimitive(100)
                  .val();
```

### The `array()` method

Can be used to create an array of `Integer[]`.

To create an array of random Integers, with values between [1000, 2000), with a length of 100: 

```java
Integer[] array = mock.ints()
                      .range(1000, 2000)
                      .array(100)
                      .val();
````