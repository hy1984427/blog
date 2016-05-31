title: 22 Rules In Mobile App Testing
date: 2016-05-13
categories:
- Test
tags:
- Mobile
- App Testing
- Rules
comments: true
---
[Translated from my Chinese article.]

Nowadays, with the popularization of mobile devices, mobile apps are widely used everywhere, and lots of companies start business in mobile apps field.

This trend brings the challenges to use: we need to deliver the mobile apps to market in time with high quality, and continuously improve them.

We can categorize mobile apps to native apps and hybrid apps. They have many differences compared with Desktop/Web applications, and testers are encountered with more challenges due to the lacks of testing tool and methodology.

With testing a Telecom mobile portal app for more than 2 years, I have summarized some testing experience and practices for native apps on iOS and Android. And most of them are also useful for testing hybrid apps.

## 1.	Define your testing devices and operation systems

As we all know, the issue of mobile devices’ fragmentation is getting more and more serious, because there are more operation systems with different versions, different hardware of devices, different screen sizes, different resolutions, different pixel densities and etc.

We can’t cover all the devices and operation systems from ROI (Return of Investment) perspective, so we need to choose the testing devices based on the popularization of them in market.

We can not only use Google Analytics or Adobe Omniture to tracking user and generate the devices’ usage report to define testing devices and operation systems, but also read the market share of iOS and Android operation system from their official website:
https://developer.apple.com/support/appstore/, http://developer.android.com/about/dashboards/index.html.

## 2.	Switch between networks

The particularity of mobile app is user always use the app on their way, so when we are conducting our testing, we should also focus on the impact of network switch to app. For example we need to cover the scenario of using app on 3G network and then switch to 4G or Wi-Fi or Airplane Mode and so on.

If the app will authenticated on the mobile network and get a token from it then proceed functionalities forward, we will definitely cover the scenario of switching mobile network to other networks.

When we perform testing on network switching scenarios, we need also pay attention to the error message given to user. The error message should be user friendly, and give user instructions to follow to solve the issues.

## 3.	Multi-tasking

We usually run multiple apps on our mobile devices, so when we testing mobile apps, we need to consider the app switching.

This includes the scenarios of switching app to another, or close app at background, then how app acts when user open it again: should it reload the info when user left, or it will start from scratch?

## 4.	Gestures

Gestures are supported by iOS and Android operation systems, when app introduces it own gestures to user, they would conflict with the system ones.

Such as the wipe gesture to go back introduced in iOS 7: for a long time, to get back one screen, you had to tap the upper left corner on screen (named as “<” or“back” button); on iOS 7, we can swipe the screen to the right and go back.

But before iOS 7 introduced this gesture, many app use this gesture to call out drawer navigation panel in app. At the time when iOS 7 is released, many apps are not ready for this gesture, so when user use this gesture in app, sometimes it will go back, and sometimes it will call out the drawer navigation panel.

Since the user experience for this is so wired, so we should not let this happen from beginning.

## 5.	User experience

As we talk about user experience, we are not only focus on the layout and stuff like that, but also test app’s portrait and landscape display.

And we need to check the level of support to accessibility, so that more people can use our app more easily and freely.

We need also check whether app is following the design guide of iOS and Android official documents. Especially for the apps support both iOS and Android operation systems, it’s better to let each app follow the design guide of corresponding operation system, rather than all apps just follow iOS design guide.

And for the apps embedded Webview, if the Webview is not Responsive designed, it will cause display issues on different devices, therefore we may spend lots of efforts to fix them.

## 6.	Display messages and notifications

Since iOS and Android have different notification methods: on Lock Screen, Status Bar, Notification Center and App Icon, we need to check how the messages display when new message come.

When app use sensors or location services, we should also notify user about the changes.

If we consider the cache mechanism in app, the scenarios will be more complex.

Although using cache, we can reduce the workload of server to provide responses to mobile app client, but it complex our testing: we need to make sure once the message is changed, even the cached message is still valid, we should show the updated message, rather than the cached one.

## 7.	Features of Operation System

Because iOS starts to support widget since iOS 8, and Android supports widget for a long time, so when we are testing mobile apps, we need to consider widget as a serious feature, and prioritize it in features list.

And for Android apps, we also need to verify when apps are installed and running on both Dalvik and ART runtimes.

For iOS, if app allow user to configure its settings in System Settings, we need to test this feature as well.

## 8.	Sync on different devices

Apps like Facebook and Twitter support both iOS and Android; also support multiple devices, such as iPad, iPhone, Android phones and Android tablets.

Some of them allow user to login the same account on different devices. When this happens, we need to check once the message is delivered on one device, this message should be synced to other devices at same time.

## 9.	Customized User Interface

Besides the OS and hardware differences, different devices may also have different user interface installed, such as Samsung’s Touchwiz, HTC Sense, LG UX and Sony Xperia UI.

For example, Touchwiz has different default font compared to Android native font, which may cause your one line sentence displayed in two lines.

And previously on HTC Sense, you will see a menu bar all the time when using an app, this will affect the display of your app.

Also on HTC Sense 4.x, the multi-tasking screen shows the screenshot of apps, but when you open the app, because the app would refresh, so they may have differences between the screenshot in multi-tasking screen and app current view.

## 10.	Support different file formats

Sometime we need to display the legal information in mobile apps, usually the legal file are stored as PDF, so it’s hard for app to support this.

One reason is the PDF is usually designed for web or desktop, when viewing it on mobile devices, it’s hard for user to see, even with zoom in.

