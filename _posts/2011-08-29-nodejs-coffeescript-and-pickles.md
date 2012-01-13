---
layout: post
title: Node.js, CoffeeScript and Pickles
---

I've been looking for an excuse to write some [CoffeeScript][coffeescript] and [Node.js][nodejs]. Currently I'm working on a new project with [@martinrue][martinrue] and having read [why GitHub hacks on side projects][githubside], I was inspired to create my own [Hubot][hubot]. Enter [Pickles][pickles]. Pickles is an IRC bot at the minute who responds to some pretty rudimentary commands like `image me {phrase}`, `weather me {location}` and `commit me {user}/{proj}`.

Building Pickles was my first time working with CoffeeScript and Node.js, it took some refactoring of code to get used to writing idiomatic CoffeeScript and Node.js code. I am beginning to like the idea of functions taking callbacks, being able to leave out `(` and `)` and being asynchronous. Pickles is an IRC bot at the minute, and I have a [Campfire][campfire] version in the works.

## Setting Up Node.js

Getting Node.js up and running was fairly simple. It was basically cloning the GitHub repository and then `configure`, `make` and `make install`. If you've built Node.js and added the directory to your path when you run `node -v` you should get the version of Node.js you are running.

Once Node.js has been set up you will want to install the Node.js package manager [npm][npm]. Installing `npm` is as easy as running `curl http://npmjs.org/install.sh | sh`. If everything went well you should be able to run `npm` and get some help output.

## Building Pickles

Pickles uses the [node-irc][nodeirc] library for handling the IRC protocol. Some of the bot code was inspired from [@defunkt][defunkt]'s [evilbot][evilbot]. So Pickles has some helper methods for specifying regular expression patterns for matching commands and providing a callback for handling those commands.

{% highlight coffeescript %}

hear = (pattern, callback) ->
  handlers.push [ pattern, callback ]

{% endhighlight %}

As you can see this is a fairly succinct function declaration that simple adds a regular expression and callback to an array of handlers. The equivalent javascript would look something like the following. 

{% highlight javascript %}

function hear (pattern, callback) {
  handlers.push([ pattern, callback ]);
}

{% endhighlight %}

I've found that I enjoy writing CoffeeScript more than normal javascript now purely because it looks much nicer and a lot of the punctation can be left out.

A lot of the commands that Pickles supports involve making a HTTP request to a web API. The `HTTP` and `HTTPS` modules provided by Node.js make doing this extremely easy. Take for example the `commit me` command of Pickles. This command calls the GitHub API for a list of commits on a given project. Pickles uses the `https.request` function to make the request to the API and then attaches event handlers to the `data` and `end` events on the `response` object for handling the response the API sends back.

## Hacking On My Side Project

Taking ideas from [Hubot][hubot] I would like to add similar functionality to [Pickles][pickles] for the experience of working with APIs, Node.js and CoffeeScript. Additionally I would like to make Pickles use Campfire because it provides a way to display images and transcript logs for the day.

If you're interested in getting into Node.js and CoffeeScript development a few things that I recommend for learning the technologies include the following:

* PeepCode: [Meet Node.js][nodejspeepcode]
* PeepCode: [Meet CoffeeScript][coffeescriptpeepcode]
* The Pragmatic Bookshelf: [CoffeeScript: Accelerated JavaScript Development][coffeescriptpragprog]
* GoRuCo 2011: [CoffeeScript for the Well-Rounded Rubyist][coffeescriptgoruco]

If you're interested in hacking on your own side project, please [fork and hack on Pickles][pickles]. If you feel that anything you've added would benefit Pickles please submit a pull request and hopefully we can add it to Pickles.

[coffeescript]: http://coffeescript.org
[nodejs]: http://nodejs.org
[martinrue]: http://twitter.com/martinrue
[githubside]: http://zachholman.com/posts/why-github-hacks-on-side-projects/
[hubot]: http://hubot.github.com/
[pickles]: https://github.com/tombell/pickles
[campfire]: http://campfirenow.com
[npm]: http://npmjs.org
[nodeirc]: https://github.com/martynsmith/node-irc
[defunkt]: http://twitter.com/defunkt
[evilbot]: https://github.com/defunkt/evilbot
[nodejspeepcode]: http://peepcode.com/products/nodejs-i
[coffeescriptpeepcode]: http://peepcode.com/products/coffeescript
[coffeescriptpragprog]: http://pragprog.com/book/tbcoffee/coffeescript
[coffeescriptgoruco]: http://www.vimeo.com/27200146
