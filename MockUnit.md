## Generic `MockUnit<T>`

This is the main interface. There are also type-specific interfaces that extends `MockUnit<T>` but contain more useful methods for processing their internal types: `MockUnitString`, `MockUnitInt`, `MockUnitDouble`, etc.

The interface contains useful methods to manipulate and obtain data:

| Method | Return | Description |
|:-------|:-------|:------------|
| [`array()`](#array) | `MockUnit<T[]>` | This method is used to return an array of arbitrary values of type `T`.|
| [`collection()`](#collection) | `MockUnit<Collection<T>>` | This method is used to return an arbitrary `Collection<T>`.|
| [`consume()`](#consume) | `void` | This method is used to consume the results of a MockUnit<T>. |
| [`list()`](#list) | `MockUnit<List<T>>` | This method is used to generate a `MockUnit<List<T>>`. |
| [`map()`](#map) | `MockUnit<R>` | This method is used to translate a `MockUnit<T>` into a `MockUnit<R>`.|
| [`mapKeys()`](#mapkeys) | `MockUnit<T, R>` | This method is used to generate a `MockUnit<Map<T, R>>` from a `MockUnit<T>`.|
| [`mapToDouble()`](#maptodouble) | `MockUnitDouble` | This method is used to translate a `MockUnit<T>` into a `MockUnitDouble`. |
| [`mapToInt()`](#maptoint) | `MockUnitInt` | This method is used to translate a `MockUnit<T>` into a `MockUnitInt`.|
| [`mapToLong()`](#maptolong) | `MockUnitLong` | This method is used to translate a `MockUnit<T>` into a `MockUnitLong`.|
| [`mapToString()`](#maptostring) | `MockUnitString` | This method is used to translate a `MockUnit<T>` into a MockUnitString. |
| [`mapVals()`](#mapvals) | `MockUnit<Map<R, T>>` | This method is used to generate a `MockUnit<Map<R, T>>` from a `MockUnit<T>`.|
| [`set()`](#set) | `MockUnit<Set<T>>` | This method is used to generate a `MockUnit<Set<T>>`.|
| [`stream()`](#stream) | `MockUnit<Stream<T>> | This method is sued to generate a `MockUnit<Stream<T>>`.|
| [`val()`](#val) | `T` | This method is used to obtain an individual value `T` from a `MockUnit<T>`.|
| [`valStr()`](#valstr) | `String` | This method is used to generate a String from the `MockUnit<T>`.| 


### `array()`

This method is used to generate a generic array `T[]` from a `MockUnit<T>`.

Example generating a `String[]` with size 100, where each `String` value is a first name:

```java
String[] names = m.names()
                    .first()
                    .array(String.class, 100)
                    .val();
```


### `collection()`

This method is used to generate a `Collection<T>` from a `MockUnit<T>`.

The collection needs to have a public NO_ARGS constructor.

Example for generating a `Vector<Boolean>` with size 100, where each `Boolean` value has a 35.5% probability of being true:

```java
Collection<Boolean> vector = mock.bools()
                                 .probability(35.50)
                                 .collection(Vector.class, 100)
                                 .val();
```

### `consume()`

This method is used to "consume" the values that are generated from a `MockUnit<T>`:

Example of printing to `stdout` a `List<String>` where each entry is an URL:
```java
m.urls().list(100).consume(System.out::println);
// Possible Output: 
// [http://www.frenziedchi.io, http://www.eyelinersbarry.gov, ..., http://www.sphygmicmillard.com]
```

### `list()`


### `val()`

This method is used to return a single `<T>` value from the `MockUnit<T>`.

Example for generating a single boolean value that has a 35.5% probability of being true:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
Boolean val = boolUnit.val();
```

We can write all of the above simpler without keeping the reference:

```java
Boolean val = mock.bools().probability(35.50).val();
```

### `valStr()`

This method works exactly like `val()` but instead of returning directly it's value, it first calls `toString` on the object. 

By default, if the generated object is `null`, it should return an empty String.

```java
String alwaysTrue = m.bools().probability(100.0).valStr();
// Possible Output: "true"
```

If the generated object is `null`, we can't specify an alternative `String` value in this case:

```java
String nullll = m.from(new String[]{ null, null, null})
                 .valStr("NULLLL");
// Output: "NULLLL"
````

## `mapKeys()`

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

## `mapVals()`

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

## `stream()`

Instead of obtaining a `Collection`, `List`, `Set` or a `Map` we can use the `MockUnit<Boolean>` to generate a `Stream<Boolean>`:

```java
Stream<Boolean> stream = mock.bools().stream().val();
```

The Stream is infinite. 

## `map()`

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