Another reason is Android doesn’t provide native support for PDF files, unlike iOS. So if we want to display PDF files to user, we need to figure out whether we need to add support to PDF, or we just let user to download one PDF viewer app to open the legal files.

If we need to deal with files in other format, we need to pay more attentions to this.

## 11.	Support multiple countries, locales and languages

Many countries have immigrants from global, and there maybe more than one language even in one country, so we can’t decide there is only one language for our app.

I know we can’t test with all countries, locale settings and languages, but we can test the widely used languages and corresponding formats, such as English, Chinese, French, Spanish and etc.

We need to verify not only the display of them in app, but also the input of them.

## 12.	High memory usage actions

Both iOS and Android have limitations for the memory size can be consumed by app, so when app need to deal with actions need to consume large memory, such as high density images, voices and videos, we need to perform the actions in another process, to avoid the memory leak or app killed by OS.

So we can check this by loading bulk of images, voices and videos. 

## 13.	Non-standard controls

On particular OS version, we may want to support some features, which OS can’t provide.

Usually we will implement it by ourselves or using 3rd party library.

One example is theme of app: in iOS 4, we need to implement it all by ourselves from scratch; in iOS 5, we can use UIApperance protocol to reduce some work; and in iOS 7, we can easily use tintColor property to achieve it.

If we are using our self made controls or 3rd party library, we need to pay more attentions when testing, to make sure the behavior the same as other parts.

And as in the example above, with the upgrades of OS, the control may be standardized and provided by OS natively. At that time, we need to merge our codes to native codes, to avoid merging it in the future.

To merge and test the codes are additional efforts compared to implemented it with native codes, so once we need to implement feature by ourselves or using 3rd party library, we need think twice before we actually work on it.

## 14.	App upgrade

Most app will upgrade gradually, to bring new feature and experience to users.

Once user installed app, when upgrade it, they won’t uninstall it first, but overwrite it or install incrementally.

So we will also test how app behaves after upgrading, especially the user profile stored previously, and other data stored.

We need be more careful if there is app database schema change in app upgrade.

Likewise, we need to verify app stored data are removed after app uninstallation.

## 15.	Integrate with 3rd party app

If app integrate with 3rd party app, such as Google Map or Facebook login, we need to make sure app can perform correctly when 3rd party app doesn’t work properly.

And we also need to monitor the 3rd party app or services.

Sometimes we need to consider to test when 3rd party app integrate with our app and want to share information to our app.

## 16.	Reduce dependencies

We can try our best to reduce the dependencies to 3rd party system/app/service and other web services we need.

This can help us minimize the complexity of app and reduce the testing efforts.

We also need to use different methodologies to make sure they work fine, such as API and integration testing and monitoring.

## 17.	Automation and Exploratory testing

Many companies and teams are using Agile now, so TDD and API testing are implemented in development.

For UI automation, we should use Test Pyramid to design our test first, then only automated test scenarios based user journey. 

Because unit test can cover the functionalities in every activity and view, but they can’t help us when we are testing the interactions between activities and views.

Likewise, when we conduct exploratory testing, we should focus on the workflows with data in them, and the ways the activities/views interact with each other.

I suggest you to use simulator or emulator to do automation, and if you are testing Android apps, Genymotion is a good choice for you; and for exploratory testing, we probably can use devices.

For the automation tools, they are plenty of them: appium, Frank, Calabash, UIAutomation, UiAutomator and so on.

## 18.	Security testing

Both iOS and Android support app to store their data in local SQLite databases, so we need to make sure the databases are encrypted.

We can also use tools like iPhone Configuration Utility and Android DDMS to sniffer the web traffic, checking whether there is cleartext in the request and response.

To test the security of mobile services, we can use the same way when we testing Web applications.

## 19.	Performance testing

Performance testing can be also involved in mobile app testing.

It includes testing the app in slow network connection, batch load large quantity of data and such scenarios.

We can also test the performance of mobile services.

## 20.	Operation System upgrade

Different with app upgrade, when operation system is going to upgrade, Apple and Google will let developers know in advanced, so that they can prepare their app to follow the new rules (guides :P).

When we testing the existing app, we need also to learn the changes introduced in new OS version, and prepare for them technically and in mind.

When the app for new OS is developing, we will cover the tests for both existing app and the new app at same time.

## 21.	Benefits from Log

In developing stage, we usually use Crashlytics, HockeyApp and such tool to collect crash issues, which can make the life for both developers and testers much easier.

And we can also us iPhone Configuration Utility and Android DDMS to collect general app logs.

When releasing app to market, it’s better we can have some ways to collect the crash information, either to ask user to give feedbacks or ask user to allow app to send logs to us when necessary.

## 22.	Continuous integration and continuous deployment

We can use continuous integration to help us find the stability issue and build a safe net for us.

Although we can’t 100% automate the process of continuous deployment, we can try our best to ease it, such as using TestFlight to distribute iOS apps, while using HockeyApp and other tools to distribute Android apps.

An alternative way to distribute Android app is using Dropbox: we can install Dropbox on the app package desktop and testing devices, once the app is packaged, we can use script to move it to the Dropbox folder on the packaging desktop; after Dropbox sync the apk file, we can open Dropxbox on testing devices to install the app.

<br>
I know those practices and sharing won’t solve all the problems and fix all the issues, but hope they can inspire you.

Since mobile apps are evolving, so does the mobile app development and testing. We will have new tools, techniques and methodologies in this trend. I am looking forward to hear your thoughts about mobile testing.

You can also find my presentation [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/22Rules_In_Mobile_App_Testing.pdf).