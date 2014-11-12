---
layout: post
title: "Tip&trick: Eclipse not building Maven project automatically"
date: 2014-11-12 12:06:26 +0000
comments: true
categories:
- tips&tricks
- java
- eclipse
- maven
- web
---

*Problem*: Eclipse not building a Maven project automatically before deploying it

<!-- more -->

Yesterday I spent over an hour trying to figure out why my webapp was not picking up my Spring annotations properly. After that dreadful 1h+, I found out that Eclipse was not building my webapp automatically before deploying it to the application server. This was the point at which I had to get up, leave the office and go to the gym, otherwise my laptop would now be reduced to its most elementary components.

Today I found out why. I created this project manually instead of using `mvn archetype:generate` because I didn't want to use an archetype (I like to have control over the stuff that's placed in my projects and I have no appreciation for the placeholders the generator creates).

When I created the `pom.xml` manually, I gave the project the type `pom`, because I meant to create several submodules inside it. I configured the project to be a Maven project at this time (right-click on the project > *Configure*  > *Convert to Maven Project*).

Later I decided I wouldn't be adding submodules for now, instead having this single project generate a WAR file. I changed the POM file to have the type `war`. Eclipse immediately detected the `web.xml` file and allowed me to deploy the project to a server. Yet, apparently, it did not know the project has to be built before, or it didn't know how.

To fix it, **make sure you update the Maven project in Eclipse** (right-click on the project > *Maven* > *Update project*).

I still think it should have picked it up automatically...
