---
layout: post
title: "All Mighty Maven"
date: 2014-11-08 14:00:09 +0000
comments: true
categories: java xml maven
---

The long overdue introduction to Maven that I promised myself I would write.

<!-- more -->

## What is Maven

Quick answer: **EVERYTHING**

{% img center /images/everything.gif "Maven does EVERYTHING" %}

Ok, not really *everything*. But it sure seems like it. At least to the new guy.

Maven has (*at least*) the following jobs within an application:

* **Manifest**, in the sense that it holds information about the project, and also its structure.
* **Dependency manager**
* **Task runner**
* **Builder**

Personally, I feel like these are too many things to a single tool. But apparently the Java world has been happy with it for a long time, so why would I fight it? Also, all of this is configured with a single `pom.xml` file (yes, XML).

{% img center /images/deal-with-it.gif "Maven is configured with XML. Deal with it" %}

Maven also enforces a folder structure, something that I found very weird. Being so used in an enterprise context (a.k.a. *professional*), I would expect the structure of a Java webapp to be something set in stone by good practices and not a tool.


## Le `pom.xml`

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

    <!-- Basics -->
    <groupId>com.iampfac</groupId>
    <artifactId>example</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>war</packaging>
    <name>Example</name>
    <description>An Example. Duh!</description>

    <!-- Properties -->
    <properties>
        <!-- ... -->
        <very.useful.property>very useful value</very.useful.property>
        <!-- ... -->
    </properties>

    <!-- Dependencies -->
    <dependencies>
        <!-- ... -->
        <dependency>
            <groupId>org.example</groupId>
            <artifactId>java-lib</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!-- ... -->
    </dependencies>

    <build>
        <finalName>${project.artifactId}</finalName>
    </build>
</project>
```
