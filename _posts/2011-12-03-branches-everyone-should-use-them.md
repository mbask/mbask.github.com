---
layout: post
title: "Branches: Everyone Should Use Them"
---

We now live in a world where distributed version control is king. Be it Git or
Mercurial (or something else). So today we're going to talk about Branching.
I've seen many different teams do many different branching strategies. All the
way from complex release cycle style branching, to refusing to branch full
stop.

Most of my examples are going to be using Git, however if you use something
different I am going to assume you have he capacity to research branching for
your DVCS of choice.

The one thing a branching strategy cannot replace is COMMUNICATION. It's very
important for a team to communicate, especially when branching for features.

## Branching like an old oak tree

We'll start at the end of the branching strategy scale where by you have a team
that uses a billion and one branches to manage their workflow. This is the
strategy I like to call **enterprise level branching**. You'll see a branch for
every deploy and you have no idea where `master` is up to. You could even see
branches for bug fixes and features.

The problem with this strategy is that it is visually very complicated. It's
very problematic to view the state of the code base between all the branches.
This branching strategy really doesn't scale and will only get more
complicated over time. If you're currently using a process that sounds some
what similar to this I highly urge you to continue reading and switch to the
more simpler strategy I will finally explain.

## Y U NO BRANCH?

At the complete opposite end of the spectrum we have the teams that just
completely omit branching at all. Everything they ever commit gets commited to
`master`. I can hear you all screaming now in pain of the strategy.

There are many reasons I have heard range from "branching is too hard" and
"long running branches will have too many conflicts". Both of which are fair
assessments.

Branching in Git is so cheap and easy to pick up, once you've learnt how to
you'll wonder why you've never done it before. If you're stuck on say something
like Subversion or CVS it's understandable why you have an adversion to
branching.

Long running branches are bound to happen, however it's so easy to keep them as
close to possible as `master` be merging changes from `master` into your
branch and keep it "up to date". Good communication comes in handy here if you
are working on long running feature branches.

## Three steps to branching bliss

So in essence the three steps are the following:

* Master
* Branch
* Merge back into Master

### Master

Your `master` branch is always deployable and if someone breaks a deploy from
`master` has broken the social contract that `master` is always in a deployable
state. So people will avoid merging shit into `master`. If you want to test
a branch you have have methods of deploying a branch to a staging environment
or subset of production servers.

### Branch

Everything you do is going to be on a separate branch, whether it's a small fix
or long running feature. It's always on a branch, if you have to ask it's on a
branch.

Make sure to keep your branch up to date with any changes which get merged into
`master` and deployed. The main reason a long running branch becomes a pain in
the ass to merge is that developers don't keep it up to date with changes from
`master`.

### Merge back into Master

When everyone is happy with the changes in the branch and people have reviewed
and signed off on the feature. You should now merge it back into `master`. If
you've kept it up to date with changes from `master` it shouldn't be that hard
to cleanly merge back into `master`.

## Pull requests, a code review godsend

If you're lucky enough to be using a GitHub organization account or GitHub
Enterprise you'll have the luxury of using the awesome pull request system. You
should use this as a code review system. It allows for everyone to comment and
give input on a pull request. You can use the pull request to communicate and
discuss the branch over a period, rather than once it's finished.

Pull requests are just awesome and simple for having a discussion over features
and even if you just end up not merging the feature, you've still learnt
something. Too many people have this thing about trying things and then when
it doesn't work out and delete it they think it's been wasted. It hasn't.

There seems to be this consensus that everything has to be done right and done
right the first time. This isn't true. There is the Thomas Edison quote about
failing.

> I have not failed. I've just found 10,000 ways that won't work.
> -- Thomas A. Edison

Branching allows you try ideas and throw them away if they don't work. If
you're not branching at all you will have a near impossible chance of trying
ideas and failing. Branching allows you to try these things and not affect your
`master` branch at all.

## Continuous integration, test all the branches

You should set up your continuous integration to build and test all your
feature branches. If you don't have a continuous integration server I suggest
you look into setting one up. Both Jenkins and TeamCity are free (TeamCity
having some limitations). It's very simple to get your continuous integration
server building new branches pushed up to `origin`.

I've also written a blog post of [integrating this with Hubot][hubot-ci] so if
you're interested in that feel free to [ping me on Twitter][twitter] or contact
me via the [ask me a question][ask] page.

## Wrapping up

Always branch. It's so easy that you shouldn't NOT be branching. If you're
stuck using a centralised version control system I highly urge you to invest
the time into switching to a distributed system.

### Some reference links

* [http://scottchacon.com/2011/08/31/github-flow.html](http://scottchacon.com/2011/08/31/github-flow.html)
* [http://zachholman.com/talk/how-github-uses-github-to-build-github](http://zachholman.com/talk/how-github-uses-github-to-build-github)
* [http://codeascraft.etsy.com/2011/12/02/moving-from-svn-to-git-in-1000-easy-steps/](http://codeascraft.etsy.com/2011/12/02/moving-from-svn-to-git-in-1000-easy-steps/)
* [http://cloud9ide.posterous.com/never-commit-to-your-master](http://cloud9ide.posterous.com/never-commit-to-your-master)

[hubot-ci]: /posts/hubot-ci-and-deploying/
[twitter]: http://twitter.com/thetombell
[ask]: /ask
