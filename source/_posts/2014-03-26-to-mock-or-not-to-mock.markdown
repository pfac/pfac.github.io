---
layout: post
title: "To Mock Or Not To Mock"
date: 2014-03-26 11:26
comments: true
published: false
categories: 
---

Unit tests must be isolated, acceptance tests must not, but both must be deterministic and independent of external dependencies. So which parts of the system are ok to fake?

<!-- more --> 

Unit tests have it easy. At any given time you are writing an unit test you are focusing on a particular test case for a particular functionality in a particular component. Any dependency must be faked to be working perfectly according to the assumptions for this implementation. This ensures a failure in one component does not affect another which depends on it, keeping unit tests simple and small.

On the other hand, **acceptance tests must use the entire system** in order to prove that each user story (or use case) works as expected. If dependencies exist, these are expected to be the real implementation, ensuring that the global behaviour is the correct one.

Yet, external dependencies introduce a critical question: **should we use them in tests, in order to ensure the system is tested like it will be used in production?**

The reasons for not doing so are well known. Take the database for example. Connecting to it introduces an unnecessary overhead to each test. Also, to ensure consistency we would have to reset the database after each test.

For a remote API wrapper the problem is even worse, since remote communication is even heavier than disk access and ensuring the consistency of data in an external system can even be impossible.

So, how do we do it?

## An example

Let's take the example of a video management web application, where the videos are streamed by an external service (like Brightcove). Let's implement the search functionality.

```gherkin
Feature: Search videos

  Scenario: List existing videos
    Given there is 1 video in service
    When "/videos.json" is accessed
    Then a JSON list with 1 element should be returned
```

## The architecture

Lately I've been studying software architectures and design patterns, with a particular interest for Uncle Bob's Clean Architecture. 
