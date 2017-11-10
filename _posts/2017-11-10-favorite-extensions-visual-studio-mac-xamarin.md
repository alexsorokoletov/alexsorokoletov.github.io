---
layout: post
title: My favorite extensions for Visual Studio for Mac
comments: True
---
4 extensions to transform Visual Studio for Mac to even more powerful IDE. 
Each of them, when discovered, allowed me to save my time and simplify my workflow.
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
Shameless plug - this is my own extension. It helps you by automatically sort and remove usings when you save a C# file.
Extending VS4Mac was moderately easy, and I do that action anyway before commit since I like to keep my code clean.

No magic involved regarding usings here, instead of going through context menu this add-in does that on file save.

[Download SortAndRemoveUsings for Visual Studio 4 Mac](https://github.com/alexsorokoletov/VisualStudioMac.SortRemoveUsings/releases/tag/1.1)

Github repo: [https://github.com/alexsorokoletov/VisualStudioMac.SortRemoveUsings](https://github.com/alexsorokoletov/VisualStudioMac.SortRemoveUsings)


### Your recommendations?

Which extensions do you use? Let me know in the comments!


 {% include social.html %}