---
layout: post
title: Coding for fun - AT&T signal levels on route from DC to Snowshoe, WV
comments: True
---
We go the [Snowshoe ski resort](http://snowshoemtn.com) almost every weekend when there is snow.
Driving there takes up to 5 hours.
As long as you drive off the interstate you loose your cell signal.

Last year we had to learn how to drive and communicate in a disconnected world. We now know the route by heart, we know stops, we know which restaurants or gas stations have Wi-Fi. We also got some low-power FRS radios to have more flexibility during our trips.

Honestly, I was always wondering why T-Mobile was so bad in this area. This spring I switched to AT&T to compare the coverage on Outer Banks (where T-Mobile is also failing to get you even basic coverage).
Now it`s winter and it's time to check out AT&T coverage in our favorite skiing area.

Let's see how we do that and what we find
<!--more-->

### Why
As the article says, point is get some code just for fun. And see, if switching from T-Mobile to AT&T makes any sense.
And see, maybe there is a reason why we have almost no coverage off the interstates. 

### Technical details
So, I had macbook with OS X and Windows 10, iPhone 6 and Lumia 950 and Lumia 925. First idea was to have Lumia 950 with AT&T and Lumia 925 with T-Mobile run same app along the route and then compare results. Unfortunately, I forgot my USB C cable so 950 died on the way there. 
So plan changed and I got an iOS app and did a AT&T cellular network signal level log on the way back. 

Interesting that iOS has no public API to get cellular service signal level. There was some way in iOS 4.2 days [VAFieldTest](https://github.com/valexa/VAFieldTest). Doesn't work now.
However, there is a way to get a signal level on iOS 9 devices. Kind gentleman shared the idea on Stack Overflow: [Measuring cellular signal strength](http://stackoverflow.com/a/34389611/883738)

That was enough for the first version. 

You can get the app from github - [github.com/alexsorokoletov/SignalLogger](https://github.com/alexsorokoletov/SignalLogger). App tracks GPS and signal level every 10 seconds, while tracking screen stays active, all results are exported to CSV and emailed to you after you stop tracking. Keep it simple.

### Results
Gist with raw CSV data: [DC to Snowshoe ATT signal strength data.csv](https://gist.github.com/alexsorokoletov/0ce68926804dee671563)

Using QGIS I was able to get the results on the map:

This is a complete route from DC to Snowshoe, WV with AT&T signal level. Green - available, red - unavailable.
I have no idea yet how to make the route colorful with orange where the signal is low and green where it is good.
![Complete route from DC to Snowshoe, WV with AT&T signal level](/assets/dc-snowshoe-att-signal-level-complete.png)

Part of the route where the service is available.
![Route from DC to Snowshoe, WV where AT&T is available](/assets/dc-snowshoe-att-signal-level-available.png)

Part of the route where the service is **not** available.
![Route from DC to Snowshoe, WV without AT&T service](/assets/dc-snowshoe-att-signal-level-unavailable.png)

And also a terrain based part of the map where the AT&T service is interrupted.
![AT&T service with terrain map as a base](/assets/dc-snowshoe-att-signal-level-terrain.png)

Looks like the rule for the AT&T service is simple. Inside park areas (forests, national and state parks - green on the terrain map) service is absent. Otherwise AT&T gives some coverage.

I was surprised to find out that the coverage is not actually related to [the US NRQZ area](https://en.wikipedia.org/wiki/United_States_National_Radio_Quiet_Zone) or distance to [the Green Bank Radio Observatory](https://en.wikipedia.org/wiki/Green_Bank_Telescope). 
One can get some details about that in the NPR article [Enter The Quiet Zone: Where Cell Service, Wi-Fi Are Banned](http://www.npr.org/sections/alltechconsidered/2013/10/08/218976699/enter-the-quiet-zone-where-cell-service-wi-fi-are-banned)
### Next steps
Next time we go to the Snowshoe resort, I'm going to get T-Mobile coverage results and then compare and see if it makes sense to have one or another.

### Thoughts
- What is wrong with Apple not providing API for cellular service signal strength?
    
  I guess it would benefit if Apple would be more open in terms of APIs available.
- There are interesting ways of getting data through private Apple APIs and from private UIView classes [Get an object properties list in Objective-C](stackoverflow.com/questions/754824/get-an-object-properties-list-in-objective-c) and [CoreTelephony.h file](https://github.com/valexa/VAFieldTest/blob/master/CoreTelephony.txt)

  These ways will stop your app from being published in the App Store, but for experiments like this are good alternative. 
- Let's see if Microsoft is more open with cellular 
  
  Probably, there are different ways to access this data just because MS had different API sets for WP7, WP8, WinRT (WPA81) and Windows 10 UWP. Let's see who is the winner.
  
- I want to render same data in the Microsoft Power BI. Presumably, this is the kind of tasks to be handled by the Power BI.
  
 {% include social.html %}