In order to start creating random data for your application the first thing is to create or to obtain an existing instance of the ``MockNeat`` class. This is the main class of the library and the source of *all evil*. 

There are a few ways you can obtain this:

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

* Create a new instance of the ```MockNeat``` class using a predefined RandomType and a seed:

```java
Long seed = 123l;
MockNeat mock = new MockNeat(RandomType.SECURE, seed);
```


