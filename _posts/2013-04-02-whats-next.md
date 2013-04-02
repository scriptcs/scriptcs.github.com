---
layout: post
title: "What's Next"
tagline: "Improving the C# scripting experience."
category: scriptcs
tags: [scriptcs, project, status]
---
{% include JB/setup %}

As promised in the [previous post](http://blog.scriptcs.net/2013/03/17/scriptcs---its-not-an-experiment) we'd like to share with some plans for the near (and more distant future).

There is definitely a ton of exciting stuff ahead! 

It's important to emphasize, that this project is owned by the community, and as such, the roadmap is also shaped by the community! After all, we want to make the C# scripting experience as smooth as possible for everyone.

Here are the major upcoming features.

### Mono support

Thanks to the amazing work of [Dale Ragan](https://github.com/dragan), we are closing in on Mono support. You can follow [his fork](https://github.com/dragan/scriptcs/tree/mono-support) to see the progress first-hand, and you can join the [discussion](https://github.com/scriptcs/scriptcs/issues/80) on Github.

### Nuget Package Installation

This has actually just made its way to the scriptcs [dev branch](https://github.com/scriptcs/scriptcs/tree/dev). Since `nuget.exe` does not resolve nested dependencies when restoring packages from a `packages.config`, we have introduced [Nuget installation](https://github.com/scriptcs/scriptcs/pull/131) built-in to scriptcs itself.

To install your NuGet packages along with their dependencies, you can simply run the following command in the same directory as your `packages.config` file:

     scriptcs -install
 
If there is anything else you'd like to see in this particular idea, let us now!

### Extensibility (Script Packs)

Script Packs provide a way for 3rd parties to plug in their own framework and remove a lot of cruft for developers using them.

The entire extensibility model is designed around three core ideas:

 - Reference additional assemblies
 - Importing `using` statements
 - Surfacing globally-scoped objects to the script

While the extensibility model for scriptcs is pretty much complete, we are working on creating samples to showcase its usage.  We plan to have an ASP.NET Web API sample out there very soon, as well as documentation on how to create your own script packs.

We think script packs will be a great addition for the community and we hope to see packs for all the popular third party frameworks i.e. Nancy, ServiceStack, Caliburn, etc. 

### Startup performance

Currently we perform a considerable amount of file IO when script gets executed. While for smaller scripts and low number of NuGet packages, it's mostly not visible, it may become annoying for larger script-based solutions or applications.

We are now working on a set of performance improvements in that area. As always, any feedback is expected - the discussion is going on [here](https://github.com/scriptcs/scriptcs/issues/143).

### Exports

We have started laying ground for a project that would allow you to [create a Visual Studio solution](https://github.com/scriptcs/scripts-export) directlz from your scriptcs script. In other words, you can start working on a script-based project, and if one day you decide it's time to move to Visual Studio, this tool will facilitate it.

We are also working on allowing an export of your script to an executable (`*.exe`) which will allow you to share the script with people who wouldn't have scriptcs installed.

### scriptcs clean

If you want to share or move around your scriptcs project, all you really need are `*.csx` files and `packages.config` (and perhaps any custom non-NuGet assemblies if you use such). 

However Nuget packages and `bin` folders are known to often weigh tens of megabytes. To improve this, we are introducing a clean command:

            scriptcs -clean

which will clean up the application folder of unnecessary files for you, making it easier to share/move your scriptcs. You can join the discussion on the topic [here](https://github.com/scriptcs/scriptcs/issues/142).

### Exception handling/logging

As we are growing more mature, we (with help from [@dschenkelman](https://github.com/scriptcs/scriptcs/pull/135)) are working on a better logging/tracing experience for `scriptcs`, as well as on a good error propagation model. 

### Lightweight configuration/opts

We are discussing several ways of a possible configuration model for scriptcs. This includes both the configuration of the scriptcs application itself, and the `opts` file to act as a shortcut to command line commands.

On a side note, we have already embraced [PowerArgs](https://github.com/adamabdelhamed/PowerArgs) for a smooth command line experience.

### Unit tests

There have been a few requests about unit testing your C# scripts. We are investigating several potential solutions of allowing you to write and execute unit tests for `*.csx` files.

Stay tuned!

### Tutorials & Distribution

We already have the nightly packages available at MyGet

  http://myget.org/gallery/scriptcsnightly

however, we are closing in on a first (pre-release) NuGet drop! As part of that, we are planning on dedicating more time to documentation and tutorials, as these items come up often in Twitter conversations!

### Going beyond CLI

We have `ScriptCs.Core` which can be used to build applications that host scriptcs outside of the command line. 

We plan to make sure the API is well-documented, clean, and understandable, as well as, at some point, introduce some samples of consuming `ScriptCs.Core`.

### Octopus Deploy support

<img height="75" width="75" src="https://twimg0-a.akamaihd.net/profile_images/2357521476/Avatar.png" alt="Drawing" style="float:left; margin:0px 10px 10px 0"/> Paul Stovell, the creator of Octopus Deploy has [just announced](http://octopusdeploy.com/blog/octopus-1.5-azure-ftp-scriptcs) that Octopus Deploy 1.5 supports scriptcs! 

This is absolutely terrific news, and we'd like to thank Paul (who also [contributed](https://github.com/scriptcs/scriptcs/pull/139) to scriptcs source) for being the first to officially embrace our platform! 

<div style="clear:both"></div>

### Further scriptcs adoption

We have some more folks interested in adopting scriptcs in their solutions/tools. We think this is great and are really excited to see how it turns out!

Hopefully many other will follow!
