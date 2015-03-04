---
layout: post
title: "Leaving the nest"
date: 2015-03-02 00:41:38 +0000
comments: true
categories: 
---

<!-- more -->

Two days ago, I officially left [VILT's Software Factory][sf-braga] team in Braga. I did so in order to chase a new challenge with another company, not because I wasn't well there. And after almost an year and a half, I feel I owe them all this post.

I arrived at VILT shortly after finishing my master's degree. It wasn't my initial plan, but the company where I was supposed to start had to make a last minute decision of absorbing another team. So out of the blue, I was applying for jobs at random companies. VILT ended up being my choice for a first job out of college.

I'm not ashamed to admit I was very afraid. I knew almost nothing about this team, but I knew VILT was the kind of company that liked to be associated with the word *enterprise*, and that's something I feared. After more than two years trashing those "sold souls" who became consultants in our field, I myself was to become one. And working with Java, of all things. Me, a C++/Ruby evangelist with a soft spot for Haskell and a weird aptitude for Javascript.

I was never bad at Java. I had to learn it while an under graduate, and truth be told I was damn good at it, but I never enjoyed it as much as C++. I did well during the job interview, but it dwelt in generic Java and generic web questions. My first day at VILT, I was in completely unknown waters. My first assignment after setting up my station was to help with a small bug in a JSP view. The only problem was I didn't even know what JSP is, let alone fix bugs in it. I had never done anything web related with Java. I was shocked to find out there was something in Java that reminded me so much of ERB.

First days went by, fixing markup, some scripts, eventually figuring out the logic of JSP core tags. In a short period my tickets went from fixing sections to fixing entire pages, to building them. Nothing fancy, but I got to show how much I knew of front-end development, and just how much it bored the hell out of me.

Then, out-of-the-blue, with no more than a few days at the office, I'm given completely different task: a bash shell utility to manage our servers in a particular client's setup. I was marvelled. My office managers saw I was getting bored and threw me a challenge more of my liking. I felt like hugging them.

This utility took longer than originally planned, but with a very good reason. They initially thought it would be a set of simple scripts, so they estimated for it. But I'm pedantic, anal even. So I built a tool suite using [Basecamp's sub project][sub], and spent a few days working out details like command line options and the sort. The result was a very easy to use tool, also very easy to extend, of which I'm still very proud.

Back to the frontend for a few weeks, and out-of-the-blue a new task. A strange request by the client had us develop a whole system to manage how scripts were to be loaded in their pages. They gave it to me since I knew my way around Javascript, and the previous task had gone well. And that became my first pure TDD project with Javascript, with little more than 100 lines of formatted code, fully tested (both unit and acceptance testing). I even placed a static page online with documentation working as a demo. *De puta madre*. This was what the manager of the project said when he saw it.

Fast forward a little more, and the same project manager asks me to work on a new task. A separate webapp to handle how media files are uploaded to the system. I end up learning [Struts 2][struts2], but deciding it is weaker than the Spring Framework, I change to [Spring 3.0][spring30] (compatibility issues). I learn Spring from the very basics, and I eventually start to map some of its concepts with [Uncle Bob's Clean Architecture][clean-arch]. I refactor the code a lot, but come up with some nice ideas for the architecture. I also spend a lot of time working on things like content negotiation. Eventually, the task is post-poned due to the delay.

A migration project starts, and it's a all hands on deck kind of thing. Months pass slowly, with the entire team figuring out issues with the new version everyday, and fighting to fix it the way the client expects. Too many front-end customizations, to the point where default menus have to be overwritten into oblivion. Yet, both me and another developer working on these customizations work on trying to do things the right way. The result, a paid trip to Madrid to commemorate.

Back with the delayed task. The version compatibility issue is removed from the equation, so I change to [Spring 3.2][spring32] (version 4 seemed hard to find documentation for). I add [Angular][angular], something I had never worked before too. My August is spent working on this, 8 hours straight from home everyday so I get to spend some hours with my girlfriend who is on vacation. The project is delivered, without any more delay in the middle of September.

Next, I'm moved to an enterily new project. A client I never dealt with before wants an effort analysis for migrating its system. Only it is a system I never worked with, and I don't really know that to do. Too many server reboots, data imports, long experiments together with misconfiguration and misinformation lead me to losing my focus. I spend days wondering what shall I do next. Errors are occurring, but the logs I know are consistently empty. I'm afraid of asking more questions, I'm starting to sound like a retard that can't hold the information he is being taught, always having more questions. The result: a light scolding, and my only not-so-good performance review. My manager says she expected better of me, I seemed like the guy who would dig under every single rock, but I didn't do that. It crushes me. I'm not even threatened with termination, but my sense of ethics tells me I deserve it. I worked longer hours after that, cut back all my distractions, and managed to find the bug. Better late than never.

After that *bad* episode, my managers decide I have the profile for working on products. According to them, my focus on code quality makes me a perfect candidate, so I'm moved to the engineering team. There, I start working on a new version of an already existing product, but I get to do it from scratch. New technologies ([Spring 4][spring4], [Spring Security][spring-security], [JPA][jpa], [Hibernate][hibernate], ...), new methodologies (automated tests, automated documentation, ...). Complete freedom. I sharpen my Spring skills, and I got so far into Spring Security that by the time I decided to leave the company, I purposed teaching what I learned and they agreed to it.

And so, I left. Not because I wasn't well. An offer was put on the table my another company, where, besides paying more, I may have the chance to become a team leader in a very short period of time. It's not so much the money (of course it matters too), but the idea of becoming a team leader by the age of 25 is very appealing. Normally, it would take me around 3 or 4 more years to reach that level. So, I decided to risk it, because I know I will not do it once I have a family to look after.

But, the consequence, sadly, is having to say goodbye.

First, to my managers Mário and Ana, definitely the most deserving of the warmest of goodbyes. The way they tried to read me, understand what I was enjoying and was not, and how they tried to always fit me into the projects that would matter the most to me never ceased to amaze me. I made sure I told them what an awesome job they were doing with the office (it's the newest in the company), and how they should just keep doing it the same. They will be the reason I will always wonder *how would things be different if I didn't*...

Second goodbye has to go to the entire team of the LaCaixa project. Like going to war, once you get there you bond over the scars you collect. But every single one in that team is amazing, even if in their own way.

Third, to the engineering team at SF Braga. Just the jokes alone were enough for me to love working on that project.

Lastly, to the rest of the team at SF Braga. I may have not worked directly with you, but bare in mind that my heart broke a little with every one I had to say goodbye.

You guys are truthfully, the best and funniest god damned team any geek can ask for their first job. Never stop being amazing.

Now, chin up, and on with my next challenge...



[angular]: https://angularjs.org/
[clean-arch]: http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html
[hibernate]: http://hibernate.org/
[jpa]: http://www.oracle.com/technetwork/java/javaee/tech/persistence-jsp-140049.html
[spring30]: http://docs.spring.io/spring/docs/3.0.x/spring-framework-reference/htmlsingle/
[spring32]: http://docs.spring.io/spring/docs/3.2.x/spring-framework-reference/htmlsingle/
[spring4]: http://docs.spring.io/spring/docs/4.1.x/spring-framework-reference/htmlsingle/
[spring-security]: http://docs.spring.io/autorepo/docs/spring-security/current/reference/htmlsingle/
[sub]: https://github.com/basecamp/sub
[struts2]: struts.apache.org/2.x/
[sf-braga]: http://www.vilt-group.com/en/services/software-factory
