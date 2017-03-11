## Generic `MockUnit<T>`

The methods of the interface are:

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
| [`set()`](#set) | `MockUnit<Set<T>>` | This method is used to generate a `MockUnit<Set<T>>` from a `MockUnit<T>`.|
| [`stream()`](#stream) | `MockUnit<Stream<T>>` | This method is sued to generate a `MockUnit<Stream<T>>`.|
| `supplier()` | `Supplier<T>` | This is the abstract, non-default method of the interface. Once it's implemented it's being called by the rest of the methods to generate data. |
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

This method is used to generate a `MockUnit<Collection<T>>` from a `MockUnit<T>`.

The collection needs to have a public NO_ARGS constructor.

Example for generating a `Vector<Boolean>` with size 100, where each `Boolean` value has a 35.5% probability of being true:

```java
Collection<Boolean> vector = mock.bools()
                                 .probability(35.50)
                                 .collection(Vector.class, 100)
                                 .val();
```

### `consume()`

This method is used to "consume" the values that are generated from a `MockUnit<T>`.

The input parameter of `consume()` is a `Consumer<T>`.

Example for printing to `stdout` a `List<String>` where each entry is an URL:

```java
m.urls().list(100).consume(System.out::println);
// Possible Output: 
// [http://www.frenziedchi.io, http://www.eyelinersbarry.gov, ..., http://www.sphygmicmillard.com]
```

### `list()`

This method is used to create a `MockUnit<List<T>>` from a previous `MockUnit<T>`.

By default the implementation used is `ArrayList<T>`.

Example for generating a `List<String>` where every `String` is a country name. The list has 100 elements:

```java
List<String> countries = m.countries().names().list(100).val();
// Possible Output: [Guam, Lao People's Democratic Republic, French Polynesia, Algeria, Somalia, Iraq, ...]
```

It is also possible to specify a different `List<T>` implementation. For example, in the previous example we can use a `LinkedList<String>` instead of the default `ArrayList<String>`:

```java
List<String> countries = m.countries().names().list(LinkedList.class, 100).val();
// Possible Output: [Guam, Lao People's Democratic Republic, French Polynesia, Algeria, Somalia, Iraq, ...]
```

*NOTE*: The `list()` method only accepts `List` implementations that have a public NO_ARGS constructor.

### `map()`

This method allows us to do pre-processing on the values before they are actually generated. 

It is also possible to help translate a `MockUnit<T>` to a `MockUnit<R>` (of a different type).

The input parameter for the method is a `Function<T, R>`. `T` can be the same `R`. 

Example for translating a `MockUnit<Boolean>` into a `MockUnit<String>` that holds the `String` values `"true"` and `"false"`:

```java
List<String> list = mock.bools().map((b) -> b.toString()).list(10).val();
```

### `mapKeys()`

The method help us generating a `MockUnit<Map<R, T>` from a `MockUnit<T>`. 

The keys can be obtained from:
- Another `MockUnit<R>`;
- From a `Supplier<R>`;
- From an `Iterable<R>`;
- From an generic array `R[]`;
- From primitives arrays: `int[]`, `double[]`, `long[]`;

Example for obtaining a `Map<Integer, Boolean>` from an array of `int[]`:

```java
int[] keys = { 100, 200, 300, 400, 500, 600 };
Map<Integer, Boolean> map = mock.bools().mapKeys(keys).val();
// Possible Output: {400=true, 100=false, 500=false, 200=true, 600=true, 300=true}
```

Example for obtaining a `Map<Double, Boolean>` (`LinkedHashMap`) from an `Iterable<Double>`:

```java
Iterable<Double> iterable = unmodifiableList(asList(0.1, 0.2, 0.3, 0.4, 0.5));
Map<Double, Boolean> map = mock.bools().mapKeys(LinkedHashMap.class, iterable).val();
// Possible Output: {0.1=true, 0.2=true, 0.3=false, 0.4=false, 0.5=true}
``` 

### `mapToDouble()` 

This method is used to transform a `MockUnit<T>` into a more specific `MockUnitDouble`.

The input parameter is a `Function<T, Double>`.

Example for translating a `MockUnit<Boolean>` into a `MockUnitDouble`. If the value it's true, it should return a value between [1.0, 2.0), else it should return a value between [2.0, 3.0):

```java
MockUnitDouble md = m.bools()
                     .mapToDouble((bool) -> {
                         if (bool)
                             return m.doubles().range(1.0, 2.0).val();
                         return m.doubles().range(2.0, 3.0).val();
                     });

Double d = md.val();
```

### `mapToInt()`

This method is used to translate a `MockUnit<T>` into a more specific `MockUnitInt`.

The input parameter for the method is a `Function<T, Integer>`.

Example for translating a `MockUnit<Boolean>` into a `MockUnitInt` by transforming `true` into `1` and `false` into `0`:

```java
MockUnitInt zeroOrOne = m.bools()
                         .mapToInt((b) -> (b) ? 1 : 0);

List<Integer> list = zeroOrOne.list(5).val();
// Possible Output: [0, 0, 0, 1, 0]
```

### `mapToLong()`

This method is used to translate a `MockUnit<T>` into a more specific `MockUnitLong`.

The input parameter of the method is a `Function<T, Long>`.

Example for translating a `MockUnit<List<Long>>` into a `MockUnitLong` by calculating the sum of the numbers in the `List<Long>`:

```java
MockUnit<List<Long>> muList = m.longs()
                                .range(0l, 100l)
                                .list(10);

Long sum = muList.mapToLong(list -> list.stream()
                                        .mapToLong(Long::longValue)
                                        .sum())
                 .val();
```

### `mapToString()` 

This method is used to translate a `MockUnit<T>` into a more specific `MockUnitString`.

If we use `mapToString()` without any parameter then `toString()` gets called directly on the generated object.

Otherwise we can use `mapToString(Function<T, String>)` and perform additional processing in the function.

Example for generating integer values as Strings:

```java
MockUnitString mus = m.ints().mapToString();
String integer = mus.val();
```

Example for generating a `MockUnitString` from a `MockUnitInt` that returns "odd" or "even" depending on the numbers generated:

```java
MockUnitString oddOrEven = m.ints().mapToString(i -> i%2==0 ? "even" : "odd");
List<String> oddOrEvenList = oddOrEven.list(10).val();
// Possible Output: [odd, even, even, odd, odd, even, even, odd, odd, odd]
```

### `mapVals()`

This method allows us to translate a `MockUnit<T>` into a `MockUnit<Map<T, R>>`.

The values can be obtained from:
- Another `MockUnit`;
- From a `Supplier<K>`;
- From an `Iterable<K>`;
- From an generic array `K[]`;
- From primitives arrays: `int[]`, `double[]`, `long[]`;

Example for generating a `Map<Boolean, Integer>` from an array of `int[]`:

```java
int[] values = { 100, 200, 300, 400, 500, 600 };
Map<Boolean, Integer> map = mock.bools().mapKeys(keys).val();
// Possible Output: {false=500, true=600}
``` 

Because the `Map` cannot have duplicate keys, the result will always contain 2 keys (true, false).

Example for generating a `Map<Boolean, Long>` from `Supplier<Long>`

```java
Supplier<Long> longSupplier = () -> System.currentTimeMillis();
Map<Boolean, Long> map = mock.bools().mapVals(10, longSupplier).val();
// Possible Output: {false=1487951761873, true=1487951761873}
```

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

This method works exactly like `val()` but instead of the value, it first calls `toString()` on the object. 

If the generated object is `null` it returns empty String.

```java
String alwaysTrue = m.bools().probability(100.0).valStr();
// Possible Output: "true"
```

If the generated object is `null`, an alternative `String` value can be specified:

```java
String nullll = m.from(new String[]{ null, null, null})
                 .valStr("NULLLL");
// Output: "NULLLL"
````

### `set()`

This method is used to obtain a `MockUnit<Set<T>>` from a `MockUnit<T>`.

Example for generating a `Set<Integer>`:

```java
Set<Integer> setInt = m.ints().set(100).val();
```

*Note:* 100 in our case doesn't necessarily represents the size of the `Set`. If the random numbers are not unique the `Set` may have less than 100 elements.

### `stream()`

This method is used to obtain a `MockUnit<Stream<T>>` from a `MockUnit<T>`.

Example for generating an infinite `Stream<Boolean>`:

```java
Stream<Boolean> stream = mock.bools().stream().val();
```

