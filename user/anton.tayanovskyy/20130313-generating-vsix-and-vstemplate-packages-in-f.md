---
title: "Generating VSIX and VSTemplate packages in F#"
categories: "f#,visualstudio,build,vstemplate,vsix,extensions"
abstract: "Announcing a small utility library for generating VisualStudio VSTemplate and VSIX packages."
identity: "3061,76244"
---
We just released a few utilities on NuGet under the temporary name of `IntelliFactory.Build v0.0.1` (see Apache-licensed [source](https://bitbucket.org/IntelliFactory/build)). Right now this kitchen-sink assembly contains F# API for generating VisualStudio template files (VSTemplate) and extension packages (VSIX), and is usable from .NET-based build automation tools such as [FAKE](http://github.com/fsharp/FAKE).  Plans are to add support for item templates and multi-project templates, and also support for embedded NuGet repositories in VSIX. Our need for this is WebSharper 2.5 - but it might be useful to you too.

If you ever tried to author VisualStudio templates and extensions by hand, you know the XML hell it creates. Various problems with the MSDN documentation do not help. In the end, we want to be programmers, not XML editing technicians running around trying to keep things in sync.  A good way to keep things in sync and to apply the do-not-repeat-yourself principle is to automate - to define things in code, not XML. F# is a good option for this. If you agree so far, and you envision building or maintaining VisualStudio templates/packages, consider giving our little library a look - it may save you some time. We also gladly accept code contributions.

I notice there are a few more missing things I would like to have in FAKE; when the build utilities mature, we might consider re-labelling them and adding to FAKE or releasing under a more specific name. Documentation is coming - for now please consult FSI signature files in source.
