---
layout: post
title: C# Standard Style - single source of formatting truth
comments: True
---

Months ago [Andrew Orsich](https://www.paralect.com/team) told me about this great tool called [javascript standard style](http://standardjs.com/).

Absolutely amazing. 

What it does is just formats code with one pre-configured, non-changeable, non-customizeable, non-arguable style. **You either use it or not**.

After that, all discussions about curly braces, tabs and spaces go to void. Done.

I'd like to have this for C#. 

<!--more-->

We all had enough of discussions whether use tabs or spaces (it's still happens), where to put braces, how do we name methods and variables. I feel this is especially important now, when .NET is opensource, when C# is being used for iOS and Android development, when many of C# developers are working on Mac and Linux. 

Frankly, we are all already using [Microsoft C# Coding Conventions](https://msdn.microsoft.com/en-us/library/ff926074.aspx) in some way. It's just these slight modifications that make us spend time on formatting and arguing about these changes or undoing them.

I want my team and everyone developing in C# focus on functionality and forget about formatting forever. 

Let's make it happen.


## There should be something for this, right?

Tools already exist, however...

One can use Visual Studio settings and configure C# formatting. Then do the same with Xamarin Studio.(strictly speaking, having XS format C# code exactly same way as VS is not possible). 
And what about VS Code?

There is an [.editorconfig support for Visual Studio](https://github.com/editorconfig/editorconfig-vscode) but it's covering just part of formatting questions. 

ReSharper is another candidate, however, it doesn't work with Xamarin Studio and you can customize rules.

[Oren](https://twitter.com/onovotny) kindly pointed there is a [dotnet/CodeFormatter project](https://github.com/dotnet/codeformatter) and [Roslyn and .NET Core teams use styles](https://github.com/dotnet/corefx/blob/master/Documentation/project-docs/contributing.md) enforced by the tool.

Good old StyleCop/FxCop might be suitable as well, but again, you can and have to customize it. 

Unfortunately, all available tools are customizable. As long as you use them, there will be always someone who will do something his way.  Again, I want to have this binary and simple choice. Use it or don't use it. 

## Should we build another tool?

Actually, CodeFormatter is a good candidate for becoming a single source of truth. It has default rules baked in, it runs on Windows and Mono and it's built with Roslyn and people are already using it. Here is a [list of rules CodeFormatter enforces](https://github.com/dotnet/corefx/blob/master/Documentation/coding-guidelines/coding-style.md).

I feel like we just need to extend it and build VS extension, Xamarin Add-in and VS Code extension to make experience seamless.

## It's just your crazy idea

According to the post from Roslyn team, I'm not alone

[Automatic code formatter released to GitHub](https://blogs.msdn.microsoft.com/dotnet/2015/02/09/automatic-code-formatter-released-to-github/)

> We strongly believe that having a consistent style greatly increases the readability and maintainability of a code base. The individual style decisions themselves are subjective, the key is the consistency of the decisions. As such the .NET team has a set of style guidelines for all C# code written by the team.



## I have another idea/opinion

I would love to hear your feedback and thoughs on this idea. Feel free to contact me or comment this article and please share it with your team and friends.


## Q&A

#### I do not want to re-format my solution.
You don't need to re-format existing codebase unless you want. When you start your next project, this is the best time to start using C# Standard Format. 

#### Why we should enforce style A and not enforce style B.

Discussing specific style settings is not the goal of this post. 

#### I will not use this C# standard formatting. 

Fine, it's "use it or don't". Nothing will change for people that don't want to use a non-customizeable set of rules. A lot of free time will come to those that want to use it.

{% include social.html %}
