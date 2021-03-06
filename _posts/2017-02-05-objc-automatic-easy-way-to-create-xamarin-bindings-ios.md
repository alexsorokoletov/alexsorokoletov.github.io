---
layout: post
title: Easy way to create Xamarin.iOS binding from CocoaPods
comments: True
---
Hey, Xamarin developers!
Developing iOS apps quite often includes using native libraries and Xamarin for iOS supports this by creating bindings.
This was 100% manual process until `ObjectiveSharpie`. After, `ObjectiveSharpie` began supporting `CocoaPods` (it is NuGet for iOS developers, so everything is in there). 
However, it doesn't work in many cases (it can't build something or reflect ObjectiveC code correctly).

Also, `sharpie` doesn't support dependencies within pods. This is where the new [`objc-automatic`](https://github.com/alexsorokoletov/objc-automatic) started.

But first, let me show you a demo how easy it is to create Xamarin.iOS binding for cocoapod.
<!--more-->
<iframe width="630" height="394" src="https://www.useloom.com/embed/e17d23d0ec3711e689c9c95ad27ead9a" frameborder="0" allowfullscreen></iframe>

### Prelude

Imagine you have to use [`Firebase/Analytics`](https://cocoapods.org/pods/FirebaseAnalytics) in your Xamarin app (Now, there is a NuGet for that, but let's rewind to the point where there wasn't one).
You would go ahead and create binding for that library, right. Unfortunately, `Firebase/Analytics` is what I call an umbrella pod. It has no code, just dependencies.

So, instead, you need to go and create bindings for pods like: `FirebaseCore`, `FirebaseInstanceID`, `GoogleInterchangeUtilities`, `GoogleSymbolUtilities` and `GoogleToolboxForMac/NSData+zlib`.
Literally, out of nowhere amount of work exploded. Now you need 5 bindings instead of just one. ObjectiveSharpie won't help us with that.

Interesting, [`FirebaseCore`](https://cocoapods.org/pods/FirebaseCore) also has dependencies. 
So, you need to create 5 Xamarin.iOS binding projects, do the code, reference them correspondingly and then you will have a binding for `Firebase/Analytics`.

Few questions arise: 
- How do you distribute that?
    Single binary? 
    Many binaries?
    NuGet package or packages?
- How do you version that?
    1.0.0 is not an option, and `Firebase/Analytics` is updated often.
    Oh, yes. Each of these pods has it's own version.

If you choose NuGet packages, then you also need to reference packages same way as pods and Xamarin.iOS projects.

Frankly, I tried to do that manually. After `Firebase/Core` and `Firebase/Analytics` I understood this is hard.
I couldn't imagine I will do that again when version 3.7.x of Firebase is released. Not mentioning sometimes you have to redo bindings to fix incorrect signature or binary


### Automate all the things.

We've been using F# FAKE for Xamarin iOS/Android builds for a long time and I love it.
I decided to try F# FAKE with this problem as well.

First, the tool generates correct tree of dependencies.
Downloads all metadata about pods. Versions, frameworks, linker flags, dependencies, etc.

Then, for each of the pods it create a subfolder inside `bindings` and puts there Xamarin.iOS binding project with C# API surface generated by `sharpie`.
It adds all linker flags, puts the iOS binary in there, adjusts the references to the dependencies (other Xamarin.iOS binding projects).

Now, you can build these Xamarin.iOS bindings using Xamarin Studio.

Turns out, it takes quite a time for XS, so the tool also creates `podname.build.sh` script to go ahead and `MSBuild` all the projects in correct order.

So the flow would be: run the script, see the errors, fix the errors, run it again (you can probably put it on --watch do run continously).

Now you have the .dll files with bindings. (pss, there is also a linker side of that story, take a look at `Linker.cs` files)


### Demo time!
Here are few videos showing how to use `objc-automatic` and how it works internally.

[Binding Firebase/Analytics using objc-automatic](https://www.useloom.com/share/7679bec0ec3911e6b8fde34d395e0c71)
<iframe width="630" height="394" src="https://www.useloom.com/embed/7679bec0ec3911e6b8fde34d395e0c71" frameborder="0" allowfullscreen></iframe>

[How objc-automatic works](https://www.useloom.com/share/f756df30ec3b11e6b8fde34d395e0c71)
<iframe width="630" height="394" src="https://www.useloom.com/embed/f756df30ec3b11e6b8fde34d395e0c71" frameborder="0" allowfullscreen></iframe>

### NuGet side of things
For the NuGet distribution, you want to have same tree of packages with same versioning.

`objc-automatic` tool does that for you. Along with projects, it generates `packages.config` and `.nuspec` files for each of the CocoaPods.
It also enhances `podname.build.sh` script to build packages and restore packages in correct order when building Xamarin.iOS binding projects.
For this goal tool creates local temporary NuGet feed and publishes built packages and restores dependencies from this source.

You want to be sure people can later restore our packages and have everything working (at least - compiling). 

`.nuspec` files will contain versions, metadata, everything needed.

### Does it work?
It worked for quite many cocoapods I used to test it with. More importantly, it worked with Firebase pods. 
For the `lottie-ios` it took around 1 minute to generate the `bindings` infrastructure (projects, NuGet package specs).
Around 1 more minute to run `lottie-ios.build.sh` and have NuGet package ready for publishing.

I'm not trying to brag here, just saying that basically compiling lottie manually and launching Xamarin Studio would take around that time.
In first place, the `objc-automatic` tool aims for complex pod bindings with dependencies. 

One binding sometimes is really hard to crack. Having 6 of them is even harder.


#### Dicsoveries
- Turns out, `ObjectiveSharpie` can't bind all the cocoapods.
- `ObjectiveSharpie` doesn't bind correctly some of the classes from iOS frameworks (misses casign of the managed class name)
- Sometimes you need to direct `ObjectiveSharpie` into correct set of header files, otherwise it skips some classes. C# API surface shows less than it should
- Xamarin.iOS linker doesn't understand that Xamarin.iOS app might be using a binding that might be using another binding. The latter one gets lost durign compilation process.
- Xamarin.iOS binding projects are actually very smart with `.framework`s.
- You can test complex NuGet package dependency scenario locally using local NuGet source
- You can create Xamarin.iOS binding with just Terminal and VS Code. Nice.
- It is so hard to debug F# FAKE scripts (ionide-fake VSCode extension doesn't help)
- CocoaPods spec can be a mess. Definitely moving forward I need to restructure this piece.

#### Links

- [Source code on GitHub: objc-automatic](https://github.com/alexsorokoletov/objc-automatic)
- [ObjectiveSharpie from Xamarin](https://developer.xamarin.com/guides/cross-platform/macios/binding/objective-sharpie/)
- [Xamarin Documentation - Binding Objective-C](https://developer.xamarin.com/guides/cross-platform/macios/binding/)

#### Disclaimer
This tool works as it is. There are still things to improve and fix (ZendeskSDK and AWSCognito pods are examples).
Not all metadata flows from pod to NuGet package.
Tool is opensourced and everyone is welcome to use it and improve it.

#### Thank you
Big thanks to Xamarin Team (Miguel and Bill) who helped taming Xamarin.iOS linker.
Thanks for F# FAKE team, this is a great tool to start with. 

And thank you for your interest and feedback. 
I appreciate any positive or negative feedback about `objc-automatic` and value any success or failure story using this tool.

 
 {% include social.html %}