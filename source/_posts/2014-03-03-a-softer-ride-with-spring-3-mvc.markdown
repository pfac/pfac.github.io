---
layout: post
title: "A Softer Ride with Spring 3 MVC"
date: 2014-03-03 15:44
comments: true
published: false
categories: 
---

Recently I was told by a senior developer to move away from Struts 2 ("it is dying"), instead performing the task I had at hand using Spring MVC. This post is the description of my crash landing in the Spring world and, hopefully, a tutorial for some beginners out there with the same doubts as me.

<!-- more -->

First thing I must state even before I begin, is that I have read multiple times that Spring is for Java EE as Rails is for Ruby for the web. While I do not disagree, some things remain confusing.

## Convention over... wait, how does this even work?

One of the most confusing things I encountered when I faced Java EE for the first time (in a professional context) was Maven. I spent many hours trying to understand it.

The first commands I used were `mvn install` and `mvn clean`. Coming from a C++ background, I initially thought of Maven like a build system, CMake and Makefiles coming together. 

Later, I found out that it also managed dependencies. Coming from a Ruby background, I associated Maven with Gemfiles and `bundle`.

Then I found it also creates project skeletons so developers can skip the boilerplate code. Coming from Ruby-on-Rails, this was my `rails new`.

And then came task running (`rake`?), and documentation generation (`doxygen`?)...

The bottom line is: Maven does too much. It is a very confusing tool and the world divides in those that give it complete control over their projects and those that believe it complicates beyond reason. The truth is you have to decide early whether you'll it in a project, and stick with your decision, because changing it half-way through might cost too much.

Being a sort of company policy to use Maven in our projects, I embraced it in my stuff, like it or not. When learning Struts 2, this came in handy. I could use the Struts 2 archetype to have a project skeleton with the expected structure, all the dependencies and sample configuration already there. Studying these defaults gave me a better notion of what Struts expected. Yet, the getting started tutorial uses some weird dependencies (`org.springframework.boot:spring-boot-starter-web`) and a parent project, leaving the user without knowing what it needs to start a project outside the scope of the Spring Starter Guides.


