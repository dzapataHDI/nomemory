# Introducing MockUnits

Each main `MockNeat` object is composed by different implementations of the `MockUnit<T>`, `MockUnitInt`, `MockUnitLong`, `MockUnitDouble`, `MockUnitString`, `MockUnitDays`, `MockUnitMonths`, `MockUnitLocalDate` interfaces.

Those implementations that are used to generate sets of data in a very generic and flexible way.

The `MockUnit<T>` is the generic interface from which all the other MockUnits inherit their methods. 

The rest of the interfaces are extending `MockUnit<T>` by enhancing it with methods that are very specific to their enclosed types (`Integer`, `String`, etc.)