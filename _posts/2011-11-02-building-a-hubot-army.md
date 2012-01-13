---
layout: post
title: "Building a Hubot Army"
---

Last week [GitHub open sourced Hubot][github-hubot] to the public. Which came
a lot sooner than I think people expected. It was greatly received by the
public.

## What is Hubot?

GitHub built [Hubot][hubot] to help automate a lot of their daily tasks from
within their Campfire rooms. This includes stuff like deploying the site,
interact with their continuous integration server and a load more.

The original Hubot was bigger and a lot messier, so they decided to rewrite him
from scratch and open source him for everyone to use.

## Meet Hubot

The new and improved Hubot is still built on top of [Node.js][nodejs] and
[CoffeeScript][coffeescript]. Hubot was released in two parts; Hubot and Hubot
Scripts.

[github/hubot][hubot-repo]

This is the main Hubot library and binary. It originally started off with just
having adapters for Campfire, IRC and the terminal, but as of writing this post
there is around twelve adapters.

[github/hubot-scripts][hubotscripts-repo]

These are the scripts which power Hubot and help him perform his magic. This is
the project which was designed to allow the community to contribute scripts of
all shapes and sizes or just look for interesting scripts to add to your bot.

## Deconstructing Hubot

Hubot essential sits on your favourite adapter be that a chat room, IRC or
even Twilio (and many more adapters). Hubot sit happily silent until he is
addressed with a trigger or hears something he is interested in.

There are two ways to trigger Hubot to get busy and do stuff for you. He can
respond, or hear things said or sent to him. Let's use the example below to
outline this.

{% highlight coffeescript %}

robot.respond /what('s| is) the time/i, (msg) ->
  msg.send "The time is #{new Date().toTimeString()}"

robot.hear /(hello|hey|hi)/i, (msg) ->
  msg.send "Hello!"

{% endhighlight %}

The above examples are pretty simple and some what pointless but they'll do for
explaining how Hubot works. The first bit will be triggered when Hubot is
addressed by `hubot what is the time` or `hubot what's the time`. So when you
use `robot.respond` you're telling Hubot to respond when someone says
`hubot <pattern>`. The second part is simpler, and whenever Hubot sees `hello`,
`hey` or `hi` he will say "Hello!".

## Deploying Hubot

One of the great features of Hubot is that the guys made it really easy to
deploy to Heroku, thanks to their beta [Cedar stack][cedar].

You can run the `bin/hubot` with the `-c` option to create a deployable Hubot
for Heroku. Unfortunately quite a few people had issues and found the process
difficult or the `README` wasn't that great.

I saw this as an opportunity to build a web application to help solve the issue
and allow people to easily and quickly deploy their Hubots to Heroku.

## Introducing the Hubot Factory

You can [visit the Hubot Factory][hubotfactory] and deploy your own Hubot to
Heroku right now.

Hubot Factory is a very simple frontend which is just a Sinatra application
with Mustache templating. The really interesting stuff happens in a Resque
worker.

The Resque worker does all it's magic using basically a bunch of `system()`
calls in Ruby to `git`, `heroku` and `sed`. You can check it out in the
[Resque task][hubotworker].

The **real** magic is the ability to transfer applications on Heroku to other
Heroku users. So I push the application up to my account initially, add the
target user as a collaborator and transfer the application to them. Finally
removing myself as a collaborator.

You can [view the Hubot Factory source][hubotsource] and fork, edit, butcher
or even send pull requests to help make it better. The project is there to help
everyone get their Hubot running to <s>decrease</s> increase producvity whether
it's personal or work.

## Thanks

Most of the thanks should go to [GitHub][github] for open sourcing and making
Hubot available to the world. Specifically I'd like to thank
[@technoweenie][technoweenie] and [@atmos][atmos] for listening to my crazy
idea and offering some really good advice. Also for also providing me with
push and pull access to the main [Hubot repo][hubot-repo] to help maintain
Hubot and make him easier to use for everyone.

The biggest thanks goes to [Jesse Newland][jnewland] who upheld his promise of
giving the person to first get Hubot to talk to Siri, a pony. Unfortunately it
is not a real pony, but a model. So it's standing proud on my desk at work.

The biggest thing I've learnt from hacking and contributing to Hubot over the
last week and building Hubot Factory is that you should always take time out to
build that crazy idea or side project. You will never know how many find it
useful.

[github-hubot]: https://github.com/blog/968-say-hello-to-hubot
[hubot]: http://hubot.github.com
[nodejs]: http://nodejs.org
[coffeescript]: http://coffeescript.org
[hubot-repo]: https://github.com/github/hubot
[hubotscripts-repo]: https://github.com/github/hubot-scripts
[cedar]: http://devcenter.heroku.com/articles/cedar
[hubotfactory]: https://hubot-factory.herokuapp.com
[hubotworker]: https://github.com/tombell/hubot-factory/blob/master/lib/hubot_factory/build_hubot.rb
[hubotsource]: https://github.com/tombell/hubot-factory
[github]: https://github.com
[technoweenie]: http://twitter.com/technoweenie
[atmos]: http://twitter.com/atmos
[jnewland]: http://twitter.com/jnewland
