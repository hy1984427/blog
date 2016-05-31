title: Android Automation - NativeDriver
date: 2012-05-09
categories:
- Testing
tags:
- Mobile
- Android
- Automation
- Framework
- NativeDriver
comments: true
---

**Library Configuration Tip:**
1. To run the Robotium tests, we need to select all in "Properties -> Java Build Path -> Order and Export";
2. Move the robotium up to top and move Android and Android Dependencies just below robotium (but still above any project).

**Tips:**
1. Create the tests as JUnit tests;
2. Under application folder, run `$Android-SDK-Prefix/platform-tools/adb shell am instrument com.calculator/com.google.android.testing.nativedriver.server.ServerInstrumentation`; then run `$Android-SDK-Prefix/platform-tools/adb forward tcp:54129 tcp:54129`
3. Connection and port forward are easily broken, need to check them when tests fail, especially once the tests fail or code is changed.
4. If the element, especially textfield, is not focused, you can't input anything in it; unless you `click()` it before `sendKeys()`
5. After input into textfield, you need to navigate back to disable keyboard, using `driver.navigate().back()`
6. No api about `wait()`, just use `Thread.sleep()`
7. Using the take screenshot method, you can only use full path

**References:**
1. [NativeDriver](http://code.google.com/p/nativedriver)
2. [NativeDriver Wiki](http://code.google.com/p/nativedriver/w/list)
3. [NativeDriver Source](https://code.google.com/p/nativedriver/source/browse/trunk/android/test/com/google/android/testing/nativedriver/)
4. [NativeDriver Google Groups](https://groups.google.com/forum/#!forum/nativedriver-users)
5. [TestNG + NativeDriver实现android的UI自动化测试](http://kongqingyun123.blog.163.com/blog/static/63772835201172935833719/)
6. [How to Screen Capture On Error](https://groups.google.com/forum/#!msg/nativedriver-users/1eZRMu3KbPA/FMn9R6upuDwJ)
7. [Windows下NativeDriver截屏功能](http://kongqingyun123.blog.163.com/blog/static/63772835201182932332956/)

### Attachments:

[NativeDriverAndroidTests.zip](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/NativeDriverAndroidTests.zip)