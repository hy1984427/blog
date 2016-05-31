title: Android Automation - Robotium
date: 2012-05-18
categories:
- Testing
tags:
- Mobile
- Android
- Automation
- Framework
- Robotium
comments: true
---

**For installation**, please reference to [Installing the SDK | Android Developers](http://developer.android.com/sdk/installing.html). The easiest way is to download Eclipse first, then install the Android Development Tools (ADT) as Plugin in Eclipse.

### How to use the attached project zip file:

1. Unzip it first then copy them to you workspace;

2. Open Eclipse, and select "File -> New -> Other...";

3. Select "Android -> Android Project", and "Next";

4. Select "Create project from existing source", and input folder path of "AndroidCalculator" project into "Location", and change "Project Name" to "AndroidCalculator";

5. "Next" and select "Build Target" as "Android 2.3.3", then "Finish".

```
Note:

Please do the same to "AndroidCalculatorTest, but change the project folder path and change "Project Name" to "AndroidCalculatorTest".*

For running RobotiumTests_DataDriven.java, you need to import TestData.csv to emulator first:

1. Open DDMS;

2. Open File Explorer;

3. Import the TestData.csv file in attached test project folder, into /data/data/com/calculator/files.

4. Run the data driven test.
```

### How to run the application we test against:

1. Create a AVD of Android 2.3.3 in Android Virtual Device Manager (AVD Manager) under "Window -> AVD Manager" in Eclipse;

2. Select project "AndroidCalculator", and "Run As" "Android Application".

### How to run tests against the application:

1. Create a AVD of Android 2.3.3 in Android Virtual Device Manager (AVD Manager) under "Window -> AVD Manager" in Eclipse;

2. Open "AndroidCalculatorTest -> src -> com.calculator.test";

3. Select any of the java file and "Run As" "Android JUnit Test".

```
Note: please only select one file to run, because all of them have a main() method, so they will conflict with each other when run them all together.
```

### Features included in "AndroidCalculatorTest":

1. "InstrumentationTests.java" is implemented by Instrumentation in Android SDK itself;

2. "RobotiumTests_APK.java" demonstrate how to test an Android APK file when only get the APK file itself, its "Package_ID" and "Main_Activity";

3. "RobotiumTests_DataDriven.java" shows how to read a group of test data from a csv file, run series of tests, then export the results to another file;

4. "RobotiumTests_Whitebox.java" shows how to use Robotium in UT level, more from code coverage perspective;

5. "RobotiumTests.java" give you a brief knowledge of writing a basic Robotium test and take screenshot in the test. (please check the screenshot under "/mnt/sdcard" in DDMS).

### Library Configuration Tip:

1. To run the Robotium tests, we need to select all in "Properties -> Java Build Path -> Order and Export";

2. Move the robotium up to top and move Android and Android Dependencies just below robotium (but still above any project).

### Issues encountered when writing Robotium tests:

1. Unable to take ScreenShot (saving the file to emulator):  [Unable to take screenshot on android using robotium and private method](http://stackoverflow.com/questions/7550201/unable-to-take-screenshot-on-android-using-robotium-and-private-method)

2. "Null pointer exception" or "Array out of boundary" when reading a csv file: please check the format of csv whether it's compatible with jxl.

### Some materials for Android - Robotium framework:

1. [Android Developers](http://developer.android.com/)

2. [robotium -  It's like Selenium, but for Android™ - Google Project Hosting](http://code.google.com/p/robotium/)

3. [Controling Quality: Design Data Driven Framework around Robotium](http://controlingquality.blogspot.com/2011/02/design-data-driven-framework-around.html)

4. [Robotium_Black_Box_Testing_For_Android](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/Robotium_Black_Box_Testing_For_Android_Chinese.pdf) *in Chinese*

### Attachments:

[Robotium.zip](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/Robotium.zip)