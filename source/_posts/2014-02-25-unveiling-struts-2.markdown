---
layout: post
title: "Unveiling Struts 2"
date: 2014-02-25 10:54
comments: true
published: false
categories: 
---

A simple file uploader using Struts 2.

<!-- more -->

As any proper Ruby and C/C++ programmer, I don't particularly like Java.
Yet, I'm paid to work on web projects, most of these using Java EE.

At the moment I am working on extending an existing web application, where
I need to handle a special case of uploading. Since some of the stuff is
already implemented using Struts 1.0 (_super deprecated_), I decided to
do my part using Struts 2.

Struts is a framework, much like Rails, but for Java EE. Like any good
framework, it works out of the box with minimal configuration if you respect
its conventions, but it also provides the ways to configure pretty much
anything (at least, as far as I needed).

Just a heads up before we begin comparing Struts 2 with Rails: Struts is not
RESTful by default, which means that using the same URL for two HTTP methods
will have to be handled by you manually. It's nothing complicated really, but
it is different.



## The boilerplate


### Creating the project

Yes, we all hate this part. If Java were any good, this part would be as easy
as `rails new`, but it isn't and some of us have to deal with it.

While I also vouch that [Maven](maven.apache.org) sucks (because it does
EVERYTHING), I have to use it in the client projects, and so I'll use it here.
It at least has the advantage of dealing with dependencies, so there is
something good in it.

Last, I use Eclipse. I also don't like it but it is completely open-source,
and is used for Android development so it may be useful to be proficient in
it.

#### Command Line

Creating the new project in the command line using Maven is not simple but it
is fairly easy:

```
mvn archetype:generate -B \
	-DgroupId=com.iampfac.struts2 \
	-DartifactId=s2fu \
	-DarchetypeGroupId=org.apache.struts \
	-DarchetypeArtifactId=struts2-archetype-blank \
	-DarchetypeVersion=2.3.16 \
	-DremoteRepositories=http://struts.apache.org
cd s2fu
mvn eclipse:eclipse
```

What is happening here? Step by step:

* `archetype:generate` is the maven command to generate a new project using
an archetype (a template)
* `-B`, or `--batch-mode`, runs maven in non-interactive mode. If we don't use
this, Maven will ask for more information to correctly configure the project.
* `-D`, or `--define` defines a system property. We use this flag several times
to set the information that otherwise Maven would ask for.
* `-DgroupId=<group_id>`; every Maven project has a grou


### `web.xml`

To hand over the control to Struts2, the deployment descriptor
must include a `filter` tag targeting a filter class
and a `filter-mapping` tag to map URLs to this new
filter.

Struts2 provides three filters:

<dl>
	<dd><code>StrutsExecuteFilter</code></dd>
	<dt>Executes the discovered request information.</dt>
	<dd><code>StrutsPrepareFilter</code></dd>
	<dt>Prepares the request for execution by a later
	`StrutsExecuteFilter` instance.</dt>
	<dd><code>StrutsPrepareAndExecuteFilter</code></dd>
	<dt>Handles both the preparation and the execution phases
	of the Struts dispatching process</dt>
</dl>

For now, let's use `StrutsPrepareAndExecuteFilter` so
we don't have to handle the two phases independently. Also, let's
use this filter for all URLs (no point in doing it any other way
for this particular project).


### `struts.xml`

By default, to serve a static file such as this one there is no
need to create any package, so this file should have remained
accessible even after restarting the server with the new
`web.xml`.

Now, to add a page so we can submit a new video. Let's make this
Rails style. Why? Because it makes sense to me and I can do whatever
I want since I'm writing this.

Continuing, let's add a new package to `struts.xml`.
Since we want to deal with all the resources under
`/video`, that must be the package namespace. We can also
use `video` as the package name (since it is a good
practice to use the same as the name and namespace). Finally, let's
make it extend `struts-default`, we won't need more than
this for now.

