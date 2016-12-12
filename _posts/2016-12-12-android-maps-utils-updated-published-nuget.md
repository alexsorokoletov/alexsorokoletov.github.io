---
layout: post
title: Android Maps Utils for Xamarin updated with new features and NuGet
comments: True
---
I've updated Xamarin Android Maps Utils recently and now one can get this as a [NuGet package](https://www.nuget.org/packages/Xamarin.Android.Maps.Utils).

`Install-Package Xamarin.Android.Maps.Utils -Pre`

<!--more-->

Currently project has dependency on the latest and greatest Google Play Services package from Xamarin (32.961.0) and requires "TargetFramework" 7.0 (which should be by default for all new projects anyway).
Besides NuGet, project has now latest (0.4.4) java code inside bringing more new features for users.

Now you get (shameless plug from original repo):

>
Marker clustering — handles the display of a large number of points
>
Heat maps — display a large number of points as a heat map
>
IconGenerator — display text on your Markers
>
Poly decoding and encoding — compact encoding for paths, interoperability with Maps API web services
>
Spherical geometry — for example: computeDistance, computeHeading, computeArea
>
KML — displays KML data
>
GeoJSON — displays and styles GeoJSON data

[Google's Github has a nice GIF demo of what Android Maps Utils can do now](https://github.com/googlemaps/android-maps-utils).

Interestingly, I was not getting any notifications about activities on this project on Github, now it's fixed. Thank you for the feedback and pull requests! 

Keep it coming!

_Related links:_

1. [Versioning of Xamarin.GooglePlayServices](https://github.com/xamarin/GooglePlayServicesComponents#versioning)
2. [Xamarin.Android.Maps.Utils on Github](https://github.com/alexsorokoletov/Xamarin.Android.Maps.Utils)

{% include social.html %}
