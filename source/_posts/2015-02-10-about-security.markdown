---
layout: post
title: "About Security"
date: 2015-02-10 20:55:51 +0000
comments: true
categories: 
---

<!-- more -->

{% img right /images/security-fail.jpg 450 "'Much numbers, wow security'" %}

Here's my daily dilemma. I have an application, it uses Spring Security, and I needed to find a way to disable security altogether with a property in a configuration file. This made me wonder whether I really should do things as I am.

Currently, security rules are placed in the application core. This means that once security is enabled, the business logic incorporates the security rules. This also allows for services, which *perform* the application use cases, to be fully secured by default.

I do not question whether these rules should be in the core. The only other option would be in an higher layer (a delivery mechanism), but this would require me to duplicate this logic in every such mechanism, which we all now would end up in every god damned developer that wanders the repository to have its own view of how security should be implemented in the delivery mechanism he happens to be working *today*.

Even so, I do question where and how this logic should be activated. While I wouldn't have anything against it being activated at all times (1) I cannot stop anyone from reimplementing the configuration class that activates it and replace it with one that does not and (2) there are *some* cases where the convenience of having the security configuration disabled surpasses the risks, even if only temporarily.

<small>Truth be told, I don't quite get it either but it is one of the requirements...</small>

Here's what I came up with.

Regarding the how, I have to find a way to hijack the application context configuration and disable security before it is set up. That much I have solved already by placing an application context initializer that handles this. The problem now is that it must be controlled by a property in a configuration file, which have not been loaded by now. I seem to have found a way to perform this by hand but I do question whether this shouldn't be done by the actual application initializer in the final delivery mechanism, instead of the core itself.

Despite having the application context initializer in the core, the fact remains that it has to be called explicitly, either in a `web.xml` or another initializer. So should I really have all this in the core, or should I just follow an approach of preparation in the core, and let the outer layers handle this part of the configuration?

{% img center /images/security-fail.gif "Nobody is safe..." %}
