---
layout: post
title: MaterialDateTimePicker bindings for Xamarin.Android apps
comments: True
---

### Bindings for MaterialDateTimePicker for Xamarin.Android apps

One of the good libraries for Android apps made in Material design is the ["Material DateTime Picker"](https://github.com/wdullaer/MaterialDateTimePicker) from [@wdullaer](https://github.com/wdullaer).
Here is a short article how to create Xamarin.Android bindings library to consume that awesome picker in your Xamarin.Android applications.
<!--more-->

TLDR:  [https://github.com/alexsorokoletov/Xamarin.Wdullaer.MaterialDateTimePicker](https://github.com/alexsorokoletov/Xamarin.Wdullaer.MaterialDateTimePicker)

### Creating java library bindings for Xamarin.Android

There is an official guide from Xamarin: 
[Binding a Java Library - Consuming Java libraries from C#](https://developer.xamarin.com/guides/android/advanced_topics/java_integration_overview/binding-a-java-library/)

First, we need to get the JAR/AAR file.
Looking at [github page](https://github.com/wdullaer/MaterialDateTimePicker), we can see Maven banner. It means that we can grab a binary from Maven so let's go there and do it:

[https://repo1.maven.org/maven2/com/wdullaer/materialdatetimepicker/2.2.0/](https://repo1.maven.org/maven2/com/wdullaer/materialdatetimepicker/2.2.0/)


Next, following the Xamarin guide, we will see 3 errors:

```
Vendor/MaterialDateTimePickerBinding/obj/Debug/generated/src/Com.Wdullaer.Materialdatetimepicker.Date.MonthView.cs(34,34): 
  Error CS0533: `Com.Wdullaer.Materialdatetimepicker.Date.MonthView.MonthViewTouchHelper.GetVisibleVirtualViews(System.Collections.Generic.IList<Java.Lang.Integer>)' 
  hides inherited abstract member `Android.Support.V4.Widget.ExploreByTouchHelper.GetVisibleVirtualViews(System.Collections.Generic.IList<Java.Lang.Integer>)'
  (CS0533) (MaterialDateTimePickerBinding)
 ```  
 
 ```
 Vendor/MaterialDateTimePickerBinding/obj/Debug/generated/src/Com.Wdullaer.Materialdatetimepicker.Date.MonthView.cs(36,36): Error CS0534: `Com.Wdullaer.Materialdatetimepicker.Date.MonthView.MonthViewTouchHelper' does not implement inherited abstract member `Android.Support.V4.Widget.ExploreByTouchHelper.GetVisibleVirtualViews(System.Collections.Generic.IList<Java.Lang.Integer>)' (CS0534) (MaterialDateTimePickerBinding)
 ```
 and
  
 ```
 Vendor/MaterialDateTimePickerBinding/obj/Debug/generated/src/Com.Wdullaer.Materialdatetimepicker.Time.Timepoint.cs(23,23): Error CS0535: `Com.Wdullaer.Materialdatetimepicker.Time.Timepoint' does not implement interface member `Java.Lang.IComparable.CompareTo(Java.Lang.Object)' (CS0535) (MaterialDateTimePickerBinding)
 ```

1st and 2nd are pretty simple. As these methods most definitely will not be consumed from C# side, we can just remove these methods from bindings.

For that we use `<remove-node />` instruction in Metadata.xml.

{% highlight xml %}
<remove-node path="/api/package[@name='com.wdullaer.materialdatetimepicker.date']/class[@name='MonthView.MonthViewTouchHelper']/method[@name='getVisibleVirtualViews' and count(parameter)=1 and parameter[1][@type='java.util.List&lt;java.lang.Integer&gt;']]" />
<remove-node path="/api/package[@name='com.wdullaer.materialdatetimepicker.date']/class[@name='MonthView.MonthViewTouchHelper']" />
{% endhighlight %}

To solve 3rd problem we need to add a partial class to Additions folder:

{% highlight csharp %}    
namespace Com.Wdullaer.Materialdatetimepicker.Time
{
    /*
     * add partial class due to Java generics and IComparable interface in Xamarin.Android
     * Vendor/MaterialDateTimePickerBinding/obj/Debug/generated/src/Com.Wdullaer.Materialdatetimepicker.Time.Timepoint.cs(23,23): Error CS0535: 'Com.Wdullaer.Materialdatetimepicker.Time.Timepoint' does not implement interface member 'Java.Lang.IComparable.CompareTo(Java.Lang.Object)' (CS0535) (MaterialDateTimePickerBinding)
     * https://forums.xamarin.com/discussion/1950/binding-jar-file-with-class-that-implements-java-lang-icomparable
     */
    public partial class Timepoint
    {
        int Java.Lang.IComparable.CompareTo(Java.Lang.Object obj)
        {
            return CompareTo ((Timepoint) obj);
        }
    }
}

{% endhighlight %}

Solution to this issue I found on [Xamarin Forums](https://forums.xamarin.com/discussion/38247/binding-issue-with-android-support-v4-widget-explorebytouchhelper-getvisiblevirtualviews).

As usual, very helpful were comments and examples from [brendanzagaeski](https://github.com/brendanzagaeski) with different approaches to different java-to-csharp binding problems. Links to these examples are in the end of the page.

### JAR/AAR dependencies
This library we are binding now has only one dependency:
[https://github.com/wdullaer/MaterialDateTimePicker/blob/master/library/build.gradle#L23](https://github.com/wdullaer/MaterialDateTimePicker/blob/master/library/build.gradle#L23)

It is a `com.android.support:support-v4:23.1.1` which translates to Xamarin Nuget package called Xamarin.Android.Support.v4 of version 23.1.1.

So we will just add this Nuget package to our bindings project and that's it.
Clean and Build the project and you should see 0 errors.
Yaay!

### How to use this library (examples)

On C# side what you need to do is following:

1. reference the bindings project in your app project
2. in your activity implement interface OnTimeSetListener/OnDateSetListener 
3. create a datepicker and show it

These steps are described in the [Readme.md file of the original library](https://github.com/wdullaer/MaterialDateTimePicker/blob/master/README.md)

{% highlight csharp %}
//implement OnDateSetListener
public class HomeView : BaseActionBarView<HomeViewModel>,  
Com.Wdullaer.Materialdatetimepicker.Date.DatePickerDialog.IOnDateSetListener

{% endhighlight %}

{% highlight csharp %}        
private void OnChooseDateButtonClick(object sender, EventArgs e)
{
    Java.Util.Calendar now = Java.Util.Calendar.Instance;
    Com.Wdullaer.Materialdatetimepicker.Date.DatePickerDialog dpd = 
        Com.Wdullaer.Materialdatetimepicker.Date.DatePickerDialog.NewInstance(
        this,
        now.Get(Java.Util.CalendarField.Year),
        now.Get(Java.Util.CalendarField.Month),
        now.Get(Java.Util.CalendarField.DayOfMonth)
    );
    dpd.Show(FragmentManager, "Datepickerdialog");
}

public void OnDateSet(Com.Wdullaer.Materialdatetimepicker.Date.DatePickerDialog p0, int p1, int p2, int p3)
{
    p0.Dismiss();
}
		
{% endhighlight %}

Make sure you are using the same version of Xamarin.Android.Support.v4 package in your application project.

### Demo

![Material Date Time Picker in Xamarin.Android app](/assets/material-date-time-picker-xamarin-android.png)

### Source code

See [Xamarin.Wdullaer.MaterialDateTimePicker](https://github.com/alexsorokoletov/Xamarin.Wdullaer.MaterialDateTimePicker) github repository. 
Clone it and use it or just add as a submodule.


### Improving further

One thing definitely open for improvement is parameter names. Right now in many places it's just p0, p1, p2, p3.

Naming them hour, minute, second, isAmPm would make code cleaner and easier to understand.

So if you feel like fixing something feel free to submit pull request. (Or if you know how to fix these names automaticall - let me know, please).

### Helpful links
- My other Xamarin.Android bindings:

  [Tutorial – Android float label binding for Xamarin.Android](http://dreamteam-mobile.com/blog/2015/04/tutorial-android-float-label-binding-for-xamarin-android/)

  [Tutorial – Android-maps-utils bindings for Xamarin.Android apps](http://sorokoletov.com/2015/12/28/android-maps-utils-available-for-xamarin-android-apps-bindings/)

  
- brendanzagaeski Metadata.xml examples:

  [https://gist.github.com/brendanzagaeski/c32d65c21e152799af69](https://gist.github.com/brendanzagaeski/c32d65c21e152799af69)
  [https://gist.github.com/brendanzagaeski/6d1052a8b76f9067548e](https://gist.github.com/brendanzagaeski/6d1052a8b76f9067548e)
  [https://gist.github.com/brendanzagaeski/69f490e31ca6a71136ff](https://gist.github.com/brendanzagaeski/69f490e31ca6a71136ff)
  [https://gist.github.com/brendanzagaeski/3868e30b85a1feb1181b](https://gist.github.com/brendanzagaeski/3868e30b85a1feb1181b)
  [https://gist.github.com/brendanzagaeski/9834034](https://gist.github.com/brendanzagaeski/9834034)
  [https://gist.github.com/brendanzagaeski/9607158](https://gist.github.com/brendanzagaeski/9607158)
 
 {% include social.html %}