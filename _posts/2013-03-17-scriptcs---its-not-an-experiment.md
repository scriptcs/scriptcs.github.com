---
layout: post
title: "scriptcs"
category : scriptcs
tagline: "it's not an experiment"
tags : [scriptcs, project, status]
---
{% include JB/setup %}

Just a little over two weeks ago scriptcs started with [Glenn's blog post](http://codebetter.com/glennblock/2013/02/28/scriptcs-living-on-the-edge-in-c-without-a-project-on-the-wings-of-roslyn-and-nuget/). In that post he talked about a lightweight approach to authoring C# programs from outside Visual Studio in a text editor without needing a solution or a project. He posted the first preview of a tool called scriptcs which enabled the workflow leveraging [Nuget](http://nuget.org) and [Roslyn](http://msdn.microsoft.com/en-us/vstudio/roslyn.aspx).

The response from the community was **extremely** positive!!!

Almost immediately after that scriptcs moved from an experiment to a fully-fledged OSS project. It started from a Skype call which resulted in [Justin Rusbatch](https://twitter.com/jrusbatch) and [Filip W](https://twitter.com/filip_woj) joining as coordinators. 

Immediately after we created a [Github organization](https://github.com/scriptcs), a [website](http://scriptcs.net), a [twitter account](http://twitter.com/scriptcsnet), a [google group](https://groups.google.com/forum/?fromgroups#!forum/scriptcs) and a [jabbr chat](https://jabbr.net/#/rooms/scriptcs).  

Flash forward two weeks later. To be honest, things have been going forward at such **crazy pace**, that it really does feel like it's been fifty two weeks instead :-) 

Therefore, it is perhaps a good moment to look back at what has been so far.

However, before that, it is necessary to emphasize that the response from the community has been [absolutely staggering](https://github.com/scriptcs/scriptcs/issues/107), and has vastly exceeded any of our expectations! This is also a great testament to how needed a good, lightweight, C# scripting environment is.

## Scriptcs, owned by the community

This project has moved far beyond Glenn's initial experiments. It is now 100% owned by the community. All decisions are made publicly and transparently in our Github issues and you are more than welcome and encouraged to particpate!

As such, I think it's important to highlight that things have really changed, because some time ago, it would be virtually impossible to have a Microsoft person involved in coordinating such a project.

There has really been a tremendous amount of contribution from the community. GitHub has been literally lit up with notifications and Twitter has been beaming with activity too.

As of today, on Github, we have:

* 5 repos within the organization
* 17 unique [code contributors](https://github.com/scriptcs/scriptcs/wiki/Contributors)
* 226 stars
* 54 forks
* 138 issues
* 71 pull requests


## What's been added

As you can see since the last post we've had an incredible amount of energy. Here's a list of the new features that have been added with links for you to find out more.

* Script includes via [#load](https://github.com/scriptcs/scriptcs/wiki/Referencing-scripts) (contributed by [@filip_woj](https://twitter.com/filip_woj))
* A [sublime text plugin](https://github.com/scriptcs/scriptcs-sublime)! Better command line parsing and output (contributed by [@follesoe](https://twitter.com/follesoe))
* Proper nuget package binding via directly leveraging nuget.core (contributed by [@filip_woj](https://twitter.com/filip_woj))
* [Debugging](https://github.com/scriptcs/scriptcs/wiki/Debugging-C%23-Scripts) in Visual Studio! (contributed by [@dschenkelman](https://twitter.com/dschenkelman))
* Pluggable script engines (contributed by [@nberardi](https://twitter.com/nberardi))
* Script Packs, scriptcs extensiblity (contributed by [@gblock](https://twitter.com/gblock))
* Team city build, nuget packages and chocolatey (contributed by [@jrusbactch](https://twitter.com/jrusbatch))
* A ton of samples and a dedicated [samples repo](https://github.com/scriptcs/scriptcs-samples) (see more below)

## Samples

Thanks to a great help from community, we are adding more and more samples (which now have their own dedicate repository too). 

At the moment we already have:

* Web API self host
* ServiceStack self host
* Nancy self host (contributed by [@thecodejunkie](https://github.com/thecodejunkie))
* Fluent Automation (contributed by [@stirno](https://github.com/stirno))
* SignalR (contributed by [@jonhcoder](https://github.com/johncoder))
* RavenDB (contributed by [@khalidabuhakmeh](https://github.com/khalidabuhakmeh))
* WPF (with MVVM) (contributed by [@khellang](https://github.com/khellang))

If you wish to contribute a new sample just head over to the repo! We are always looking for examples, showcasing different uses cases of `scriptcs`.

## Packages feed

We are publishing our nightly builds to our [MyGet feed](www.myget.org/gallery/scriptcsnightly), which is available at

            http://www.myget.org/F/scriptcsnightly/

So you are strongly encouraged to try it out. The initial release is an `alpha` with version `0.3`. This includes:

* scriptcs v0.3.0-alpha (Chcolatey package)
* ScriptCs.Contracts v0.3.0-alpha (Extension point for script packs)
* ScriptCs.Core  v0.3.0-alpha (Core service)
* ScriptCs.Engine.Roslyn v0.3.0-alpha (Roslyn-based execution engine)

## Contributing to it's success, "you take it"
Scriptcs will only be succesful with the communities help. If you want to jump in and help out, we've put up a [contribution guideline document](https://github.com/scriptcs/scriptcs/blob/master/CONTRIBUTING.md) which has worked out great, both for the project and everyone wishing to contribute. We also introduced a new ["you take it"]([labeling system](https://github.com/scriptcs/scriptcs/issues/79) label for issues that we explicitly WANT you to take!

We also have a project logo, created by [Salman Quazi](https://twitter.com/splusq), and that has helped us develop an identity of the project.

Glenn has also been on dotNetRocks podcast to talk to Carl and Richard about `scriptcs`! Make sure to [grab the podcast](http://dotnetrocks.com/default.aspx?showNum=853).

## What's next?

There is a ton of exciting stuff ahead! We'll have a separate post about that.

## We want your feedback and your help!

We'd love to hear from you about any use cases that you might have for `scriptcs`!

To sum it all up - it's been really crazy two weeks, and exciting times are ahead! We can't wait to see where this goes!
