# Introducing MockUnits

Each main `MockNeat` object is composed by `MockUnit<T>`, `MockUnitInt`, `MockUnitLong`, `MockUnitDouble`, `MockUnitString`, `MockUnitDays`, `MockUnitLocalDate` objects that are used to generate sets of data in a very generic and flexible way.

The `MockUnit<T>` is the generic interface from which all the MockUnits inherit their methods. 

The rest of the units are particular implementations that are enhancing the general interface with methods that are specific to their enclosed types (Integer, String, etc.)

## MockUnit<T>

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


### The `val` method

If we want to obtain a Boolean we will call the val() method on the `MockUnit` instance:

```java
MockUnit<Boolean> boolUnit = mock.bools().probability(35.50);
Boolean val = boolUnit.val();
```

We can write all of the above simpler without keeping the reference:

```java
Boolean val = mock.bools().probability(35.50).val();
```

### The `list` method

Let's say we want to re-use the same generator but this generate `List<Boolean>` values, while keeping the same constraints. The List implementation we want to use is `LinkedList`.

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

And we can go like this forever... *(well not exactly because after a few thousand `List<List<List....<Boolean>..>`  we will eventually receive a `StackOverFlow` Exception)*.

For making the code look more clean, there's no reason why we should keep references of `MockUnit`s all the time.  
We can simply start chaining our methods starting with `MockNeat` instance:

```java
List<Boolean> list = mock.bools().probability(35.50).list(100).val();
List<List<Boolean>> listList = mock.bools().probability(35.50).list(100).list(100).val();
List<List<List<Boolean>>> listListList = mock.bools().probability(35.50).list(100).list(100).list(100).val();
// Show can go on (if there's a good reason...)
```


