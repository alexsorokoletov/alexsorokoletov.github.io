---
layout: post
title: My favorite extensions for Visual Studio for Mac
comments: True
---
5 extensions to transform Visual Studio for Mac to even more powerful IDE. 
Each of them, when discovered (or developed), allowed me to save my time and simplify my workflow.
<!--more-->

### 1. NuGet Package Management 
This one I found out quite recently. If you ever used VS 2017, you know that it has much nicer and comfortable UI to manage NuGet packagets.
You can install and update multiple selected packages at once.

I found it very useful, especially when installing next MvvmCross update and keeping all other packages unchanged.

VS4Mac team, you should have that built in!
Thanks to @brent.l for the tip.

[Download NuGet Package Management extensions](https://github.com/mrward/monodevelop-nuget-extensions)

Github repo: [https://github.com/mrward/monodevelop-addins](https://github.com/mrward/monodevelop-addins)
 
### 2. DefaultDesigner 
This is a Xamarin Studio & Visual Studio for Mac add-in to open .xib and .storyboard files with Xcode Interface Builder by default.
By double clicking it. 

Awesome timesaver. Since I edit all storyboards in XCode only, this is a must for me.

[Download DefaultDesigner for Visual Studio for Mac](https://github.com/colbylwilliams/DefaultDesigner)

Github repo: [https://github.com/colbylwilliams/DefaultDesigner](https://github.com/colbylwilliams/DefaultDesigner)

### 3. MFractor
Kudos to Matthew for this amazing mega-extension (comes in two flavors, lite and pro).
I can't even start to describe what you can do with it, instead go and check the website https://www.mfractor.com/

Some of the features I like:
- XAML helpers essential for any Xamarin.Forms developer
- Android Resource intellinsense and help with navigation and replace with resource lookup
- Point out missing constructors for Xamarin.Android views/classes
- many other things I have not learned yet

MFactor Pro: [https://www.mfractor.com/products/mfractor-for-visual-studio-mac](https://www.mfractor.com/pages/download)

MFractor Lite: [https://www.mfractor.com/pages/download](https://www.mfractor.com/pages/download)

### 4. SortAndRemoveUsings
Disclaimer: this is my own extension. It helps me by automatically sort and remove usings when you save a C# file.
Extending VS4Mac was moderately easy, and I do that action anyway before commit since I like to keep my code clean.

No magic involved regarding usings here, instead of going through context menu this add-in does that on file save.

![Screenshot of VS4Mac extension to sort/remove C# usings automatically](https://github.com/alexsorokoletov/VisualStudioMac.SortRemoveUsings/raw/master/Meta/xamarin-save-sort.gif?raw=true)


[Download SortAndRemoveUsings for Visual Studio for Mac](https://github.com/alexsorokoletov/VisualStudioMac.SortRemoveUsings/releases/tag/1.1)

Github repo: [https://github.com/alexsorokoletov/VisualStudioMac.SortRemoveUsings](https://github.com/alexsorokoletov/VisualStudioMac.SortRemoveUsings)

### 5. SolutionName
Disclaimer: this is my own extension. This extension helps me easily determine which solution is open right now if I have several Visual Studio instances open. In a way, this is a cheap replacement for Windows 10 task switcher which shows icon and solution name when switching between VS 2017 instances.

![Screenshot of VS4Mac extension to show solution name](https://github.com/DreamTeamMobile/VS4Mac.SolutionName/raw/master/Meta/screenshot.png?raw=true)

[Download VS4Mac.SolutionName for Visual Studio for Mac](https://github.com/DreamTeamMobile/VS4Mac.SolutionName/releases)

Github repo: [https://github.com/DreamTeamMobile/VS4Mac.SolutionName](https://github.com/DreamTeamMobile/VS4Mac.SolutionName)


### Your recommendations?

Which extensions do you use? Let me know in the comments!


 {% include social.html %}