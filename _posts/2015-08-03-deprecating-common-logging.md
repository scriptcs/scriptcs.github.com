---
layout: post
title: "Deprecating Common.Logging"
tagline: "The first step in removing Common.Logging"
category: Releases
tags: [changes]
---
{% include JB/setup %}

We decided some time ago to remove [Common.Logging](https://github.com/net-commons/common-logging) as a dependency from scriptcs. In scriptcs 0.15 we've taken the first step in this process by introducing our own logging API based on [LibLog](https://github.com/damianh/LibLog) and deprecating the use of Common.Logging.

Why are we doing this? Common.Logging has been a pain point for some time now for anyone who is using scriptcs in a hosting scenario or building their own NuGet packages based on scriptcs. Package version mismatches/binding redirect tangles, breaking changes in the 2.x version range (either accidental or deliberate) have been plaguing us for a while so we decided it was time to remove the problem once and for all. scriptcs.exe itself also ran into problems when we discovered [Common.Logging 2.2.0 is not compatible with Mono on Linux](https://github.com/scriptcs/scriptcs/issues/670).

Rather than remove Common.Logging right away, we decided to introduce our new logging API alongside Common.Logging, with [Obsolete attributes](https://msdn.microsoft.com/en-us/library/system.obsoleteattribute(v=vs.110).aspx) added to communicate the deprecation. This way, you can upgrade to your projects to our 0.15 NuGet packages and keep them using without any immediately necessary changes.

Unfortunately, as is often the case with such things, the upgrade path isn't *quite* so simple. We've done our best to ensure backwards API compatibility between our 0.14.1 and 0.15.0 packages. This means that you should be able to switch out 0.14.1 binaries for 0.15.0 binaries for use by your compiled assemblies and everything should continue to work. However, if you are expecting scriptcs to inject a `Common.Logging.ILog` into your assembly via MEF, this will no longer work, since scriptcs only has the new `ScriptCs.Contracts.ILog` type registered. Instead, take a dependency on `ScriptCs.Contracts.ILogProvider`. Also, if your source code refers to `Common.Logging.ILog` as just `ILog with `using Common.Logging;` *and* you have `using ScriptCs.Contracts`, you will receive a compiler exception because `ILog` is now ambiguous. This should require a very simple fix to your source code to get things compiling again. We decided to treat this as a non-breaking change, since `using` statements are syntactic sugar designed to reduce verbosity in source code and are always susceptible to the introduction of a type with the same name as a type in another namespace, whereas backwards compatibility for compiled assemblies, which will always reference types unambiguously, has been preserved.

If you're hosting or maintaining your own NuGet package and using scriptcs 0.14.1 or lower, upgrade to 0.15.0 today and start switching over to our new logging API. We will be removing Common.Logging from scriptcs entirely in a forthcoming release (exactly when is not yet decided) so the sooner you make the switch, the sooner you will immunise yourself against that future change. When we finally remove Common.Logging entirely, scriptcs will have one less, (problematic) dependency and will be more lightweight and easier to use for hosting and for building your own scriptcs based NuGet packages.
