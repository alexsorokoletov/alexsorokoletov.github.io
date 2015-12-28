---
layout: post
title: Android-maps-utils bindings for Xamarin.Android apps
comments: True
---
###Bindings for android-maps-utils for Xamarin.Android apps

Many developers use Google Maps for Android (Google Play Services) to display maps and related content in Xamarin-based Android apps.

Javascript SDK for Google Maps has clustering and other neat stuff built in. Android Maps SDK has none of this included. 
However, there is a solution!

<!--more-->

To address this problem, Google Maps team shared so called `android-maps-utils` extensions to the Android maps SDK which allows you do following:

- Marker clustering — handles the display of a large number of points
- Heat maps — display a large number of points as a heat map
- IconGenerator — display text on your Markers (see screenshot to the right)
- Poly decoding and encoding — compact encoding for paths, interoperability with Maps API web services
- Spherical geometry — for example: computeDistance, computeHeading, computeArea
- KML — displays KML data (Caution: Beta!)
- GeoJSON — displays and styles GeoJSON data

(Please read more on the official website: [http://googlemaps.github.io/android-maps-utils/](http://googlemaps.github.io/android-maps-utils/))

Down the road you can find steps and details how the bindings for quite complex java library could be created and how these fresh bindings for Android Maps utils could be consumed in the Xamarin Android application.

**To get to the how to part, scroll down**

###Existing Xamarin.Android bindings
Particularly I was interesting about clustering, as the application has to display around 500 markers in a dense area. 

Looking for existing solution to the problem, one can find [bindings made by Øystein Heimark](https://github.com/oystehei/MapsUtilityDemo). But these are quite old. To practice and to get the freshest code, I've created the updated bindings:

[github.com/alexsorokoletov/Xamarin.Android.Maps.Utils](https://github.com/alexsorokoletov/Xamarin.Android.Maps.Utils)

###Creating java library bindings for Xamarin.Android

There is an official guide from Xamarin: 
[Binding a Java Library - Consuming Java libraries from C#](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/binding-a-java-library/)

Following the guide, one can sometimes see warnings or erros on the bindings.
For example, 
`/GoogleMapsUtilityBinding/obj/Debug/generated/src/Com.Google.Maps.Android.Geojson.GeoJsonLineStringStyle.cs(118,118): Error CS0234: The type or namespace name 'IGeoJsonStyle' does not exist in the namespace 'Com.Google.Maps.Android.Geojson'. Are you missing an assembly reference? (CS0234) (GoogleMapsUtilityBinding)`

To fix this error all you need is to manually [edit Metadata.xml file and introduce these changes](https://github.com/alexsorokoletov/Xamarin.Android.Maps.Utils/blob/master/Transforms/Metadata.xml#L15)

As you proceed, there will be a few more errors and several warnings. Fixes for them are documented in the [Metadata.xml file](https://github.com/alexsorokoletov/Xamarin.Android.Maps.Utils/blob/master/Transforms/Metadata.xml)

More than helpful were comments and examples from [brendanzagaeski](https://github.com/brendanzagaeski) with different approaches to different java-to-csharp binding problems. Links to these examples are in the end of the page.

####JAR/AAR dependencies
Case with `android-maps-utils` is interesting because the jar/aar file depends on google-play-services.jar and android-support-v4.jar.
Xamarin suggests to add these jar files as ReferenceJar items by setting Build Action to ReferenceJar. What is missing here for a successful compilation is a C# side of these reference jars. After adding the jars you need to add a Nuget packages or Xamarin components with C# bindings for these jars.
With that change,C# bindings generator will generate correct code an generated C# code will be compilable, because you already have C# part of the referenced libraries.

One of the questions one might have - which version of google-play-services.jar should we use?
[The android-maps-utils library is compiled against google-play-services 7.8.0](https://github.com/googlemaps/android-maps-utils/blob/master/library/build.gradle#L19), so, basically, we can use anything >=7.8.0

###How to use this library (examples)

On C# side what you need to do is following:
0) reference the bindings
1) instantiate cluster manager

{% highlight csharp %}
//on MapReady
_clusterManager = new ClusterManager(this, _map);
_map.SetOnCameraChangeListener(_clusterManager);
_map.SetOnMarkerClickListener(_clusterManager);
{% endhighlight %}

If you already have custom handlers to map CameraChange/MarkerClick, you can do following (specifics of Xamarin.Android events subscriptions)
{% highlight csharp %}        
private void map_MarkerClick(object sender, GoogleMap.MarkerClickEventArgs e)
{
    var handled = _clusterManager.OnMarkerClick(e.Marker);
    if (handled)
        return;
    //your custom code
}
private void map_CameraChange(object sender, GoogleMap.CameraChangeEventArgs e)
{
    _clusterManager.OnCameraChange(e.Position);
    //your custom code
}
		
{% endhighlight %}


2) implement custom IClusterItem object
{% highlight csharp %}
public class ServiceClusterItem : Java.Lang.Object, IClusterItem
{
    public ServiceClusterItem(ServiceItem serviceItem)
    {
        Position = new LatLng(serviceItem.Lat, serviceItem.Lon);
    }

    public ServiceClusterItem(IntPtr handle, Android.Runtime.JniHandleOwnership transfer)
        : base(handle, transfer)
    {
        
    }

    public LatLng Position
    {
        get;
        set;
    }
}
{% endhighlight %}
3) add items to the clusterer and enjoy
{% highlight csharp %}
var clusterItem = new ServiceClusterItem(...);
_clusterManager.AddItem(clusterItem);
{% endhighlight %}



###Helpful links
- My other Xamarin bindings:
  [Tutorial – Android float label binding for Xamarin.Android](http://dreamteam-mobile.com/blog/2015/04/tutorial-android-float-label-binding-for-xamarin-android/)
  
- "Maps Shortcuts: Android Maps Utility Library" youtube video
	[Maps Shortcuts: Android Maps Utility Library](https://www.youtube.com/watch?v=nb2X9IjjZpM)
- example of use for original android maps util library
  [github.com/googlemaps/android-maps-utils/tree/master/demo](https://github.com/googlemaps/android-maps-utils/tree/master/demo)

- brendanzagaeski Metadata.xml examples:

  [https://gist.github.com/brendanzagaeski/c32d65c21e152799af69](https://gist.github.com/brendanzagaeski/c32d65c21e152799af69)
  [https://gist.github.com/brendanzagaeski/6d1052a8b76f9067548e](https://gist.github.com/brendanzagaeski/6d1052a8b76f9067548e)
  [https://gist.github.com/brendanzagaeski/69f490e31ca6a71136ff](https://gist.github.com/brendanzagaeski/69f490e31ca6a71136ff)
  [https://gist.github.com/brendanzagaeski/3868e30b85a1feb1181b](https://gist.github.com/brendanzagaeski/3868e30b85a1feb1181b)
  [https://gist.github.com/brendanzagaeski/9834034](https://gist.github.com/brendanzagaeski/9834034)
  [https://gist.github.com/brendanzagaeski/9607158](https://gist.github.com/brendanzagaeski/9607158)
 
 {% include social.html %}