The `MockNeat` is composed by a set of objects that help us generate arbitrary data.

Each of those objects is an implementation of the interface `MockUnit<T>` which is described in great detail in the section ["Introducing Mock Units"](https://github.com/nomemory/mockneat/wiki/mockunits). 

The most important methods that can be accessed on the `MockNeat` object are:

| Method | Return Object Type | Description |
|:------ |:------ |:----------- |
| [`mock.bools()`](#bools) | `Bools` | The `Bools` class implements `MockUnit<Boolean>`. It is used to generate arbitrary `Boolean` values. |
| [`mock.chars()`](#chars) | `Chars` | The `Chars` class implements `MockUnit<Character>`. It is used to generate arbitrary character values (letters, digits, special characters, etc.). |
| [`mock.creditCards()`](#creditcards) | CreditCards | The `CreditCards` class implements `MockUnitString`. It is used to generate valid Credit Card Numbers (numbers that pass the [Luhn Check](https://en.wikipedia.org/wiki/Luhn_algorithm)). Using [`mock.creditCards().names()`](#creditcardsnames) it is possible to generate Credit Card names as Strings. |
| [`mock.currencies()`](#currencies) | `Currencies` | This is a helper class that groups different methods related to currencies: [`mock.currencies().name()`](#currenciesname), [`mock.currencies().symbol()`](#currenciessymbol), [`mock.currencies().code()`](#currenciescode), [`mock.currencies().forexPair()`](#currenciesforexpair) |
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

### `currencies()`

This method is only used to create an instance of the `Currencies` object. 

The most important methods attached to the `Currencies` object are:

- [`mock.currencies().name()`](#currenciesname);
- [`mock.currencies().symbol()`](#currenciessymbol);
- [`mock.currencies().code()`](#currenciescode), 
- [`mock.currencies().forexPair()`](#currenciesforexpair)

### `currencies().name()`

This method returns currency names from around the world. Some of the possible outputs: "Leu", "Krona", "Pound", etc.

Example:

```java
String currencyName = mock.currencies().name().val()
```

### `currencies().symbol()`

This method returns currency symbols from around the world. Some of the possible outputs: "Lek", "£", "Ls", "$", etc.

Example:

```java
String currencySymbol = mock.currencies().symbol().val()
```

### `currencies().code()`

This method returns currency codes from around the world. Some of the possible outputs: "CZK", "JPY", "SVC", "ANG", etc.

Example:

```java
String currencyCode = mock.currencies().code().val();
```

### `currencies().forexPair()`

This method returns the most common [Forex Pairs](https://en.wikipedia.org/wiki/Currency_pair). Some of the possible outputs: "EUR/GBP", "USD/ZAR", "EUR/GBP", "EUR/HUF", etc.

Example:

```java
String forexPair = mock.currencies().forexPair().val();
```

### `cvvs()`

This method helps us generate [CVV](https://en.wikipedia.org/wiki/Card_security_code) codes. Currently it supports only two types of CVV. Those types are defined into the enum called `CVVType`. For the moment there are only two types defined: 
- `CVV3` - Represents a 3 digits random String.
- `CVV4` - Represents a 4 digits random String.

By default the type we use is: `CVV3`.

Example generating a 3-digit CVV:

```java
String cvv = mock.cvvs().val();
```

Example generating a 4-digit CVV:

```java
String cvv4 = mock.cvvs().type(CVV4).val();
```

### `dicts()`

The library comes with a set of internal dictionaries. Basically those dictionaries are plain-text files that are included in the `.jar` file. 

Each of these files are mapped through an enum called `DictType`.

The current list of `DictType` is:

| Enum Value | Contents |
|:---------- |:---------|
|`COUNTRY_NAME`| Contains an exhaustive list of country names. Possible values: "Chile", "Saudi Arabia", etc.|
|`COUNTRY_ISO_CODE_2` | Contains an exhaustive list of country iso codes. Possible values: "AM", "UK", etc. |
|`DOMAIN_EMAIL` | Contains a list of possible email domains. This dictionary is used internally by [`emails()`](#emails). Possible values: "gmail.com", "msn.com", etc. |
|`DOMAIN_TOP_LEVEL_ALL`| Contains an exhaustive list of possible URL domains (suffixes). This dictionary is used internally by [`urls()`](#urls). Possible values: "com", "comcast", "ml", etc. |
|`DOMAIN_TOP_LEVEL_POPULAR`| Contains a list of the most popular URL domains (suffixes). This dictionary is used internally by [`urls()`](#urls). Possible values: "com", "org", "net", etc. |
|`FOREX_PAIRS`| Contains a list of the most popular Forex Currency Pairs. This dictionary is used internally by [`currencies().forexPair()`](#currenciesPair). Possible values: "USD/MXN", "USD/NOK", "USD/PLN", etc. |
|`CREDIT_CARD_NAMES`| Contains a list of the most popular credit card names. This dictionary is used internally by [`creditCards().names()`](#creditCardsnaems). Possible values: "Visa", "Mastercard", etc. |
|`FIRST_NAME_MALE_AMERICAN`| Contains the most common first names for males in the US. |
|`FIRST_NAME_FEMALE_AMERICAN` | Contains the most common first names for females in the US. |
|`LAST_NAME_AMERICAN`| Contains the most common last names in the US. |
|`EN_ADJECTIVE_1SYLL`| Contains a list of 1-Syllable English adjectives. |
|`EN_ADJECTIVE_2SYLL`| Contains a list of 2-Syllable English adjectives. |
|`EN_ADJECTIVE_3SYLL`| Contains a list of 3-Syllable English adjectives. |
|`EN_ADJECTIVE_4SYLL`| Contains a list of 4-Syllable English adjectives. |
|`EN_ADVERB_1SYLL`| Contains a list of 1-Syllable English adverbs. |
|`EN_ADVERB_2SYLL`| Contains a list of 2-Syllable English adverbs. |
|`EN_ADVERB_3SYLL`| Contains a list of 3-Syllable English adverbs. |
|`EN_ADVERB_4SYLL`| Contains a list of 4-Syllable English adverbs. |
|`DEPARTMENTS`| Contains a list of possible department names from a company. Possible values: "Insurance", "Inventory", "Licenses" |

Example for generating a country name directly from the dictionary:

```java
String countryName = mock.dicts().type(COUNTRY_NAME).val()
```

### `days()`

This method helps us generate name of the Days of the week.

Example to generate an arbitrary day of the week:

```java
DayOfWeek day = mock.days().val();
// Possible Output: SUNDAY
```

Example to generate an arbitrary day of the week as `String` using `display()`:

```java
String dayStr = mock.days().display(TextStyle.SHORT).val();
// Possible Output: "Tue"
```

Example to generate an arbitrary day of week after "Thursday" using `after()`:

```java
DayOfWeek dayAfterThursday = mock.days().after(THURSDAY).val();
// Possible Output: "FRIDAY"
```

Example to generate an arbitrary day of the week before "Thursday" using `before`:

```java
DayOfWeek dayBeforeThursday = mock.days().before(THURSDAY).val();
// Possible Output: "MONDAY"
```

Example to generate an arbitrary day of the week between [Sunday, Saturday] using `rangeClosed()`:
```java
DayOfWeek weekEnd = mock.days().rangeClosed(SATURDAY, SUNDAY).val();
```

Note: There is also the `range()` method, that uses an open interval.

### `departments()`

This method is used to generate department names from a company.

```java
String dept = mock.departments().val();
// Possible Output: "Insurance"
```

### `domains()`

This method is used to generate domains for URLs. 

There are two types of domains that can be generated:
- `DomainSuffixType.ALL` - This contains an exhaustive of possible domains;
- `DomainSuffixType.POPULAR` - This is a shorter list of domains, containing only the most popular: "com", "net", "org", etc. If not type is specified this is picked by default.

Example to generate a domain `String`:

```java
String domain = mock.domains().val();
// Possible Output: "io"
``

Example to generate a domain `String` giving the type:
```java
String all = mock.domains().type(ALL).val();
// Possible Output: "analytics"
```
### `doubles()`

This method is used to generate double values.

Example to generate a single double value in the interval [0.0, 1.0):

```java
Double val = mock.doubles().val();
// Possible Output: 0.26378031782078615
```

Example to generate a single double value bounded, in interval [0.0, bound)
```java
Double bound = 10.0;
Double boundedVal = mock.doubles().bound(bound).val();
// Possible Output: 7.9842438463207905
```

