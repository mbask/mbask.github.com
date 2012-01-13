---
layout: post
title: "Hubot: Continuous Integration and Deploying"
---

If you have [read my previous Hubot post][blogpost] you will know I have an
interest in making Hubot do some funky shit. I happen to also help maintain
him. I'm in the fortunate position of being able to use Hubot and
[Campfire][campfire] at work with the other developers within our team.

When you first show off Campfire to other people they think "how is this
useful?" and "isn't this just another chat room?". Then when you add a "chat
robot" into the equation it can be quite tough explaining its usefulness to
non-techies. However that wasn't an issue for our team, however some people
where ultimately skeptical about the usage.

## Hubot Meet the Team, Team Meet Hubot

When I first introduced our team to Hubot they were instantly using the "fun"
commands for Hubot such as "image me" and "Y U NO BE PRODUCTIVE?". So initially
people felt it wouldn't lead to much productivity. But low and behold, I
preempted this and began hacking together scripts so Hubot could talk with our
continuous integration server and deployment.

One of the infamous things GitHub has Hubot do is run builds on their CI
server, they use Jekins and also are able to deploy their applications <s>TO
THE CLOUD</s> to their servers. They use use the top sekret sauce in between
Hubot and their Capistrano deployments to do this (if I disclosed anymore
information about this their Hubot would hunt me down and silence me).

I wanted to add similar functionality to our Hubot at work, so I began hacking
and making shit happen (like I always seem to do). The `hubot-scripts` package
ships with a `teamcity.coffee` (TeamCity being the CI server of choice here)
which I used as a starting point. Out of the box it is pretty limited only
showing the latest three builds, but I didn't let this stop my awesome skills
at hacking on some more functionality.

## TeamCity RESTless API

TeamCity comes with a "REST" API plugin, I put "REST" in quotes because it's as
restful as a hyperactive child who's forgotten to take his ritalin and necked a
tube of Smarties. What it really is a few crafted URLs you can send HTTP
requests to, to retrieve various build information.

## TeamCity HTTP POST Hooks

Given we're on version what? 6.5 of TeamCity and it **still** doesn't have
built in HTTP POST hooks for sending a HTTP POST request with a payload to a
configured URL when a build starts and finished is beyond me. So I searched
high and low for a plugin, however Google makes this pretty trivial and found
the [tcWebHooks plugin][tcwebhooks]. Install this plugin, restart your TeamCity
server and **BOOM** you now have web hooks.

## Hubot Listen!

So I can hear you all asking, "but Tom Hubot only gives you a way to respond to
and listen to messages!". Hubot is built on [Node.js][nodejs] which is evented
so we can be cool and also have Hubot spin up a HTTP server to listen for
incoming HTTP requests like the boss he is.

{% highlight coffeescript %}

http = require "http"

module.exports = (robot) ->
  server = http.createServer (req, res) ->
    if req.url is "/build"
      data = ""
      req.setEncoding "utf8"

      req.on "data", (chunk) ->
        data += chunk

      req.on "end", ->
        body = JSON.parse data
        # ...
  server.listen 9292, "0.0.0.0"

{% endhighlight %}

The above code makes some pretty bold assumptions like your favourite meat is
turkey and you love drinking shots off the tummy of college girls. It's the
first iteration of my hacks, and it's purely written to **get shit working**.
Once I can confirm it's bullet proof, I can go and refactor it into much nicer
sparkly code.

Now we have Hubot listening and waiting for requests we can configure TeamCity
to throw HTTP requests at Hubot and make him do the hard work. If you've
installed the tcWebHooks plugin correctly you should see a "Web Hooks" tab when
you click on a project or build configuration. You can find further detailed
instructions on the tcWebHooks documentation page.

You'll want to put in the IP or hostname of the server Hubot is running on
including the port and remember to add `/build` to the end this will act as a
psuedo-namespace incase you want to add other points for Hubot to listen on
(like funky GitHub post receive HTTP POST hooks).

## Incoooooming... JSON

