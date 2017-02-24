`MockUnit<T>` is a [Functional Interface](https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html) that contains only one abstract method: `Supplier<T> supplier();`.

**Example**: Creating a custom MockUnitString that returns random strings with the following format: `[letter][digit][letter]`. Examples: "a2b", "w9z", "f8k", etc.

```java
Supplier<String> supp = () -> {
    StringBuilder buff = new StringBuilder();
    buff.append(mock.chars().digits().val())
        .append(mock.chars().lowerLetters().val())
        .append(mock.chars().digits().val());
        return buff.toString();
};
MockUnitString mockUnit = () -> supp;

// The custom created mockUnit can now be used
// as any other MockUnitString
String cStr = mockUnit.val();
List<String> lStr = mockUnit.list(10).val();
//etc.
```







