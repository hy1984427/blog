title: Quick start for UI tests with XCTest
date: 2017-11-24
categories:
- Testing
tags:
- UI Tests
- XCTest
- iOS
- Quick-Start
comments: true
---
##### XCTest is introduced by XCode 5 for iOS 7, but until XCode 8 with iOS 10, XCTest became the default and the only choice if you want to conduct UI testing, the previous UIAutomation is deprecated at that time.

### Prerequisite:

  * XCode and XCode command line tools should be installed.

  * Download the sample XCOde project from [here](https://developer.apple.com/library/content/samplecode/UICatalog/UIKitCatalogiOSCreatingandCustomizingUIKitControls.zip), unzip and open the Swift project.

### Get Started for XCTest:

1. Add a new Target of iOS UI Testing through "File"->"New"->"Target..."->"iOS UI Testing Bundle", and input all the required info.

2. Use the red dot (named "Record UI Test") on the bottom to start recording, once finished, click it again to complete.

3. Add assertions to the ends of each scenarios, such as the script in the "UIKitCatalogUITests.swift" of [XCTestSample.zip](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/XCTestSample.zip).

  For the detailed usage of assertions, you can reference to the "Test Assertions" in the [XCTest Topics](https://developer.apple.com/documentation/xctest).

4. It's hard to just check the text on some labels, so if we want to check it, we need to add value to identifier of the label, then in the testing script, using following lines:

  ```
  let label=app.staticTexts["label_identifier"]
  XCTAssertEqual(label.label,"label_text")
  XCTAssert(app.staticTexts["label_text"].exists)
  ```

5. If you want to know more usage of the XCTest, you can reference to [the Cheat Sheet](https://github.com/joemasilotti/UI-Testing-Cheat-Sheet).
