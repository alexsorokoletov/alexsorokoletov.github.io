---
layout: post
title: Contributing to .NET Core from OS X
---

Imagine you forgot your Windows laptop and have only old Macbook and you are on .NET hackathon. What do you do there? 

Well, you have number of options. What you probably dont  know *yet* is that you can develop using .NET also on your Macbook (or your spouse).

My choice was to contribute to [.NET Core](http://dotnet.github.io/core/) which works on everything (have you read recent announcement from RedHat?)

## Getting ready to rock

Before you can do anything with .NET on your Mac, you will need to do several things:

1) [Download Visual Studio Code (vscode)](https://code.visualstudio.com/Download)

2) Install the .NET Core itself. Here is a good guide to follow: [http://dotnet.github.io/core/getting-started/](http://dotnet.github.io/core/getting-started/)

3) Next step is to fork [dotnet/corefx repository](https://github.com/dotnet/corefx). In my case it's [https://github.com/alexsorokoletov/corefx](https://github.com/alexsorokoletov/corefx)


In order to get and compile current master of [dotnet/corefx repository](https://github.com/dotnet/corefx) you need to have the very latest Mono version installed. Mono website provides CI packages (nightly builds) for Windows and *nix, and provides nothing for OS X. So you have to compile it manually (just once, no worries)

### Compiling Mono on OS X
Good article to follow is available on the Mono website [http://www.mono-project.com/docs/compiling-mono/mac/#building-mono-from-a-git-source-code-checkout](http://www.mono-project.com/docs/compiling-mono/mac/#building-mono-from-a-git-source-code-checkout).

To make it easy to follow, here is what I did:

1) clone mono repository

{% highlight bash %}
mkdir monoci

git clone --recursive git@github.com:mono/mono.git

cd monoci/mono
{% endhighlight %}

2) install required tools

{% highlight bash %}brew install autoconf automake libtool pkg-config{% endhighlight %}

3) the compilation itself. In my case folder with mono was `~/work/monoci/mono`

{% highlight bash %}
CC='cc -m32' ./autogen.sh --prefix=~/work/monoci/mono --disable-nls --build=i386-apple-darwin11.2.0
make
make install
{% endhighlight %}
These lines of code took around hour to execute on my wife's old macbook.
Take a nap or go to gym. 

4) now, when mono is compiled, make sure you tell your system to use the fresh mono. To do that, just add ahead of the PATH variable your folder with mono

For example, modify ~/.bash_profile file and add line like that

{% highlight bash %}
export PATH="/Users/theuser/work/monoci/mono/bin:$PATH"
{% endhighlight %}
and then just quit and reopen terminal.

5) try it out by running `mono --version` in the terminal

{% highlight bash %}
Marias-MacBook-Pro:tests theuser$ mono --version
Mono JIT compiler version 4.3.0 (master/c278ead Thu Nov  5 11:03:13 PST 2015)
Copyright (C) 2002-2014 Novell, Inc, Xamarin Inc and Contributors. www.mono-project.com
	TLS:           normal
	SIGSEGV:       altstack
	Notification:  kqueue
	Architecture:  x86
	Disabled:      none
	Misc:          softdebug 
	LLVM:          supported, not enabled.
	GC:            sgen
{% endhighlight %}

6) last thing is to copy PCL reference libraries.
[Get it here 9.8mb](https://www.dropbox.com/s/6eg6diva5j33vus/mono-pcl-bits.tgz?dl=0) and unzip to folder `~/work/monoci/mono/lib/mono/xbuild-frameworks`

After doing that you should see two subfolders there:

{% highlight bash %}
Marias-MacBook-Pro:xbuild-frameworks theuser$ ls -la ~/work/monoci/mono/lib/mono/xbuild-frameworks
total 0
drwxr-xr-x   4 theuser  staff  136 Nov  5 11:53 .
drwxr-xr-x  12 theuser  staff  408 Nov  5 11:24 ..
drwxr-xr-x   9 theuser  staff  306 Nov  5 11:24 .NETFramework
drwxr-xr-x   6 theuser  staff  204 Aug 13 01:35 .NETPortable
{% endhighlight %}

### Compiling .NET Core on OS X

1) clone your fork of corefx

{% highlight bash %}
git clone --recursive https://github.com/alexsorokoletov/corefx.git
cd corefx
{% endhighlight %}

2) run the build script (yay!)

{% highlight bash %}
./build.sh /p:SkipTests=true
{% endhighlight %}
The parameter is telling build script to skip tests for now, let's first make sure the code compiles. Also, this part will take around 30 minutes to compile. 

The build process first gets some Nuget packages, then spins number of parallel compiler processes.
On my machine it failed at some point to spin new processes. To overcome that, I had to run this command in the terminal `ulimit -n 2048` to make sure system can spin more procesess - my limit was 256.

