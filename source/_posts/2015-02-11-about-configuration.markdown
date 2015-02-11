---
layout: post
title: "About configuration"
date: 2015-02-11 11:51:08 +0000
comments: true
published: false
categories: 
---

<!-- more -->

{% img right /images/toolbox.jpg 450 "Do you want fries with your order?" %}

What came first, the egg or the chicken?

A funny question but one that fits here, the egg being my configuration files and the chicken being my Spring application context.

Here's a little context. If you happen to be developing an application with Java, especially a MVC web application, chances are you're using the Spring framework. And if you're using it, you almost have to be using its dependency injection system with its beans, its `@Autowired`s and the sort. And if your're application's configuration comes from a standard properties file (typical in Java it seems), you're probably using `@PropertySource` (or the XML equivalent) to load it automatically into your app's environment.

Now, if you dwell in the worlds of Spring since version 3, you are probably using all sorts of `@EnableX` annotations, like, say, `@EnableGlobalMethodSecurity`. When your application starts, Spring will create an application context and start scanning and detecting all your annotations and magically configure everything automatically for your lazy ass (not that you have much of an option nowadays).

And you're happy that way. Code flows out of you. Tests pass, the code builds and results are puked out. Everything is ok in your little world.

But in comes your boss. Out of the blue (it always is) someone decided that you need a kill switch for that feature that `@EnableX` provides. You, being a good developer skilled in the arts of Spring immediately think *profiles*. Just one environment variable, or `-D` parameter for the JVM, and everything will be automatically handled by the framework. You just need a few annotations.

&quot;Sure, but&quot;, there's always a but, &quot;you should be able to configure that through the properties file. We don't want to have to change the application arguments or have to set up environment variables. Besides, clients are running the old version, and they use the properties file. You must use the properties file.&quot;

{% img left /images/aint-nobody-got-time.jpg 300 "Ain't nobody got time fo dat!" %}

So, what, do I have to find a way to load properties before Spring even starts working? Because hell knows I can't just undo whatever magic it has done after seeing that annotation. &quot;Exactly.&quot;




