The `MockNeat` is composed by a set of objects that help us generate arbitrary data.

Each of those objects is an implementation of the interface `MockUnit<T>` which is described in great detail in the section ["Introducing Mock Units"](https://github.com/nomemory/mockneat/wiki/mockunits). 

Composing Objects:

| Method | Object | Description |
|:------ |:------ |:----------- |
| [`mock.bools()`](#bools) | Bools | A helper object that implements `MockUnit<Boolean>` and help us generate random boolean values|


#### `bools()`

This method help us generate `Boolean` values.

Example:
```
MockNeat m = MockNeat.threadLocal();
boolean b = m.bools().val();
```

### `
