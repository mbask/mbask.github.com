---
layout: post
title: "Zero to Viral in 24 Hours"
---

One Thursday evening after work I was settling down to work on some presentation slides and was looking for a unique title for my talk. The talk is about [CoffeeScript][coffeescript] and I tweeted that I was looking for a title. One reply to this tweet would set the wheels in motion of a rollercoaster of a ride. When I say rollercoaster, it was more like dodge 'ems at a really bad fair.

<blockquote>
  <p>Caius: @tombell trollscript</p>
</blockquote>

After seeing this reply the challenge was accepted (I exclaimed *challenge accepted* just like our favourite friend Barney Stinson, okay maybe not). I shall not name my talk trollscript, I shall invent a language called trollscript.

## Challenge Accepted

I decided I might as well just create a derivative of [Brainf\*ck][brainfuck]. Instead of the operators: `<`, `>`, `+`, `-`, `.`, `,`, `[` and `]` I would implement operators that essentially formed a string that would look something like the following.

{% highlight console %}
trolloloolllooollololoooollololooll.
{% endhighlight %}

Initially I thought I could get away with combinations of `o` and `l` being up to 2 characters. Unfortunately this only allowed me to have 6 operators. Yes 6, not the 8 that Brainf\*ck has. A friend has sent me a message saying I should use [trigraphs][trigraphs]. This method easily gave me enough operators.

## TrollScript, Because Brainf\*ck is so Last Year? 

As trollscript is just an alternative set of operators for Brainf\*ck I thought I might as well seek out some Ruby-based interpreter for Brainf\*ck and hack the hell out of that to run trollscript. The implementation I found that I was going to hack on was [this gist][gist] I found from Googling. The 8 eight operators for trollscript are shown below with the matching Brainf\*ck operator.

* `ooo` is `>`
* `ool` is `<`
* `olo` is `+`
* `oll` is `-`
* `loo` is `.`
* `lol` is `,`
* `llo` is `[`
* `lll` is `]`

## Hacking the Interpreter

The interpreter essentially is a huge hack. Your trollscript source file cannot contain anything but the operators. Oh and it can optionally start and end with `tro` and `ll.` respectively. The interpreter reads in the entire source file followed by stripping out all whitespace, then iterating over every 3 characters. It then executes a different lambda set up for each operator so it can do some magic stuff. I'm not going to bore you with any more details about the implementation. Feel free to ping [@thetombell][thetombell] on Twitter if you have any questions.

## Trolling Hacker News and Reddit

I commited the source with a `README` to a [repository][repo] on GitHub and showed the people in the #nwrug channel on FreeNode. The next thing I knew [Will Jessop][will_j] had submitted the repository as a [story to Hacker News][hn]. The story slowly spiralled into a larger snowball, with Will also submitting it [to Reddit][reddit]. An interesting thing to note about both submissions is that the comments I saw on Reddit were the ones I would have expected to read on Hacker News and vice versa.

News started to travel fast across the Internet about trollscript. The repository slowly gained more and more watchers and some forks. I went to sleep that night with the story sitting comfortably at #3 on Hacker News.

![Hacker News Story](/imgs/posts/2011-09-12/hackernews.png)

## Trending TrollScript

The following day at work I told some people about trollscript and they had a good laugh about it. I kept checking up on the Hacker News story throughout the day. It remained on the frontpage for pretty much the remainder of the day. 
Settling on 104 points and around 40 comments.

I also check the [explore page on GitHub][github] on a daily basis, and this day I had to double take to see if what I thought I saw was correct. Apparently trollscript was a trending repository for the day.

![GitHub Trending Repositories](/imgs/posts/2011-09-12/github.png)

## TrollScript Interpreter, the new Todo List

The repository is now sitting at 73 watchers and 13 forks. During the day I saw that one of the forks had converted the interpreter into one that ran on [Node.js][nodejs]. Through the day and following days I began to see a lot of different implementations of the trollscript interpreter pop up.

* **Official** Ruby implementation
* Two JavaScript Node.js implementations
* Scheme Brainf\*ck interpreter with trollscript support
* LOLCODE implementation
* Clojure implementation

If you're feeling adventureous feel free to implement a trollscript interpreter in your favourite language of choice. Bonus points if you have the stamina to write one in trollscript itself.

## The Ultimate TrollScript Script

On Saturday evening I was greeted in my email inbox by a pull request for the trollscript repository. It was for one more example trollscript to add to the examples folder. I shall leave you with a screenshot of the script in action.

![TrolLScript ASCII Art](/imgs/posts/2011-09-12/trollface.png)

Never let a bit of harmless programming fun get in the way of your serious work. All work and no play can become boring. Have fun.

[coffeescript]: http://coffeescript.org
[brainfuck]: http://en.wikipedia.org/wiki/Brainfuck
[trigraphs]: http://en.wikipedia.org/wiki/Trigraph
[gist]: https://gist.github.com/69910
[thetombell]: http://twitter.com/thetombell
[repo]: https://github.com/tombell/trollscript
[will_j]: http://twitter.com/will_j
[hn]: http://news.ycombinator.com/item?id=2975898
[reddit]: http://www.reddit.com/r/programming/comments/k9cbj/trollscript_an_esoteric_dialect_of_brainfuck/
[github]: https://github.com/explore
[nodejs]: http://nodejs.org
