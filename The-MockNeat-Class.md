The `MockNeat` is composed by a set of objects that help us generate arbitrary data.

Each of those objects is an implementation of the interface `MockUnit<T>` which is described in great detail in the section ["Introducing Mock Units"](https://github.com/nomemory/mockneat/wiki/mockunits). 

Composing Objects:

| Method | Return Object Type | Description |
|:------ |:------ |:----------- |
| [`mock.bools()`](#bools) | `Bools` | The `Bools` class implements `MockUnit<Boolean>`. It is used to generate arbitrary `Boolean` values. |
| [`mock.chars()`](#chars) | `Chars` | The `Chars` class implements `MockUnit<Character>`. It is used to generate arbitrary character values (letters, digits, special characters, etc.). |
| [`mock.creditCards()`](#creditCards) | CreditCards | The `CreditCards` class implements `MockUnitString`. It is used to generate valid Credit Card Numbers (numbers that pass the [Luhn Check](https://en.wikipedia.org/wiki/Luhn_algorithm)). Using [`mock.creditCards().names()`](#creditcardsnames) it is possible to generate Credit Card names as Strings. |
| [`mock.currencies()`](#currencies) | `Currencies` | This is a helper class that groups different methods related to currencies: [`mock.currencies().name()`](#currencies().name()), [`mock.currencies().symbol()`](#currencies().symbol()), [`mock.currencies().code()`](1), [`mock.currencies().forexPair()`](2) |
| [`mock.cvvs()`](#cvvs) | `CVVS` | The `CVVS` class implements `MockUnitString`. It is used to generate CVV codes for Credit Cards (3 or 4 digit numbers).
| [`mock.dicts()`](#dicts) | `Dicts` | The `Dicts` class is used as a an utility class that facilitates generating random lines from the internal library dictionaries. The internal dictionaries are collections of data grouped into files. The enum `DictType` maps all those files. |
| [`mock.days()`](#days) | `Days` | This `Days` class implements `MockUnitDays`. It is used to generate days of the week.|
| [`mock.departments()`](#departments) | `Departments` | The `Departments` class implements `MockUnitString`. It is used to generate arbitrary department names from company. |
| [`mock.domains()`](#domains) | `Domains` | The `Domains` class implements `MockUnitString`. It is used to generate web domains like 'com', 'net', 'org' and so on. |
| [`mock.doubles()`](#doubles) | `Doubles` | The `Doubles` class implements `MockUnitDouble`. It is used to generate double numbers. |
| [`mock.emails()`](#emails) | `Emails` | The `Emails` class implements `MockUnitString`. It is used to generate emails. |
| [`mock.files()`](#files) | `Files` | The `Files` class implements `MockUnitString`. It is used to read random lines from "external" files that are loaded in memory. |
| [`mock.floats()`](#floats) | `Floats` | The `Floats` class implements `MockUnit<Float>`. It is used to generate float numbers. |
| [`mock.formatter()`](#formatter) | `Formatter` | The `Formatter` class implements `MockUnitString`. It is used to generate formatted Strings. |
| [`mock.ints()`](#ints) | `Ints` | The `Ints` class implements `MockUnitInt`. It used to generate integer numbers. |
| [`mock.intSeq()`](#intSeq) | `IntSeq` | The `IntSeq` class implements `MockUnitInt`. It used to generate integer numbers in a sequence. |
| [`mock.ipv4s()`](#ipv4s) | `IPv4s` | The `IPv4s` class implements `MockUnitString`. It is used to generate arbitrary IPv4 addresses. |
| [`mock.ipv6()`](#ipv6s) | `IPv6s` | The `IPv6s` class implements `MockUnitString`. It is used to generate arbitrary IPv6 addresses. |
| [`mock.localDates()`](#localDates) | `LocalDates` | The `LocalDates` class implements `MockUnitLocalDate`. It is used to generate random date objects. |
| [`mock.longs()`](#longs) | `Longs` | The `Longs` class implements `MockUnitLong`. It is used to generate random long numbers. |
| [`mock.longSeq()`](#lonSeq) | `LongSeq` | The `LongSeq` class implements `MockUnitLong`. It is used to generate long numbers in a sequence. |
| [`mock.macs()`](#macs) | `Macs` | The `Macs` class implements `MockUnitString`. It is used to generate MAC addresses. |
| [`mock.markovs`](#markovs) | `Markovs` | The `Markovs` class implements `MockUnitString`. It is used to generate Markov Text. |
| [`mock.objs(Class<T>)`](#objs) | `Objs<T>` | The `Objs<T>` class implements `MockUnit<T>`. It is used to generate instances of class `T`. |
| [`mock.months()`](#months) | `Months` | The `Months` class implements `MockUnitMonth`. It is used to generate month names. |
| [`mock.money()`](#money) | `Money` | The `Money` class implements `MockUnitString`. It used to generate random sums of money for a certain `Locale`. |
| [`mock.names()`](#names) | `Names` | The `Names` class implements  `MockUnitString`. It is used to generate random names. |
| [`mock.passwords()`](#passwords) | `Passwords` | The `Passwords` class implements `MockUnitString`. It is used to generate random passwords. |
| [`mock.strings()`](#strings) | `Strings` | The `Strings` class implements `MockUnitString`. It is used to generate random strings. |
| [`mock.urls()`](#urls) | `URLs` | The `URLs` class implements `MockUnitString`. It is used to generate arbitrary URL values. |
| [`mock.uuids()`](#uuids) | `UUIDs` | The `UIDs` class implements `MockUnitString`. It is used to generate unique identifiers. |
| [`mock.users()`](#users) | `Users` | The `Users` class implements `MockUnitString`. It is used to generate random usernames. |


#### `bools()`

This method help us generate `Boolean` values.

Example:
```
MockNeat m = MockNeat.threadLocal();
boolean b = m.bools().val();
```

Example generating a boolean value that has 99.99% of being `true`:
```java
boolean almostAlwaysTrue = m.bools().probability(99.99).val();
```

### `chars()` 

This method helps us generate `Character` values.

Example generating a lower letter (eg: 'a', 'b', ..., 'z'):
```java
Character lowerLetter = mock.chars().lowerLetters().val();
```

Example generating an upper letter (eg.: 'A', 'B', ... , 'Z'):
```java
Character upperLetter = mock.chars().upperLetters().val();
```

Example generating a digit (eg: '1', '2', ... , '0'):
```java
Character digit = mock.chars().digits().val();
```

Example generating a hex value:
```java
Character hex = mock.chars().hex().val();
```

### `creditCards()`

This method help us generate Credit Cards.

There a few supported types of Credit Cards defined in the `CreditCardType` enum. 

Example generating a credit card, by default the `CreditCardType` is `AMERICAN_EXPRESS`:

```java
String amex = m.creditCards().val()
```

Example generating a 16 digits VISA:

```java
String visa16 = m.creditCards().type(VISA_16).val();
```

Example generating an arbitrary credit card that can be `VISA_16` or `MASTERCARD`. 

```
String visaOrMastercard = m.creditCards()
                           .types(VISA_16, MASTERCARD)
                           .val();
```

### `creditCards().names()` 

This generates the name of a Credit Card.

The possible returning values are: `"American Express"`, `"China Union Pay"`, `"Diners Club"`,  `"Discover"`,  `"Inter Payment"`, `"Insta Payment"`, `"JCB"`, `"Maestro"`, `"Mastercard"`, `"Visa"`.

Example:

```java
String ccName = m.creditCards().names().val();
```