So if you have the same problem, just do the magic and restart the build. Probably, the build.sh should handle that process limit better

Great stuff is that build is incremental so it will start where it failed and will save time for all of us :)

3) Assuming you waited enough and things compile (if not, contact me and I will help), you're all set.

### Before we change anything in the .NET Core

So in order to contribute there are several things you need to keep in mind.

One, there is a legal side of any contribution. Read on that here, I'm not a lawyer.

Two, there is a contribution guide you should be familiar with.

Three, the flow is simple - find task -> create branch -> do change -> run tests -> commit & push changes -> create pull request.

I will be covering the flow, you need to read legal stuff and contribution guide.

### Changing .NET Core

For my first contribution I decided to find something easy and Stephen Toub was very kind and suggested to look at this issue: 

[Code cleanup for System.IO.Packaging #2699](https://github.com/dotnet/corefx/issues/2699)

Nothing fancy and a good way to learn.

1) Let's create a branch for changes
{% highlight bash %}
git branch fix2699
git checkout fix2699
{% endhighlight %}
2) Open VS Code and change something.

3) Let's compile and run tests

To make life easier, it's good to create an alias to the msbuild run by mono, so let's do that.
Edit ~/.bash_profile and add a line like that

{% highlight bash %}
alias mbuild='mono ~/work/corefx/packages/Microsoft.Build.Mono.Debug.14.1.0.0-prerelease/lib/MSBuild.exe'
{% endhighlight %}

.NET Core picks up the msbuild.exe from the Nuget package, so the exact package version could be different. 
If it is, look it up in the `build.sh` file around line 190
{% highlight bash %}
__msbuildpackageid="Microsoft.Build.Mono.Debug"
__msbuildpackageversion="14.1.0.0-prerelease"
{% endhighlight %}

Restart the terminal (or do `source ~/.bash_profile`) and let's go into the folder of the exact package you're touching.

{% highlight bash %}
cd src/System.IO.Packaging/tests/
{% endhighlight %}

and then run

{% highlight bash %}
mbuild /t:rebuildandtest /p:OSGroup=OSX
{% endhighlight %}

This code will compile whatever it sees (either whole .NET Core or just a package). If the changes are local to the package, there is no reason to recompile whole framework.

You should see:

{% highlight bash %}
  xUnit.net console test runner (64-bit .NET Core)
  Copyright (C) 2014 Outercurve Foundation.
  
  Discovering: System.IO.Packaging.Tests
  Discovered:  System.IO.Packaging.Tests
  Starting:    System.IO.Packaging.Tests
  Finished:    System.IO.Packaging.Tests
  
  === TEST EXECUTION SUMMARY ===
     System.IO.Packaging.Tests  Total: 122, Errors: 0, Failed: 0, Skipped: 0, Time: 2.080s
  Touching "/Users/theuser/work/corefx/bin/tests/OSX.AnyCPU.Debug/System.IO.Packaging.Tests/dnxcore50/tests.passed.without.OuterLoop.failing".
Done Building Project "/Users/theuser/work/corefx/src/System.IO.Packaging/tests/System.IO.Packaging.Tests.csproj" (rebuildandtest target(s)).

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:22.61
{% endhighlight %}

Nice, everything works!

4) Commiting the change. 
As the contributing guide suggest, you commit message should look like

{% highlight bash %}
Code cleanup in System.IO.Packaging

This request has several changes in ContentType.cs to make sure I understand the contribution workflow

Related to #2699
{% endhighlight %}

The empty lines are required because of the way github handles commits and pull requests.
As my commit doesn't really fixes anything, I wrote in the last line "Related to #issue_number" and not "Fixed #issue_number".

After you commit, push the changes to the github and create a pull request.

Github will pickup the pull request contents automatically.

Time to wait till .NET Core automatic build compiles your changes and also till .NET team provides feedback on the changes. 

5) Common thing is that you get feedback on pull request and need to do some minor changes.
After you do the changes, you might want to create a new commit, but there is a better way.

You can update your previous commit, by ammending the previous commit and force-pushing it to the server.

Easy in the terminal:

{% highlight bash %}
git commit --amend
git push origin +fix2699
{% endhighlight %}

Thanks for this tip, Stephen!

6) Then your pull request is accepted and merged. 
Congratulations!


### Summary

Things I just did were impossible all 12 years .NET existed before.

It is a still mind-blowing how .NET Core, build and tests work on anything running OS X, Linux and Windows.

What is great is that you can already write apps using .NET and C# for OS X (console and web apps so far). 

What is incredibly great is that how easy is to contribute to the .NET now. 

First time it might be hard, submitting first pull request took me 4 hours. Submitting second took me 30 minutes.

Huge thanks and kudos to the .NET Core team, Mat, Stephen and everyone else helping me!

I hope this experience will be helpful to other people having spare time and experience to share and reuse.


{% include social.html %}