Inside this new package let's make an action to deal with all the
stuff behind submitting new videos. We shall name it
`new`, because we're hipsters.

Now, inside the new action, add a `result` tag
pointing to `/views/videos/new.jsp`.

Save, restart Tomcat and go!

Saw an error didn't ya? That's natural, and a sign it is all
working properly, since you never got to create that JSP file. Go
ahead and do that. Remember the path set in the action result:
`/views/videos/new.jsp`. You should now see your brand new
JSP file (just make sure you add something to it so you know it's the
correct one m'kay?).



## Data Go'Round

Neat, we have the very basics of a Struts 2 application. We have a
simple package, with a simple action, simply showing a JSP. Simple enough
isn't it?

Now let's upgrade this by actual changing something. We'll be adding a
form to `new.jsp` and making it send a message to the
system, which will then be used in another JSP.


### `new.jsp`

First things first, we need to add the Struts tags library to the
JSP so we can use all that sugar that comes with Struts in our views.
Let's go ahead and use `s` as the prefix (if you don't,
adapt what you are reading).

By adding the taglib, the tags `s:form`,
`s:textfield</code> and <code>s:submit` should be available
to build the form in an easier fashion. Go ahead and place them instead
of the usual HTML tags.

Now let's finish the form by adding the correct attributes to these
tags. For the form, add the attribute `action` with the
value `new`. For now, this will prevent an error due to an
unknown action and allow us to work with a REST-y API. Later we'll be
using the method of the HTTP request to differentiate between a call
to the form (GET) and the submission of data (POST).

