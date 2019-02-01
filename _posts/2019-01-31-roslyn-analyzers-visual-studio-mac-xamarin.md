---
layout: post
title: C# Analyzers and Xamarin (Visual Studio for Mac)
comments: True
---

The news of having C# compiler written in C# were crazy at first. Then everyone realized the power that comes with it.
One of the great features that came with Roslyn compiler was support for analyzers.

Analyzers are like a plugins for C# compilers. Easy to write and thus really flexible to adjust to your specific situation.

It's a powerful addition and there are hundreds of _GREAT_ analyzers on GitHub already.

How do we use that greatness with Xamarin apps?

<!--more-->

Visual Studio for Mac is still a main tool for anyone working with Xamarin and iOS and Android apps.
While we have Roslyn-based compiler there and .NET Core and .NET Standard support, analyzers are not yet first-class citizen

### So what do we do?
We need to do some manual actions to add analyzers to your Xamarin project.
I'll guide you through

1. Download analyzers that you want to use and place it in the folder at the solution level. Say, `Analyzers`.
    Look for any dependencies that these analyzers require.

    VS4Mac does not download analyzers automatically if you add Nuget package with an analyzer and there is also an issue to get together all dependencies.

    So we do it ourselves.
    Here I downloaded [VS Threading analyzers](https://github.com/Microsoft/vs-threading) since they provide warnings for `async void` methods and lambdas.

    ![Do it yourself - get analyzers and dependencies together](/assets/analyzers-download-manually.png)

 2. Open your project file `App.Core.csproj` and include your analyzer. Here is an example:

    ```
    <ItemGroup>
        <Analyzer Include="..\Analyzers\Microsoft.VisualStudio.Threading.Analyzers.dll" />
    </ItemGroup>
    ```

 3. Reload the project and enjoy

    These simple steps work for .NET Standard libraries and for Xamarin.Android projects. Xamarin.iOS projects seems to ignore `<Analyzer>` tag altogether.

    If you miss something from the dependencies, build log will have few warnings (not compilation warnings) saying the analyzer can't be loaded.
    Watch for these during setup!

4. _Optional_ - setup some of the warnings analyzers produce as errors.

    This is a nice addition you can have. While analyzers usually produce warnings, you can force them to be errors.
    For example, I really try to stick to the golden rule `avoid async void`.
    So, the threading analyzer would produce a warning with the code [VSTHRD100](https://github.com/Microsoft/vs-threading/blob/master/doc/analyzers/VSTHRD100.md).
    I'd like to make that a compilation error instead.

    It's as simple as step 2 - open the project file and add a line:
    ```
        <WarningsAsErrors>VSTHRD100;VSTHRD101;</WarningsAsErrors>
    ```
    The example also includes [VSTHRD101](https://github.com/Microsoft/vs-threading/blob/master/doc/analyzers/VSTHRD101.md) which is even more tricky - async void lambdas are hard to spot and easy to come by these days.

    If you want to ignore some warnigns (an analyzer can be noise, can't it?), here is another line to add to the project:

    ```
        <NoWarn>VSTHRD200</NoWarn>
    ```
    This one says do not warn me about making all async methods end with `Async` [VSTHRD200](https://github.com/Microsoft/vs-threading/blob/master/doc/analyzers/VSTHRD200.md)


### Any good analyzers you know?

My goal was to make sure there is an automatic safeguard against `async void` methods, anonymous or no. 
Thanks to Roslyn analyzers one can have it on 2 of 3 projects in a typical iOS/Android crossplat app.

Here are some good ones I've came through:

[DotNetAnalyzers](https://github.com/DotNetAnalyzers) - the most known collection of analyzers

[Microsoft/vs-threading/](https://github.com/Microsoft/vs-threading/) - great help with async void and overall async/await usage

[ironcev/awesome-roslyn](https://github.com/ironcev/awesome-roslyn) - more like an overview of analyzers you can get

Let me know which rules you specifically use in your projects, I'd be glad to try them as well and share with my readers.

### Visual Studio (not for Mac)
VS 2017 supports analyzers just fine, allowing you to install them from Nuget packages.
To be honest, I'm not sure why the Mac version does not support them yet. 

It's a great technology basically wasted on Xamarin side.

### Hidden candy

Nice thing about these analyzers - AppCenter.ms respects them. So if you follow steps 1 to 4, your builds in pull requests or branches will fail once someone uses `async void`. I like that.


That's it for today, thank you for reading!

 {% include social.html %}