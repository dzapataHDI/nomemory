The `MockUnitString` extends the `MockUnit<String>` interface.

```java
@FunctionalInterface
public interface MockUnitString extends MockUnit<String>
```

This means that it "interits" all the methods from MockUnit<String>, but it also adds new functionalities related to String operations.

The easiest way to obtain a `MockUnitString` implementation is by calling [`mapToString()`](MockUnit#maptostring) on any `MockUnit<T>`. A lot of the methods are already returning `MockUnitString` by default. 

Methods that are particular to `MockUnitString`:

| Method | Description |
|:-------|:------------|
| [`append()`](#append) | Appends to an existing `MockUnitString` a constant String value and then it returns a new `MockUnitString` that takes in consideration the previous operation. |

### `append()` 

Example:

```java
// APPEND
String[] cityAppend = mock.cities()
                            .capitals()
                            .append("-001") // To each generated String we append "-001"
                            .array(10)
                            .val();
// Possible Output: [Bishkek-001, Ulaanbaatar-001, Kigali-001, Bratislava-001, Sarajevo-001, Kabul-001, Lusaka-001, Port Vila-001, Tegucigalpa-001, Astana-001]
```