For the text field tag, provide the `key` attribute,
where the value should be the name of the instance variable where you
will store that data (I'm going with `message`, duh). The
submit button tag does not require any attribute.

By now your page should have gotten a pretty ugly form that does
essentially nothing. Are you proud?


### `struts.xml`

Now to make some information go around. Head back to the
`struts.xml</code> file and add the attribute <code>class`
to our action, redirecting it to a new action class (I'm going with
`com.iampfac.struts2.s2fu.action.NewVideoAction`). Restart
your server.

"BOOM!", right? You are using a class that does not exist (yet), so
let's make it. Make it a simple class, nothing besides a Plain Old
Java Object with nothing on it. Wait for Tomcat to pick it up, and
refresh your form page.

"BOOM!, The Return". Ok, now what? You should be seeing a stacktrace
in your screen (if you don't, enable `devMode` in your
`struts.xml` file and restart the server). At the top of that
stacktrace you see a `NoSuchMethodException`, stating you
tried to invoke a method `execute()` on your shiny new action
class, but it does not exist. You did no such thing! Feel enraged! Feel
insulted! Curse your Tomcat and all it represents! Abandon all hope on
web development, get on a plane to Tibet and become a monk for the rest
of your life!

Calm down, please. Let's be civil about this. What happens here is
that Struts 2 expects the action class to implement its
`Action</code> interface, which declares an <code>execute()`
method, which you are not implementing. So, you see, it is really your
fault.

You can fix this in two ways. The first is, of course, by making your
class implement the `Action` interface, and then implement
the `execute()` method. But then again, you still don't know
what this method must do, and you are a lot lazier than me.

The other alternative is having your new class extend
`ActionSupport`. In fact, this was the class you were using
until a while back, because it is the class used by default for new
actions in Struts 2. Extending this class has the advantage that you
can keep using its execute method until you can come up with something
better, and it will provide you with some more goodies down the road.
So let's go ahead and extend it with your shiny new action class.

### `NewVideoAction.java`

Okaaay, a shiny new action class, everything working... and it does...
Nothing? Oh c'mon why bother with this? So much text and we're in the
same spot. It's like going around in some creepy woods made of text.

Hold your underwear. There is a reason for all this. I think.

Having an action class now we can have that fugly form of ours do
something useful for a change. Like sending its message to another JSP.
Yes, that's what it shall do, that's what is trully useful in the middle
of all this.

First, let's start by adding a new string instance variable to the
action class (using the same name we used in the `key`
attribute for the text field <a href="#new-jsp-i">before</a>). Remember
to respect the conventions: make it private and add a getter and a setter.
Struts 2 assumes these fields will be accessible through such methods.

Refresh your page aaaaand TADAH!

I know you don't see shit. But its there, I promise. Don't believe me
you ungrateful minion? Just add `${message}` anywhere in
`videos/new.jsp`, refresh and resubmit. In your face!



## Beam me up, Scotty

Next step here is using what we practiced for sending a message to the
action and extrapolating for files. In fact, it is pretty easy and requires
only a few changes.


### `videos/new.jsp`

First things first, we need our form to collect the files and send
them to the action. For this we add a file input field (using Struts 2
`file` tag of course), and give it the name of the instance
variable as the `key` attribute. Surprise, surprise, I'm
going with `file` (don't you just love these plot twists)?

That's almost it, we're just missing one thing. We need to set the
`enctype` attribute of the form to
`multipart/form-data`. The reason behind this is kind of an
advanced topic, but let's just say that, by default forms use
`application/x-www-form-urlencoded`, which means the data
is transmitted in a way similar to a query string in an URL, and files
simply cannot be transmitted that way.

Optionally, you may add `${file.name}` somewhere in this
file so you can see when the file is transmitted and the action comes
back. You don't have to, but you won't see anything different if you
don't do it.


### `NewVideoAction.java`

Well, I kinda wish I had something truly great to teach here, but the
truth is that you only need to know that the type to use for the new
instance variable is `java.io.File` (duh!). Add the variable,
the getter and the setter, and that's pretty much it. You have a file
reaching the action and getting all the way back to the JSP file.

Additionally, I can tell you that you can have two more fields for
the price of one. You see, your form is sending you a file, but the
Interceptor stack in Struts 2 is in fact giving more than just the plain
file. You can add the strings `fileFileName` and
`fileContentType` (with the respective getters and setters)
and the framework will take care of filling them automatically.

Congratulations, you have an uploader. A dumb one, but an uploader
nonetheless.


## Make a class out of it

A video is more than just a file isn't it? Well, I know it isn't, but where
I'm trying to get at is that you eventually may want some extra metadata to be
associated with your video, like, I don't know, categories, year, list of
featuring porn stars, and whatnot.

In the context of an MVC pattern, we're talking about a model. And since
Struts is an MVC web framework, we can relate with that. As you will surely
already know, MVC stands for Model-View-Controller. In the context of web
applications, this is usually implemented by having a fourth entity working
like a router. The router gets the request, depending on the URL it targets
a specific controller, which is generally associated with one or more models.
The controller uses the models to do its business related stuff, and when it
is done it renders the views with the resulting information.

In Struts, `struts.xml` defines the rules for the router, which
calls a specific action (the controller), and redirects to a given JSP file
(the view) depending on what happens in the action. The only piece missing here
is the model.

We are going to make a model out of the concept of a video. That way, instead
of having to add a variable for each piece of information to the action, we
can have a class handling those bits of information, and have the action
focusing on controller stuff.


### `Video.java`

The model part is fairly simple. Just create a plain new Java class, and
move the message and file variables from the action (along with the getters
and the setters).

That's it, we're done here. On to the next file.


### `NewVideoAction.java`

The changes here also minimal, but not so simple.

First you should have removed the message and file instance variables, and
you should replace it with a single variable for a Video.

Now comes the strange part: Struts works with something called a
__value stack__, which it uses to access the values from the JSPs
for example. I won't deal into much detail on it for now, but we now have to
tell Struts that it should access the variables of the model automatically
(both to set and get them).

We do this by making our action class implement the interface
`ModelDriven</code>. This interface defines <code>getModel()`,
which Struts uses to place the model in the value stack. That way, when you it
searches for a `message` property, it will look for it inside the
model first.

That's all. Refresh the page and submit, it should work perfectly.
