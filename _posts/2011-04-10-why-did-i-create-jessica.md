---
layout: post
title: Why Did I Create Jessica?
snip: Walking the red carpet in style
---

A [question I received][question] made me think about the reasons behind working on [Jessica][jessicafx] and not contributing to [Nancy][nancyfx]. My first ever time seeing a *micro web framework* was the time I saw [Sinatra][sinatra]. Sinatra provides a [DSL][dsl] that has a certain beauty about it. I have seen other *micro web frameworks* try to create the Sinatra experience in other languages ranging from [PHP][limonade] to [Python][flask].

It wasn't until about six months ago I noticed the .NET community start to create *micro web frameworks*. I looked at what the community had to offer, and nothing I came across gave off the same radiating beauty that Sinatra gives off. The closest clone that was some what comparable to Sinatra was Nancy.

## What is a Micro Web Framework?

There are plenty of interpretations of the term *micro web framework* floating around the Internet, here's mine:

* Framework for creating web applications that only provides a minimum feature set
* Provides the least required set of helpers
* Uses a simple pattern for creating actions/routes for the application
* Nonessential features provided as third-party extensions.

One of the main appeals of [Sinatra][sinatra] is [Rack][rack]. This means that a Sinatra application can run on any Rack supported web server. Unfortunately in the .NET world we do no have anything that we could describe as being synonymous with Rack. However the future is looking bright because of efforts going into the [OWIN][owin] project.

Currently a .NET web application can run on IIS, IIS Express or Mono's XSP web server which all support ASP.NET. Jessica makes use of this existing support for ASP.NET and doesn't attempt to abstract away from it, allowing its implementation to stay true to the core goal of simplicity.

Nancy has abstracted away from ASP.NET, `System.Web` and `System.Web.Routing`, allowing users to not only host web applications on ASP.NET, but also self-hosted, WCF, and OWIN. This also makes it compatible with the .NET client profile. This means they have rewritten what ASP.NET is already providing and thus moved away from the *micro web framework* definition.

* Framework for creating web applications that only provides a minimum feature set

Rack is providing Sinatra with the HTTP and routing features it requires, and ASP.NET is providing the equivalent for .NET. A *micro web framework* abstracting away from existing functionality that is widely available on the web servers you will be hosting on, is only adding complexity to your framework.

Nancy has taken an approach to making many parts of the framework replaceable; if you don't like the functionality of X you can create your own, and have Nancy use that instead. While this is a respectable goal for a web framework, the focus for Jessica has been to stay as simple and as close to Sinatra as possible.

## How Does Jessica Stand Out From the Crowd?

The main goal of Jessica is simply:

* Provide a simplistic approach to creating a web application with a minimum amount of friction

Jessica is aiming to fulfil what Sinatra fulfilled for Ruby. A simple framework for creating web applications without any fuss. This includes not providing any features for the sake of providing a feature. ASP.NET and it's routing allows Jessica to easily register user defined routes and associate an anonymous function to execute for the route. So keeping within the goal of simplicity, Jessica builds on ASP.NET  and not implementing existing functionality.

Currently Jessica only provides one main point of extension for third-parties; the factory which creates the instance of your `JessModule` (and creating instances of autoloaded view engines). You can then create your own implementation of `IJessicaFactory` for your favourite dependency injection framework. The would allow you to inject dependencies into your modules constructor. I believed this is the only major extensibility point Jessica really needs to care about.

The way Jessica handles view engines now is similar to Nancy's implementation, however it now seems that this method is slightly more complicated than it could be, and I believe could be made a lot more simpler, and may end up being rewritten in the next few days.

Essentially I would like Jessica to become a feature equivalent of Sinatra for .NET, without heading in a similar direction that Nancy has taken. Jessica should have *simple configuration*, *simple conventions*, and *simple code*.

I have nothing but praise for the guys working on Nancy and itself as a web framework, I just believe it has outgrown the definition of a micro web framework. Thus Jessica was born.

[question]: http://twitter.com/#!/TheCodeJunkie/status/55983124449992704
[jessicafx]: http://jessicafx.org
[nancyfx]: http://nancyfx.org
[sinatra]: http://sinatrarb.com
[dsl]: http://en.wikipedia.org/wiki/Domain-specific_language
[limonade]: http://www.limonade-php.net
[flask]: http://flask.pocoo.org
[rack]: http://rack.rubyforge.org
[owin]: http://owin.org
