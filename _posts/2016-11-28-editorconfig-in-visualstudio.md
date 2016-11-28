---
layout: post
title: .editorconfig support in Visual Studio 2017
comments: True
---
Just recently, Visual Studio team published a post on [great features coming to the VS 2017 that will increase your productivity](https://blogs.msdn.microsoft.com/visualstudio/2016/11/28/productivity-in-visual-studio-2017-rc/). Among those, one small but powerful feature coming to the Visual Studio.

It's .editorconfig support.
What's so great about this?
<!--more-->

EditorConfig support is a good first step to support all platforms and editors of choice uniformely. Working with Xamarin apps from Windows and OS X in distributed teams, I learned hard way how tabs (gosh, again?) could be disruptive to smooth team work.

#### What is EditorConfig?
Quote from [EditorConfig website](http://editorconfig.org/):

>EditorConfig helps developers define and maintain consistent coding styles between different editors and IDEs. The EditorConfig project consists of a file format for defining coding styles and a collection of text editor plugins that enable editors to read the file format and adhere to defined styles. EditorConfig files are easily readable and they work nicely with version control systems.

In other words, having .editorconfig in the root of your project will guarantee that any editor supporting it will comply with defined settings and will keep your codebase in the same format as team agreed.

#### What can we config in the editor then?
EditorConfig in VS 2017 supports following settings:

- indent_style
- indent_size
- tab_width
- end_of_line
- charset
- trim_trailing_whitespace
- insert_final_newline

[Overview how to setup .editorconfig](https://docs.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options) is available in the Microsoft Docs.

I will quote one paragraph from the docs since it's very well written:

>... EditorConfig files enable you to maintain consistent coding styles and settings for a language ... in a codebase regardless of the editor or IDE you use.

EditorConfig doesn't support full set of C# formatting rules, so it's not a replacement for formatting settings in VS or code formatting policy in Xamarin Studio (XS stores settings in a .sln file).

I am sure we will see more tools/features coming to support different editors and environments to keep consistent coding styles.

If you are interested in the topic, please read my recent article on having [standard C# formatting style](.2016/11/13/csharp-standard-style/) and feel free to share your opinion.

##### Additional reading
[EditorConfig.org plugins for IDE](http://editorconfig.org/#download)

[Overview of VS 2017 Release Candidate](https://blogs.msdn.microsoft.com/visualstudio/2016/11/16/visual-studio-2017-rc/)

{% include social.html %}
