# From sources:

Clone it from github:

```
git clone https://github.com/nomemory/mockneat.git
```

Build the "fat jar" - the jar that contains MockNeat and all its dependencies: 

```
gradle shadowJar
```

This will create in the folder `./build/libs/` there a `.jar` file: `mockneat-<version>-all.jar`. 

Copy this jar into your projects classpath and you are good to go.

# With gradle

To check the latest version please check the library's [jcenter page](https://bintray.com/nomemory/maven/mockneat).

Add `jcenter()` as a repository in your gradle.build file:

```
repositories {
    // something else
    jcenter()
}
```

Add the following line a dependency:

```
dependencies {
      // something else
     compile 'net.andreinc.mockneat:mockneat:0.1.0'
}
```

## With Maven

To check the latest version please check the library's [jcenter page](https://bintray.com/nomemory/maven/mockneat).

pom.xml dependency:

```xml
    <repositories>
        <repository>
            <id>jcenter</id>
            <url>https://jcenter.bintray.com/</url>
        </repository>
    </repositories>

    <dependencies>
        <dependency>
            <groupId>net.andreinc.mockneat</groupId>
            <artifactId>mockneat</artifactId>
            <version>0.1.1</version>
        </dependency>
    </dependencies>
```
