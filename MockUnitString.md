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
| [`array()`](#array) | The method is used to translate an existing `MockUnitString` to a new `MockUnit<String[]>`. |
| [`base64()`](#base64) | Transform the generated value by encoding it into *base64* and afterwards returns a new `MockUnitString`. |
| [`escapeCsv()`](#escapeCsv) | Escapes the previously generated String in order to be `.csv` compliant. Returns a new `MockUnitString` |
| `escapeEcmaScript()` | Escapes the previously generated String if it contains Ecma Script code. Returns a new `MockUnitString` |
| `escapeHtml()` | Escapes the previously generated String if it contains HTML code. Returns a new `MockUnitString` |
| `escapeXml()` | Escapes the previously generated String if it contains XML code. Returns a new `MockUnitString` |
|`format()`| Formats the previously generated String. Supported formats are defined by the StringFormatType

### `append()` 

Example:

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
### `array()`

Example:
```java
// ARRAY
String[] someDays = mock.days()
                        .display()
                        .array(10)
                        .val();

// Possible Output: [Saturday, Monday, Monday, Sunday, Sunday, Tuesday, Thursday, Friday, Monday, Wednesday]
```

### `base64()`

Example:
```java
// Generate a list of names
List<String> names = mock
                     .names()
                     .first()
                     .format(CAPITALIZED)
                     .list(5)
                     .val();
// Possible Output: [Carroll, Zane, Alfred, Brent, Loren]

// Using seq() we iterate through the previous list and we encode the strings
List<String> base64names = mock
                            .seq(names)
                            .mapToString()
                            .base64()
                            .list(5)
                            .val();
// Possible Output: [Q2Fycm9sbA==, WmFuZQ==, QWxmcmVk, QnJlbnQ=, TG9yZW4=]
```

### `escapeCsv()`

Example
```
String[] notFriendlyCsv = { "\"", /* OTHERS */};

String friendlyCSV = mock.from(notFriendlyCsv)
                         .mapToString()
                         .escapeCsv()
                         .val();
```


