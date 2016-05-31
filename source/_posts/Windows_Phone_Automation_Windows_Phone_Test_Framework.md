title: Whindows Phone Automation - WindowsPhoneTestFramework
date: 2012-05-18
categories:
- Testing
tags:
- Mobile
- Windows Phone
- Automation
- Framework
- WindowsPhoneTestFramework
comments: true
---

Better to install VS2010 first, otherwise required testing framework/dll can't be found.

[**WindowsPhone system structure**](http://msdn.microsoft.com/zh-cn/windowsphone/ff955778.aspx)

**WindowsPhoneApp developing basics:**
1. [Windows Phone 7 开发常见问题汇集贴](http://social.msdn.microsoft.com/Forums/zh-CN/windowsphonezhchs/thread/5aacf6ab-dab7-4dcc-9d3d-d2c9095e490e)
2. [Windows Phone 7 手机开发](http://msdn.microsoft.com/zh-cn/ff380145)

**WindowsPhone Automation test:**
1. [Expensify/WindowsPhoneTestFramework](https://github.com/Expensify/WindowsPhoneTestFramework)
2. [UI Testing on Windows Phone 7](http://www.slideshare.net/cirrious/ui-testing-on-windows-phone)
3. [Windows Phone应用开发](http://www.cnblogs.com/chenkai/tag/Windows%20Phone/)

**Suggestions for development:**
1. WindowsPhone app use MVVM pattern: M is Model, it controls the data and events; V is View, which controls the display; and VM is View-Model, which is the bridge from Model to View.
2. A better way to design the app is heavily use M and VM, with light V. So that we can test the app easily and quickly, with stub and mock, rather than heavy UI tests.
3. A tool recommended is MVVM light, check the references from http://mvvmlight.codeplex.com/ and http://www.galasoft.ch/mvvm/.

**Problem encountered:**

The development of WhindowsPhone app is quite easy (please refer to [this](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/WP7App.zip), it's a basic WP7 app), but for automation testing, especially UI testing, it's the opposite.

When using WindowsPhoneTestFramework, following the steps in video of [Ui Testing on Windows Phone](http://www.slideshare.net/cirrious/ui-testing-on-windows-phone), the step of install BDD class library show error of JsonValue 0.5.0 can't be installed, even try to install JsonValue 0.6.0 still fails.

I am blocked by this. No clue found.