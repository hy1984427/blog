title: iOS Automation - Frank
date: 2012-05-14
categories:
- Testing
tags:
- Mobile
- iOS
- Automation
- Framework
- Frank
comments: true
---

Please download and unzip [HelloWorld_Frank.zip](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/HelloWorld_Frank.zip), and select "**HelloWorld copy**" to run FrankTests.

**Frank (Frankenstein!?) is 'Selenium for native iOS apps'.**
1. Frank use KIF and UISpec as base structure, and use UIQuery to locate elements.
2. Frank used to based on other framework, but recently it's changed to KIF, so some documents may out of date. Try to use the latest documents and samples.
3. You can use Ruby in Frank rather than Objective-C in KIF.

**Environment preparation:**
1. Install rvm, ruby (1.9.2 is preferred to avoid issue when installing ruby-debug later), frank-cucumber and follow the [installation script](http://www.testingwithfrank.com/installing.html) provided by Frank.
2. For debug use, I have added ruby-debug, please gem install it when you see message of that gem is required.

***Note:***
1. Screenshots will be generated under *HelloWorld/Frank/screenshots*, if the 1st step in `Then /^I can see "([^"]*)"$/ do |welcomeMessage|` is enabled;
2. DOM can be exported if the 2nd step in `Then /^I can see "([^"]*)"$/ do |welcomeMessage|` is enabled;
3. Add *@record* before scenario can start recording, but you should use it only once.

**Highly recommend before you start using Frank:**
1. http://www.testingwithfrank.com/screencasts.html
2. http://www.testingwithfrank.com/presentations.html

**Tips/Limitations:**
1. Build the app first, then run cucumber; otherwise sometimes, Frank can't attach to the simulator.
2. Encounter one issue which is not fixed, post in the gmail group: Google Groups. It's about how to check a text filed with space in its value.
3. For label, we can't access their value and check on them, Apple should be blamed, so we can only write code by ourselves to implement it from ground: set accessibilityValue -> write down the method to locate and compare the value -> write the step definition to recall the method -> use the step definition in Frank.
4. Few documents/samples for Frank, even how to use selector; and some step definitions are not suitable for use, need to google or write down basic steps for action and validation.

**How to use Symbiote (the elements' tree viewer):**

Few material for it, only found http://vimeo.com/22644221 and http://blog.thepete.net/2011/05/inspect-state-of-our-running-ios-apps.html

**Debug:**
1. Using Frank is not easy to debug, using ruby, so can use ruby-debug to debug, but you need to install gem of ruby-debug first, then follow steps in https://gist.github.com/1333785.
2. A better way is just installing ruby1.9.2 rather than 1.9.3
3. But for the step in Frank itself, you can only use "Console.app" on Mac to see log. Only few useful info can be found.

[**How to select elements for Frank using UIQuery/UISpec?**](http://stackoverflow.com/questions/9745842/how-to-select-elements-for-frank-using-uiquery-uispec)

**Recording of script running:**
1. Will recording everything from the scenario marked as *@record*.
2. With multiple *@record* tags, it will open recording app multiple times, which is unusable when recording. So only use *@record* tag once.
3. The recording is a feature of QuickTime player.

**Reference**
1. https://github.com/moredip/Frank
2. http://www.testingwithfrank.com/
3. https://groups.google.com/forum/?fromgroups#!forum/frank-discuss
4. http://code.google.com/p/uispec
4. Ruby-debug: http://bashdb.sourceforge.net/ruby-debug.html