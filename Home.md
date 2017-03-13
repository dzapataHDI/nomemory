MockNeat is a **Java 8+** library that facilitates the generation of arbitrary test data for your application(s). 

**MockNeat** helps you create: 

- arbitrary values for the basic java types: [`bools()`](MockNeat#bools), [`chars()`](MockNeat#chars), [`doubles()`](MockNeat#doubles), [`floats()`](MockNeat#floats), [`ints()`](MockNeat#ints), [`longs()`](MockNeat#longs), [`strings()`](MockNeat#strings);
- arbitrary time related information: [`days()`](MockNeat#days), [`months()`](MockNeat#months), [`localDates()`](MockNeat#localdates).
- arbitrary user information: [`emails()`](MockNeat#emails), [`users`](MockNeat#users), [`uuids()`](MockNeat#uuids), [`names()`](MockNeat#names), [`passwords()`](MockNeat#passwords);
- arbitrary text based on [Markov Models](https://en.wikipedia.org/wiki/Markov_model): [`markovs()`](MockNeat#markovs);
- arbitrary data related to networking: [`ipv4s()`](MockNeat#ipv4s), [`ipv6s()`](MockNeat#ipv6s), [`macs()`](MockNeat#macs), [`urls()`](MockNeat#urls);
- arbitrary data for financial apps: [`creditCards()`](MockNeat#creditcards), [`currencies()`](MockNeat#currencies), [`money()`](MockNeat#money),
- arbitrary geographical data;
- instantiate java objects with arbitrary values (based on a set of conditions): [`constructor()`](MockNeat#constructor), [`factory()`](MockNeat#factory), [`reflect()`](MockNeat#reflect);
- construct complex collections structures and fill them with arbitrary values: [`collection()`](MockUnit#collection), [`mapKeys()`](MockUnit#mapkeys), [`mapVals()`](MockUnit#mapvals), [`set()`](MockUnit#set);
- *and much-much more*.

