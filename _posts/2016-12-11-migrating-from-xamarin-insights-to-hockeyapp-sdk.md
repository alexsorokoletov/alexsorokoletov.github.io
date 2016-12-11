---
layout: post
title: Easiest way to migrate from Xamarin.Insights to HockeyApp 
comments: True
---
Xamarin.Insights was and still is a great exception tracking system for Xamarin applications. However

>... Do also note that Xamarin Insights is now considered to be end of life: since the acquisition by Microsoft, Insights has been deprecated in favour of HockeyApp.

Many of us have apps integrated with Xamarin.Insights. Now it's time to ditch Xamarin.Insights and put HockeyApp SDK instead.
HockeyApp has different integration points and API and some of the features from Insights are missing.

What would be the easiest way to migrate from Xamarin Insights to HockeyApp?

<!--more-->

### Meet DT.InsightsToHockey 
With this new project and NuGet package, all you have to do is:

- to remove Xamarin.Insights package and reference
- and  install DT.InsightsToHockey package.

Then, update the API key (change Insights' one to HockeyApp app id) and you good to go.

Quite simple, right?

Here is how it looks in the project:

Before:
![Xamarin Insights package in references before](/assets/insights_to_hockeyapp_before.png)

After:
![DT.InsightsToHockey package in references after](/assets/insights_to_hockeyapp_after.png)

Events already coming to HockeyApp after the changes:
![Events coming to HockeyApp after change](/assets/insights_to_hockeyapp_events.png)

### How it works?
Under the hood, package exposes exactly same API under same namespace, reporting and integrating with HockeyApp instead.

Literally, you have to change no code, just references.
Project supports PCL and iOS as of now, Android is in progress. I've tried to match all features available in Insights to HockeyApp SDK features. HockeyApp team was very helpful providing some feedback on features parity.

I will be glad to receive any feedback and help with this project (Android and UWP are waiting!)

#### Additional information
I recommended to take a look at features and limitations of HockeyApp SDK for each specific platform. 

HockeyApp is powerful and well-know metrics and crash tracking system. 
You can do in-app updates for iOS/Android, password-protected screens in the app (very well suited for diagnostics/admin screens). You can integrate a feedback page as well.


This fall Azure integrated HockeyApp with Azure. Data coming to HockeyApp can be available in App Insights for analysis and intelligence.
It is very powerful way to see and analyze events coming from your app. [Step by step guidance from AppInsights team.](https://azure.microsoft.com/en-us/blog/access-hockeyapp-data-in-ai-with-hockeyapp-bridge-app/)

Thank you for reading. Please let me know your thoughts on the subject.

_Related links:_

1. [DT.InsightsToHockey on Github](https://github.com/alexsorokoletov/DT.XamarinInsightsToHockeyApp)
2. [HockeyApp SDK for Xamarin](https://github.com/bitstadium/HockeySDK-Xamarin) and for [iOS](https://github.com/bitstadium/HockeySDK-iOS) and [Android](https://github.com/bitstadium/HockeySDK-Android)
3. [Xamarin.Insights Guide](https://developer.xamarin.com/guides/insights/)
4. [What is happening with Xamarin.Insights](https://www.xamarin.com/faq#qha1)
5. [Exploring HockeyApp data in Application Insights: introducing the Bridge App](https://azure.microsoft.com/en-us/blog/access-hockeyapp-data-in-ai-with-hockeyapp-bridge-app/)
6. [Custom events for everybody](https://www.hockeyapp.net/blog/2016/08/30/custom-events-public-preview.html)

{% include social.html %}
