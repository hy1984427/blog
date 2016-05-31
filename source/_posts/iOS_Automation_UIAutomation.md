title: iOS Automation - UIAutomation
date: 2012-04-26
categories:
- Testing
tags:
- Mobile
- iOS
- Automation
- Framework
- UIAutomation
comments: true
---

**This framework is not suggested!**

**Some materials for iOS - UIAutomation framework:**
1. [Working with UIAutomation](http://alexvollmer.com/posts/2010/07/03/working-with-uiautomation/)
2. [UI Automation](http://www.sunsetlakesoftware.com/sites/default/files/Fall2010CourseNotes/ui%20automation.html)
3. [UI Automation JavaScript Reference for iOS](https://developer.apple.com/library/prerelease/ios/documentation/DeveloperTools/Reference/UIAutomationRef/)
4. [How Do I Perform UI Automation Testing in iOS 4](http://www.codeproject.com/Articles/107595/How-Do-I-Perform-UI-Automation-Testing-in-iOS-4)
5. [iOS - UI Automation get textfield by accessibility label?](http://stackoverflow.com/questions/8306076/ios-ui-automation-get-textfield-by-accessibility-label)
6. [iOS Automated Tests with UIAutomation](http://blog.manbolo.com/2012/04/08/ios-automated-tests-with-uiautomation)
7. [Can't get value of UIAStaticText?](http://stackoverflow.com/questions/7688311/cant-get-value-of-uiastatictext)

**UIAutomation**
1. Using JavaScript: each line of JavaScript in UIAutomation will trigger a request to iOS simulator, which make the awful performance of it;
2. The framework is unstable: for the script attached, sometimes the testing results are different, even same environment and script are provided; and sometimes the simulator or instrument will hung and quit expected;
3. The testing supporting of this framework is not good: the element can be get by its id, but can only get by its name/label occasionally, and there are some issues unresolved, like value of label item can't be get;
4. Readiness of JavaScript is hard, it doesn't provide the capability to integrate with tools like Cucumber;
5. It doesn't have the capability to integrate with CI tools;
6. It can't run on real devices, but only on simulator, which means it can't run parallel.

### Attachments:

[UIAutomation.zip](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/UIAutomation.zip)