It's best to have the payload come in as JSON so we can just parse it with
`JSON.parse` which doesn't require loading third-party stuff. Luckily the JSON
sent is pretty simple to understand and find the properties you're looking for.

{% highlight coffeescript %}

parseBuildPayload = (data, callback) ->
  buildStatus = data.build.buildStatus
  buildResult = data.build.buildResult
  buildNum    = data.build.buildNumber
  buildName   = data.build.buildName
  projName    = data.build.projectName

  msg = switch buildResult
    when "running"
      "started"
    when "success"
      "successful (#{buildStatus})"
    when "failure"
      if buildStatus is "Will Fail"
        "will fail, please check configuration"
      else
        "failed (#{buildStatus})"

  callback "Build #{buildNum} of #{projName}/#{buildName} #{msg}"

{% endhighlight %}

So you can see I just construct a pretty status message that Hubot will send to
the room. `buildStatus` contains the status of the build for example number of
passing/failing tests. `buildResult` is the result of the build and the final
three properties are pretty self documenting.

So now you pretty much understand how to get Hubot showing build statuses when
they're triggered on your CI server. I am still hacking away at letting our
Hubot respond to `ci status :project/:build` and `ci build :project/:build`.
This will probably involve having to store a map of build type IDs to project
and build configuration names (damn you TeamCity).

## Ship Early, Ship Often, Ship Easy

We're primarily a .NET development team, so we can use Web Deployment Projects
in our solutions for web applications to make our deployment easier. The new
MS Deploy stuff is just so bad I'd rather manually copy and paste than use it.

So I created a couple of build configurations on TeamCity called "deploy to
staging" and "deploy to production". The last build step of these is a
PowerShell script step. This basically does the following:

1. Backup the web site root to another directory
2. Copy the new web site to the web site root
3. Copy the `Web.config` for the production/staging site

So now our "deployments" are configured we need to tell Hubot to trigger those
build configurations. This is done by sending a HTTP request to
`/httpAuth/action.html?add2Queue=:buildTypeId` where `:buildTypeId` is the
build type of the build configuration you want to trigger. We currently hard
code these values into our scripts. This might be changed once I figure out a
better way to handle it (I'm looking at you `redis-brain.coffee`).

{% highlight coffeescript%}

robot.respond /deploy (\w+) to (production|staging)/i, (msg) ->
    user = process.env.HUBOT_TEAMCITY_USERNAME
    pass = process.env.HUBOT_TEAMCITY_PASSWORD
    host = process.env.HUBOT_TEAMCITY_HOSTNAME

    build = switch msg.match[2]
      when "production"
        "bt4"
      when "staging"
        "bt6"

    msg.http("http://#{host}/httpAuth/action.html?add2Queue=#{build}")
      .headers(Authorization: "Basic #{new Buffer("#{user}:#{pass}").toString("base64")}", Accept: "application/json")
      .get() (err, res, body) ->
        msg.send "TeamCity says: #{err}" if err?

{% endhighlight %}

We use the `scoped-http-client` available via the `msg` to make the HTTP
request also authenticating with basic authentication. The only output is the
error message if anything doesn't work. The listening HTTP server will
receive any build status notifications because our deployments are build
configurations in TeamCity.

## Wrapping Up

So even though the code is pretty unstable and hacky you can get the gist of
the direction I am heading in. Hopefully once the code is more stable and
configurable I will release something into the wild everyone can sink their
teeth into with their Hubot.

If you have any questions regarding the things discussed here, or Hubot in
general please contact me using the email on the [ask][askme] page.

### Some references links

* [http://confluence.jetbrains.net/display/TW/REST+API+Plugin](http://confluence.jetbrains.net/display/TW/REST+API+Plugin)
* [http://confluence.jetbrains.net/display/TCD65/Accessing+Server+by+HTTP](http://confluence.jetbrains.net/display/TCD65/Accessing+Server+by+HTTP)

[blogpost]: /posts/building-a-hubot-army/
[campfire]: http://campfirenow.com
[tcwebhooks]: http://tcplugins.sourceforge.net/info/tcWebHooks
[nodejs]: http://nodejs.org
[askme]: /ask
