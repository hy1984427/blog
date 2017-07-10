title: Quick start for appium (iOS and Android)
date: 2017-07-10
categories:
- Testing
tags:
- Mobile
- Appium
- Automation
- iOS
- Android
- Quick-Start
comments: true
---
##### There are many changes compared to the early version of appium, so I would like to start from scratch to build a simple testing script for appium. For further reading about how to apply Page Object and BDD to appium, you can read my previous article [Cookbook for appium](http://hy1984427.github.io/appium/) and [BDD with PageObject](http://hy1984427.github.io/BDD-with-PageObject/).

### Environment Settings:

1. Install XCode and its command line tool.

  To install XCode command line tool, you can use the following command in Terminal:

  `xcode-select --install`

  And download the OS versions you want to work with.

2. It's better to choose Ruby to write the test script as it's easy to get familiar with. And to install/upgrade it, we can use following commands:

  1) Install Homebrew

   `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

   2) Install Ruby

   `brew install ruby`

3. Install appium and related gems:

  ```
  brew install libimobiledevice --HEAD
  brew install node
  npm install -g ios-deploy
  sudo gem install xcpretty
  npm install appium-doctor -g
  npm install -g appium
  npm install wd
  npm install appium-xcuitest-driver
  brew install carthage
  gem install --no-rdoc --no-ri bundler
  gem install --no-rdoc --no-ri appium_lib
  gem install --no-rdoc --no-ri appium_console
  ```

4. Install JDK for Android:

  Download from [Java SE Development Kit 8 - Downloads](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) and install.

5. Install Android Studio from [Download Android Studio and SDK Tools | Android Studio](https://developer.android.com/studio/index.html) and download the OS version you want to work with.

6. Configure the bach_profile to set environment parameters:

  Edit your *`~/.bash_profile`*, if you don't have one, just create it and add following lines to it:

  ```
  export ANDROID_HOME=/Users/$your_name/Library/Android/sdk
  export ANDROID_SDK=$ANDROID_HOME
  PATH=$PATH:$ANDROID_HOME/build-tools
  PATH=$PATH:$ANDROID_HOME/platform-tools
  PATH=$PATH:$ANDROID_HOME/tools
  export JAVA_HOME="`/System/Library/Frameworks/JavaVM.framework/Versions/Current/Commands/java_home`"
  PATH=$PATH:$JAVA_HOME/bin
  export PATH
  ```

7. Run *`appium-doctor`* in Terminal to see if everything works fine. (You need to see there are all green.)

### Generate the apps to be tested:

1. Download the iOS UIKit Catalog source code from [UIKitCatalog](https://developer.apple.com/library/content/samplecode/UICatalog/UIKitCatalogiOSCreatingandCustomizingUIKitControls.zip).

  1) Unzip it and open the project in the Swift folder.

  2) Build it in XCode by selecting the OS version you would like to test.

  3) Expand the "Products" folder in the project structure, and select the app generated.

  4) Right click it and select "Show in Finder", and copy the app with extension as .app to your test folder for future use.

2. Download the "TextSwitcher" from "Import an Android code sample" in Android Studio welcome screen.

  1) Change the "minSdkVersion" from 7 to 9 in "build.gradle (Module:Application)" and build the app.

  2) Right click the root of project structure and select "Reveal in Finder", and copy the app with extension as .apk in "/Users/$your_name/AndroidStudioProjects/TextSwitcher/Application/build/outputs/apk" to your test folder for future use.

### Identify elements using appium GUI:

1. Download the appium GUI through their website;

2. Start appium GUI after install;

3. Start the server by default;

4. Start a new session;

5. Input all capabilities you want to use in "Desired Capabilities", such as:

  ```
  platformName   text   iOS
  platformVersion   text   10.2
  deviceName   text   iPhone 7
  app   text /Users/$your_name/Documents/UIKitCatalog.app
  ```

  Above shows the basic capabilities you must pass to appium to run the app. (For Android, it's basically the same, unless you first activity is not MainActivity so you need to set "appActivity" pointed to the right one.)

6. Then you can actually "Start Session". (For Android, you need to start your emulator before this, appium won't automatically start Android emulator like it does for iOS.)

7. Wait for some time, another window would popup. In this window, you should see the app's visual and could inspect elements in it.

8. Once click the element on app UI in the inspector window, you can see the properties of the element shows on the right side.

  1) Try to use id to locate element as possible.

  2) The "Tap", "Send Keys" and "Clear" button in the right "Selected Element" panel will simulate the corresponding action, and you will see the consequences in the app UI.

  3) You can use "Back" on the top of this window to simulate hardware Back action, and next to it is Refresh.

  4) The eye icon is very useful if you would like to record all your steps into a script. Any action after you click that icon will be recorded, until it's been clicked again.

  5) You can choose the language you want to record, and you can select to show/hide the supporting code (the code to support you start and end tests rather than the test steps), copy and delete the test codes.

  6) The last one on the top will close this window and entire session.

9. With the inspector, you have recorded some script, or you have written your own code with the inspector to locate elements. The code should look like following:

  ```
  require 'rubygems'
  require 'appium_lib'
  caps = {}
  caps["platformName"] = "iOS"
  caps["platformVersion"] = "10.2"
  caps["deviceName"] = "iPhone 7"
  caps["app"] = "/Users/$your_name/Documents/UIKitCatalog.app"
  opts = {
  sauce_username: nil,
  server_url: "http://localhost:4723/wd/hub"
  }
  driver = Appium::Driver.new({caps: caps, appium_lib: opts}).start_driver
  driver.navigate.back
  el1 = driver.find_element(:xpath, "//XCUIElementTypeCell[14]")
  el1.click
  el2 = driver.find_element(:xpath, "//XCUIElementTypeCell[1]/XCUIElementTypeTextField")
  el2.send_keys "123"
  el3 = driver.find_element(:xpath, "//XCUIElementTypeCell[2]/XCUIElementTypeTextField")
  el3.send_keys "456"
  driver.quit
  ```

  Save the script to a .sh file, such as test.sh.

### Run appium test:

1. Now we can run the script through the command line (Terminal).

2. After we close appium GUI, we need to run appium in a Terminal tab, then open your test folder. Under the folder, run following command:

    `ruby test.sh`

3. You should see the simulator started and test runs.

4. But we DO NOT have any assertion. Let's add some.

  1) Install RSpec through

  `gem install rspec`

  2) Include RSpec and its matchers in the "Require" section

  ```
  require 'rubygems'
  require 'appium_lib'
  require 'rspec'
  include RSpec::Matchers
  ```

  3) Add assertions to the test, like:

  ```
  driver.navigate.back
  el1 = driver.find_element(:xpath, "//XCUIElementTypeCell[14]")
  el1.click
  el2 = driver.find_element(:xpath, "//XCUIElementTypeCell[1]/XCUIElementTypeTextField")
  el2.send_keys "123"
  el2.value.should == "123"
  el3 = driver.find_element(:xpath, "//XCUIElementTypeCell[2]/XCUIElementTypeTextField")
  el3.send_keys "456"
  el3.value.should == "456"
  ```

  4) To make sure the assertions really works, we can alter like:

  `el3.value.should == "345"`

  5) And run it again, it should show following in Terminal:

  ```
  /usr/local/lib/ruby/gems/2.4.0/gems/rspec-support-3.6.0/lib/rspec/support.rb:87:in `block in <module:Support>': expected: "345" (RSpec::Expectations::ExpectationNotMetError)
  got: "456" (using ==)
  ```

  6) Now we can revert the change, and complete the test script.

  7) The final test script should looks like:

  ```
  require 'rubygems'
  require 'appium_lib'
  require 'rspec'
  include RSpec::Matchers
  caps = {}
  caps["platformName"] = "iOS"
  caps["platformVersion"] = "10.2"
  caps["deviceName"] = "iPhone 7"
  caps["app"] = "/Users/yhuang/Documents/appium/UIKitCatalog.app"
  opts = {
  sauce_username: nil,
  server_url: "http://localhost:4723/wd/hub"
  }
  driver = Appium::Driver.new({caps: caps, appium_lib: opts}).start_driver
  driver.navigate.back
  el1 = driver.find_element(:xpath, "//XCUIElementTypeCell[14]")
  el1.click
  el2 = driver.find_element(:xpath, "//XCUIElementTypeCell[1]/XCUIElementTypeTextField")
  el2.send_keys "123"
  el2.value.should == "123"
  el3 = driver.find_element(:xpath, "//XCUIElementTypeCell[2]/XCUIElementTypeTextField")
  el3.send_keys "456"
  el3.value.should == "456"
  driver.quit
  ```

5. Now it's all done.

### Please Note:

1. This is just a sample to run test, but we actually should write it in BDD way with Page Object.

2. It should also be integrated with CI.

3. If you want to use Appium to test Android, I recommend you to use Genymotion rather than AVD, even you are using x86 kernel for AVD.

4. You can reference to the [app](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/UIKitCatalog.zip) and [script](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/UIKitCatalog.sh) for UIKitCatalog and the [app](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/AndroidCalculator.apk) and [script](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/AndroidCalculator.sh) for Android Calculator.
