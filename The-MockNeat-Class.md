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
| [`mock.fmt()`](#fmt) | `Formatter` | The `Formatter` class implements `MockUnitString`. It is used to generate formatted Strings. |
| [`mock.ints()`](#ints) | `Ints` | The `Ints` class implements `MockUnitInt`. It used to generate integer numbers. |
| [`mock.intSeq()`](#intseq) | `IntSeq` | The `IntSeq` class implements `MockUnitInt`. It used to generate integer numbers in a sequence. |
| [`mock.ipv4s()`](#ipv4s) | `IPv4s` | The `IPv4s` class implements `MockUnitString`. It is used to generate arbitrary IPv4 addresses. |
| [`mock.ipv6()`](#ipv6s) | `IPv6s` | The `IPv6s` class implements `MockUnitString`. It is used to generate arbitrary IPv6 addresses. |
| [`mock.localDates()`](#localdates) | `LocalDates` | The `LocalDates` class implements `MockUnitLocalDate`. It is used to generate random date objects. |
| [`mock.longs()`](#longs) | `Longs` | The `Longs` class implements `MockUnitLong`. It is used to generate random long numbers. |
| [`mock.longSeq()`](#longseq) | `LongSeq` | The `LongSeq` class implements `MockUnitLong`. It is used to generate long numbers in a sequence. |
| [`mock.macs()`](#macs) | `Macs` | The `Macs` class implements `MockUnitString`. It is used to generate MAC addresses. |
| [`mock.markovs()`](#markovs) | `Markovs` | The `Markovs` class implements `MockUnitString`. It is used to generate Markov Text. |
| [`mock.objs()`](#objs) | `Objs<T>` | The `Objs<T>` class implements `MockUnit<T>`. It is used to generate instances of class `T`. |
| [`mock.objs().constructor()`](#objsconstructor) | `Objs<T>` | The `Objs<T>` class implements `MockUnit<T>`. It is used to generate instances of class `T`. |
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
|`FOREX_PAIRS`| Contains a list of the most popular Forex Currency Pairs. This dictionary is used internally by [`currencies().forexPair()`](#currenciesforexpair). Possible values: "USD/MXN", "USD/NOK", "USD/PLN", etc. |
|`CREDIT_CARD_NAMES`| Contains a list of the most popular credit card names. This dictionary is used internally by [`creditCards().names()`](#creditcardsnames). Possible values: "Visa", "Mastercard", etc. |
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
```

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

Example to generate a single double value in interval [0.0, bound)

```java
Double bound = 10.0;
Double boundedVal = mock.doubles().bound(bound).val();
// Possible Output: 7.9842438463207905
```

Example to generate a single double value in a given range [100.0, 200.0)

```java
Double valInRange = mock.doubles().range(100.0, 200.0).val();
// Possible Output: 194.88613464097585
```

### `emails()`

This method is used to generate emails. 

Example to generate a random email address:

```java
String email = mock.emails().val();
// Possible Output: icedvida@yahoo.com
```

Example to generate a random email with a fixed "domain". This is useful when generating emails for a specific "company".

```java
String corpEmail = mock.emails().domain("startup.io").val();
// Possible Output: tiptoplunge@startup.io
```

Example to generate an email with fixed "domains":

```java
String domsEmail = mock.emails().domains("abc.com", "corp.org").val();
// Possible Output: funjulius@corp.org
```

### `files()`

This method is useful to generate random lines from a given file. 
The file is loaded in memory only once, so the recommendation is to avoid keeping large files into memory.

Example to generate a random line from a file '/Users/nomemory/Desktop/test.txt'. The file contents are:

```
text1
text2
text3
text4
```

```java
String line = mock.files().from("/Users/andreinicolinciobanu/Desktop/test.txt").val();
// Possible Output: text2
```

### `floats()`

This method is used to generate float numbers.

Example to generate a single float value in the interval [0.0f, 1.0f):

```java
Float val = mock.floats().val();
// Possible Output: 0.414554
```

Example to generate a single float value in interval [0.0f, bound), where bound=10.0f:

```java
Float boundVal = mock.floats().bound(10f).val();
// Possible Output: 2.0780544
```

Example to generate a single double value in a given range [100.0f, 200.0f)

```java
Float rangeVal = mock.floats().range(100.0f, 200.0f).val();
// Possible Output: 163.93002
```

### `fmt()` 

This method is used to generate formatted `String` values. Each specified param is a named one `#{param1}`.

Example of generating a random string in the form: `#{digit1}#{digit2}#{letter}#{specialChar}` (eg.: `"12a@"`):

```java
String result = mock.fmt(templ)
                    .param("d1", mock.chars().digits())
                    .param("d2", mock.chars().digits())
                    .param("l1", mock.chars().letters())
                    .param("sc", mock.from(SPECIAL_CHARACTERS))
                    .val();
// Possible Output: 45q'
```

### `ints()`

This method is used to generate int numbers.

Example of generating int numbers (negative or positive):

```java
Integer i1 = mock.ints().val();
// Possible Output :1952620297
```

Example of generating int numbers bounded by a certain value: [0, bound), where bound = 10:

```java
Integer bounded = mock.ints().bound(10).val();
// Possible Output: 4
```

Example of generating int numbers in a certain open range: [10, 20):

```java
Integer ranged = mock.ints().range(10, 20).val();
// Possible Output: 19
```

### `intSeq()`

This method is used to generate sequence of numbers (integers). 

Example of generating a seq of integers starting with 0, incrementing each time with 1.

```java
IntSeq seq = mock.intSeq();
for(int i = 0; i < 20; i++) {
      System.out.print(seq.val() + " ");
}
// Output: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 
```

Example of generating a sequence of integers starting with 5, incrementing with 2, and if it reaches a value bigger than 20 it resets back to 5:

```java
IntSeq seq2 = mock.intSeq()
                  .start(5)
                  .increment(2)
                  .max(20)
                  .cycle(true);
for(int i = 0; i < 50; i++) {
    System.out.print(seq2.val() + " ");
}
// Output: 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 
```

Negative increments work, but instead of setting a `max()` value a `min()` needs to be set, otherwise it will default to `Integer.MIN_VALUE`.

### `ipv4s()`

This method is used to generate arbitrary IPv4 addresses.

An enum `IPv4Type` exists and it describes each possible IPv4 classes: `CLASS_A`, `CLASS_A_LOOPBACK`, `CLASS_A_PRIVATE`, `CLASS_B`, `CLASS_B_PRIVATE`, `CLASS_C`, `CLASS_C_PRIVATE`, `CLASS_D`, `CLASS_E`, `NO_CONSTRAINT`.

Example of generating an IPv4 address that has no class constraint:

```java
String ipv4 = mock.ipv4s().val();
// Possible Output: 192.21.139.180
```

Example of generating an IPv4 address from Class A:
```java
String ipClassA = mock.ipv4s().type(CLASS_A).val();
// Possible Output: 57.253.133.154
```

Example of generating an IPv4 address from Class A or Class B:
```java
String classAorB = mock.ipv4s().types(CLASS_A, CLASS_B).val();
// Possible Output: 119.110.27.146
```

### `ipv6s()`

This method is used to generate an arbitrary IPv6 address.

Example:

```java
String ipv6 = mock.iPv6s().val();
// Possible Output: 35f1:b02f:8843:9abb:82bf:967a:34f5:ed8b
```

### `localDates()` 

This method is used to generate arbitrary `LocalDate` objects.

Example of generating a random date in the past between: [1970, now()):

```java
LocalDate localDate = mock.localDates().val();
// Possible Output: 1984-05-10
```

Example of generating a random date in the past between: [1987-1-30, now()):

```java
LocalDate min = LocalDate.of(1987, 1, 30);
LocalDate past = mock.localDates().past(min).val();
// Possible Output: 2014-05-30
```

Example of generating a random date in the future between: [now(), 2020-1-1);

```java
LocalDate max = LocalDate.of(2020, 1, 1);
LocalDate future = mock.localDates().future(max).val();
// At this moment this is the possible output: 2019-08-03
```

Example of generating a random date between [1989-1-1, 1993-1-1):

```java
LocalDate start = LocalDate.of(1989, 1, 1);
LocalDate stop = LocalDate.of(1993, 1, 1);
LocalDate between = mock.localDates().between(start, stop).val();
// Possible Output: 1989-02-04
```

### `longs()`

This method is used to generate int numbers.

Example of generating long numbers (negative or positive):

```java
Long i1 = mock.longs().val();
// Possible Output : -2445487402940283422
```

Example of generating long numbers bounded by a certain value: [0, bound), where bound = 10:

```java
Long bounded = mock.longs().bound(10).val();
// Possible Output: 8
```

Example of generating long numbers in a certain open range: [10, 20):

```java
Long ranged = mock.longs().range(10, 20).val();
// Possible Output: 19
```

### `longSeq()`

This method is used to generate sequence of numbers (longs). 

Example of generating a seq of integers starting with 0l, incrementing each time with 1l.

```java
LongSeq seq = mock.longSeq();
for(int i = 0; i < 20; i++) {
    System.out.print(seq.val() + " ");
}
// Output: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 
```

Example of generating a sequence of longs starting with 5, incrementing with 2, and if it reaches a value bigger than 20 it resets back to 5:

```java
LongSeq seq2 = mock.longSeq()
                   .start(5)
                   .increment(2)
                   .max(20)
                   .cycle(true);
for(int i = 0; i < 50; i++) {
    System.out.print(seq2.val() + " ");
}
// Output: 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 9 11 13 15 17 19 5 7 
```

Negative increments work, but instead of setting a `max()` value a `min()` needs to be set, otherwise it will default to `Long.MIN_VALUE`.

### `macs()`

This method is used to generate arbitrary MAC addresses.

The MAC address can be formatted in a few ways. Each of these ways is mapped in the enum `MACAddressFormatType`: `DASH_EVERY_2_DIGITS`, `COLON_EVERY_2_DIGITS`, `DOT_EVERY_2_DIGITS`, `DOT_EVERY_4_DIGITS`.

Example of generating a MAC addresses and showing it in the format `DOT_EVERY_4_DIGITS`:
```java
String mac = mock.macs().type(DOT_EVERY_4_DIGITS).val();
// Possible Output: 9a25.aa24.df7c
```

### `markovs()`

This method is used to generate Markov Text.

For the moment this feature is not: 
- very memory efficient;
- there aren't many options, but to generate text from Kafka's Metamorphosis.

Example in generating a 512 characters text:

```java
String text = mock.markovs().size(512).type(KAFKA).val();
//Possible Output: In his efforts to muster all the things that concerned him. This meant not only in secret. For two whole days, all the harder. Across the room came in early in the doorway with a knife, he thought it was easy to see him. He had forgotten, but instead of being afraid, the charwoman stood in the direction of the bedrooms: come and give me a bit of him and seemed, at first, that was quite incapable of going backwards in a nice, gilded frame. It showed a lady fitted out with a few raisins and almonds; some chee
```

### `objs()`

This method is used to mock instantiate objects.

The class fields are being set through reflection directly, and not through setter(s). This means that private fields are also accessible. The instantiation of the object is done through the `NO_ARGS` constructor.

There's another method on [`objs().constructor()`](#objsconstructor) that uses a public constructor for generating the object. 

Example of mocking a custom class `Test` that has 3 fields:
- String x;
- Integer y;
- Boolean z;

```java
Test t = mock.objs(Test.class)
             .field("x", mock.strings().size(10))
             .field("y", mock.ints().range(100, 200))
             .field("z", mock.bools())
             .val();
// Possible Output: Test{x='Y9mGANWaY1', y=185, z=true}
```

### `objs().constructor()`

This method is used to instantiate objects through public constructors.

The constructor that is going to be used is going to be inferred based on the list of arguments supplied to the method.

Example of mocking a custom class `Test` that has a public constructor `public Test(String x, Integer y, Boolean z)`:

```java
Test t2 = mock.objs(Test.class)
                .constructor(
                    mock.strings().size(10),
                    mock.ints().range(0, 10),
                    mock.bools())
                .val();
// Possible Output: Test{x='BfMT25Glvu', y=4, z=false}
```

### months()

This method is used to generate Months names. It is used to return `Month` objects.

Exmaple to generate a `Month`:

```java
Month m = mock.months().val();
// Possible Output: FEBRUARY
```

Example to generate a month in the range `[June, August]` (summer):

```java
Month summer = mock.months().rangeClosed(JUNE, AUGUST).val();
// Possible Output: August
```

Note: You can use the `range()` method for an open-interval.

Example to generate a month before the start of the summer, in range [January, June)

```java
Month beforeSummer = mock.months().before(JUNE).val();
// Possible Output: APRIL
```

### money()

This method is used to generate money amounts as Strings (Money Symbol + Sum of Money).

Example to generate a "fortune" in Japan. By fortune I mean a value in [100, 200) interval (YEN):

```java
String japanFortune = mockNeat.money()
                                .locale(JAPAN)
                                .range(100, 200)
                                .val();
// Possible Output: ￥138
```

Example to generate millions of Euros:

```java
String millions  = mockNeat.money()
                           .locale(GERMANY)
                           .range(1000000, 10000000)
                           .val();
Possible Output: 1.604.518,56 €
```

### `names()`

This method is used to generate both first names and last names.

There is an enum class `NameType` that maps names into several categories:
- `NameType.FIRST_NAME` : Contains both female and male first names;
- `NameType.FIRST_NAME_MALE` : Contains only male first names;
- `NameType.FIRST_NAME_FEMALE` : Contains only female first names;
- `NameType.FIRST_NAME_MALE_AMERICAN` : Contains only male first names that are popular in the US;
- `NameType.FIRST_NAME_FEMALE_AMERICAN` : Contains only female first names that are popular in the US;
- `NameType.LAST_NAME` : Contains only last names;

To use those classifications the `names()` method must be used in conjunction with `type()`.

Example to generate name of type `FIRST_NAME_FEMALE` :

```java
String firstName = mock.names().type(FIRST_NAME_FEMALE).val();
// Possible Output: Allie
```

There are also shortcuts for generating first names, last names and full names with optional initials:

Example for generating an arbitrary first name (male or female):

```java
String first = mock.names().first().val();
```

Example for generating an arbitrary last name:

```java
String last = mock.names().last().val();
```

Example for generating a full name that has a 90% chance to contain an "initial":

```java
String withInitial = mock.names().full(90).val();
// Possible Output: Bryce I. Hibma
```

### `passwords()`

This method is used to generate passwords.

Different types of password strengths can be generated, all strengths are mapped into the enum `PassStrengthType`: `WEAK`, `MEDIUM`, `STRONG`.

Example on how to generate a medium-strength password:

```java
String medium = mock.passwords().type(MEDIUM).val();
// Possible Output: cent>ilLion
```

###