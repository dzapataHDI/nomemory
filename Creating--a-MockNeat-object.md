`MockNeat` is the main class of the library and the source of *all evil*.

To obtain a `MockNeat` instance:

*  Re-use the default instance of a ```MockNeat``` class that internally uses a [ThreadLocalRandom](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadLocalRandom.html) implementation from the Java library.

```java
MockNeat mock = MockNeat.threadLocal();
```

* Re-use the default instance of a ```MockNeat``` class that internally uses a [SecureRandom](http://docs.oracle.com/javase/8/docs/api/java/security/SecureRandom.html) implementation from the Java library.

```java
MockNeat mock = MockNeat.secure();
```

* Re-use the default instance of a ```MockNeat``` class that internally uses the *old* [Random](https://docs.oracle.com/javase/8/docs/api/java/util/Random.html) class from the Java library. 

```java
MockNeat mock = MockNeat.old();
```

* Call the constructor:

```java
Long seed = 123l;
MockNeat mock = new MockNeat(RandomType.SECURE, seed);
```
