---
layout: post
title: "All Mighty Maven"
date: 2014-11-12 01:14 +0000
comments: true
categories: [java, xml, maven]
---

The long overdue introduction to Maven that I promised myself I would write.

<!-- more -->

## What is Maven

Quick answer: **EVERYTHING**

{% img center /images/everything.gif "Maven does EVERYTHING" %}

Ok, not really *everything*. But it sure seems like it. At least to the new guy.

Maven may have (*at least*) the following jobs within an application:

* **Manifest**, holding information about the project and its structure;
* **Dependency manager**, having the list of dependencies and automatically load them from the registered repositories;
* **Task runner**, like `make` or `jake` but more confusing
* **Builder**, compiling all the code and packaging it nicely (this part is convenient, to say the least).

Personally, I felt like these were too many things to a single tool. But apparently the Java world had been happy with it for a long time, so why would I fight it?

An year later, I've seen a lot of things that make me appreciate Maven. Using a single `pom.xml` file (yes, I also thought XML was dead), I can set almost everything that describes my project. It even assumes a given [standard directory structure][1], which I strongly believe is preventing every ego in an enterprise from coming up with its own.


## Le `pom.xml`

A `pom.xml` file is rather intuitive. Start with a simple XML skeleton using the `project` tag:

``` xml
<project
    xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://maven.apache.org/POM/4.0.0
        http://maven.apache.org/xsd/maven-4.0.0.xsd
    "
>
    <modelVersion>4.0.0</modelVersion>
    <!-- ... -->
</project>
```

Note the `modelVersion` tag in this example. It states the version of the object model this POM is using. General rule, it doesn't change so I keep it in the skeleton.

Next we add some basic information regarding this project:

``` xml
    <!-- Basics -->
    <groupId>com.iampfac</groupId>
    <artifactId>example</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>war</packaging>
    <name>Example</name>
    <description>An Example. Duh!</description>
```

I believe this part is trivial enough. Next we have the properties. These can be used later in the file using the `${...}` notation.

``` xml
    <!-- Properties -->
    <properties>
        <!-- ... -->
        <very.useful.property>very useful value</very.useful.property>
        <!-- ... -->
    </properties>
```

For instance, one use the `${very.useful.property}` as the value for other tags in the document, and it would be the same as writing the value directly (but more DRY). The basic information tags also have their equivalent properties, but their value may not be the value of a property. So using the `${...}` to set the value of one of those tags would not work. As an example, the `artifactId` tag sets the value for `${project.artifactId}`.

Next, the dependencies:

``` xml
    <!-- Dependencies -->
    <dependencies>
        <!-- ... -->
        <dependency>
            <groupId>org.example</groupId>
            <artifactId>java-lib</artifactId>
            <version>1.2.3</version>
            <type>war</type>
            <scope>test</scope>
            <exclusions>
                <!-- ... -->
                <exclusion>
                    <groupId>org.example.another</groupId>
                    <artifactId>unwanted-lib</artifactId>
                </exclusion>
                <!-- ... -->
            </exclusions>
        </dependency>
        <!-- ... -->
    </dependencies>
```

Note that what identifies a dependency is the same information that we set above in the basics section of our POM file. Maven repositories use this information to index the packages. `type` and `scope` tags are optional and define the type of dependency and the scope where it is required; by default they hold the values `jar` and `compile`, respectively.

`exclusions` is also an optional tag but a little more special, as it allows to exclude any unwanted dependency of our dependency. For instance, the `spring-core` package includes the `commons-logging` package, but one may prefer to use SLF4J, in which case the `commons-logging` package would have to be included in the `exclusions` tag.

Lastly, the build related stuff:

``` xml
    <build>
        <finalName>${project.artifactId}</finalName>
        <plugins>
            <!-- ... -->
            <plugin>
                <groupId>org.example</groupId>
                <artifactId>maven.plugin</artifactId>
                <version>1.0</version>
                <!-- ... -->
            </plugin>
            <!-- ... -->
        </plugins>
    </build>
```

Here we can set the name of the package to be generated when building this project (for example, when the project is an webapp we can set the name of the resulting `.war` file), and add plugins which perform additional tasks like running tests, customizing the build process, etc...

## Conclusion

Honestly I don't know at this moment what else I can write about Maven, and since this post is already long I'll wrap it here. Maybe I'll add a section later with an example of how to structure the project with multiple POM files. If I feel like it.

{% img center /images/deal-with-it.gif "My blog, my rulez!" %}

[1]: http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html