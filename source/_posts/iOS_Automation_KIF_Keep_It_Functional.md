title: iOS Automation - KIF (Keep It Functional)
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

Please download and unzip [KIF.zip](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/KIF.zip), and select "HelloWorld copy" to run KIFTests.

**Note:**
1. I have modified KIF framework: add "[(UITextField *) view setText:nil];" in KIFTestStep.m to clear text before input;
2. By default, using "[super initializeScenarios]" in EXTestController.m to initialize all scenarios, the order of scenarios are defined according to the scenario name ascending; to change the order of them, change corresponding lines in "initializeScenarios" in EXTestController.m.
3. For KIF tests, in order to get value of text field, we need to set "accessibilityValue" of it in code first, then we can access its value in testing code. It's better we can package them into one class, which we can reuse later.

**References for KIF:**
1. KIF uses undocumented Apple APIs. This is true of most iOS testing frameworks, and is safe for testing purposes, but it's important that KIF does not make it into production code, by duplicating a second target for the KIF-enabled version of the app to test. This gives you an easy way to begin testing -- *just run this second target* -- and also helps make sure that no testing code ever makes it into your App Store submission and gets your app rejected.
2. All of the tests for KIF are written in Objective C, so the performance is good.
3. Some detailed steps are not documented in KIF official documents, need to compare the docs with code provided in GitHub.

#### Advantages of KIF comparing to UIAutomation (official by KIF group but briefly):

**KIF Pros**
1. All objective-C. No need to write translations from JavaScript.
2. Can easily hook into your code base for obscure things, like "fake a credit card swipe."
3. Easily runs on the command line for CI. Current shipping version of UIAutomation does not.
4. No external dependencies. UIAutomation currently requires Instruments.
5. Easy to integrate into your Xcode workflow.
6. Tests run quickly. Runs in the app rather than being translated from Instruments/JS.
7. It's open source. If there's something you want to add, you can. If something's broken, it can be fixed quickly.

**UIAutomation Pros**
1. Runs outside of the process, so it could potentially simulate things like switching apps or hitting the home button. KIF could possibly do some of this using private API, but in general you need to mock these sorts of interactions.
2. JS is less verbose and may appeal to some more than ObjC.
3. Apple has full access to the frameworks and can potentially do some awesome things in the future.
4. We get new features for free over time, and old ones will always be supported. KIF uses a lot of undocumented API, so it's possible that future iOS releases would require it to be updated.

**Some materials for iOS - KIF framework:**
1. [KIF](https://github.com/square/KIF)
2. [KIF Google Groups](https://groups.google.com/forum/?fromgroups#!forum/kif-framework)
3. [iOS Integration Testing](http://corner.squareup.com/2011/07/ios-integration-testing.html)
4. [Enabling Accessibility Programatically on iOS Devices](http://sgleadow.github.com/blog/2011/11/16/enabling-accessibility-programatically-on-ios-devices/)