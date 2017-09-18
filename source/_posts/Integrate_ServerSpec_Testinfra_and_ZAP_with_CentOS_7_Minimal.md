title: Integrate ServerSpec, Testinfra and ZAP with CentOS 7 Minimal
date: 2017-09-07
categories:
- Testing
tags:
- ServerSpec
- Testinfra
- ZAP
- Python
- Ruby
- Integration
- CentOS 7 Minimal
comments: true
---
##### We already know how to setup tests with ServerSpec and Testinfra in [Quick start for ServerSpec and Testinfra, also comparison of them](http://hy1984427.github.io/2017/09/01/Qucik_Start_For_ServerSpec_and_TestInfra_also_comparison_of_them/) and [Quick start for integrating ZAP into CI](http://hy1984427.github.io/2017/09/06/Qucik_Start_For_Integrating_ZAP_into_CI/). Now, let's see how to integrate them with CentOS 7 Minimal.

### Prerequisite:

1. Install CentOS 7 Minimal successfully with root user setup and login.

2. Enable network on CentOS 7 Minimal following [this article](https://lintut.com/how-to-setup-network-after-rhelcentos-7-minimal-installation/).

  The detailed steps are:

  1) Open Network Manager through `nmtui`;

  2) Choose “Edit connection” and press Enter (Use TAB button for choosing options);

  3) Choose your network interfaces and click “Edit”;

  4) Choose “Automatic” in "IPv4 CONFIGURATION" and check "Automatically connect" checkbox, then press "OK" to quit from Network Manager;

  5) Reset network services through `service network restart`.

3. Adjust the screen resolution following [this article](https://superuser.com/questions/816528/with-centos-7-as-a-virtualbox-guest-on-a-mac-host-how-can-i-change-the-screen-r).

  The detailed steps are:

  1) Edit GRUB file: `vi /etc/default/grub`

  2) Append `vga=792` to the line `GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=cl/root rd.lvm.lv=cl/swap rhgb quiet`,

  and you will have `GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=cl/root rd.lvm.lv=cl/swap rhgb quiet vga=792`;

  *Check for the detailed [GRUB VGA Modes](#GRUB-VGA-Modes), or scroll down to the bottom of this article.*

  3) Add this change to GRUB configuration: `grub2-mkconfig -o /boot/grub2/grub.cfg`

  4) Reboot to take effect: `reboot`.

4. Add a Yum source and update.

  ```
  yum install epel-release
  yum -y update
  ```

5. Configure ssh key to connect server easily.

  * Generate ssh key: `ssh-keygen -t rsa`
  * Copy ssh public key to server: `ssh-copy-id -i ~/.ssh/id_rsa.pub username@server`
  * Add following lines to ~/.ssh/config.

    ```
    Host server
      User username
    ```

### Quick start for ServerSpec:

1. Install Ruby.

  `yum install ruby`

2. Install all necessary libraries.

  ```
  yum install gcc g++ make automake autoconf curl-devel openssl-devel zlib-devel httpd-devel apr-devel apr-util-devel sqlite-devel wget net-tools

  yum install ruby-rdoc ruby-devel
  ```

3. Install RubyGems.

  `yum install rubygems`

4. Install Rake.

  `gem install rake`

5. Install ServerSpec.

  `gem install serverspec`

6. Initial ServerSpec folder with basic settings. Please note the server will be set in this step.

  `serverspec-init`

7. Write and run the ServerSpec script according to its [API document](http://serverspec.org/resource_types.html).

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/ServerSpec.zip).

  Using `rake spec` under the test folder to run and check the result.

8. To run specific test rather than the entire test suite.

    Using `rake spec spec/host_server/sample_spec.rb` under the test folder to run and check the result.

### Quick Start for Testinfra:

1. Python 2.7 is installed by default, so we just need to install Pip.

  ```yum -y install python-pip
  pip install --upgrade pip
  ```

2. Install Testinfra and Paramiko.

  `pip install testinfra`
  `pip install paramiko`

3. Write and run the Testinfra script according to its [API document](http://testinfra.readthedocs.io/en/latest/modules.html).

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/testinfra_test.py).

  Using `testinfra testinfra_test.py` to run and check the result.

4. Some useful arguments we can use to make the test result more clear.

  Instead of using `testinfra testinfra_test.py` directly, we can add some arguments, such as `-q`, `-s`, `--disable-warnings` and `--junit-xml`.

  * The argument `-q` will run Testinfra in quiet mode, with less info exposed
  * The argument `-s` will let Testinfra capture No pre-test info
  * The argument `--disable-warnings` will disable warnings during Testinfra runs
  * The argument `--junit-xml` will export Testinfra test result into a xml file

  After adding those arguments, the command should be look like `testinfra -q -s --disable-warnings testinfra_test.py --junit-xml=report.xml
`

5. Now we can run Testinfra against the server using `testinfra -q -s --disable-warnings --ssh-config=/Path/to/ssh/config --hosts=server testinfra_test.py --junit-xml=report.xml`.

### Quick Start for ZAP:

1. Install JDK.

  `yum install java-1.8.0-openjdk*`

2. Download ZAP installation script.

  `wget https://github.com/zaproxy/zaproxy/releases/download/2.6.0/ZAP_2_6_0_unix.sh`

3. Change permission of the installation script and execute it.

  ```
  chmod 777 ZAP_2_6_0_unix.sh
  ./ZAP_2_6_0_unix.sh
  ```

4. Install required libraries.

  1) Install Selenium-WebDriver

   `gem install selenium-webdriver`

  2) Install IO

   `gem install io`

  3) Install Rest-Client

   ```
   yum install gcc-c++
   gem install rest-client
   ```

  4) Install RSpec

   `gem install rspec`

  5) Install and configure the headless Firefox

    ```
    yum -y install firefox Xvfb libXfont Xorg
    yum -y groupinstall "X Window System" "Desktop" "Fonts" "General Purpose Desktop"
    Xvfb :99 -ac -screen 0 1280x1024x24 &
    export DISPLAY=:99
    ```

  6) Download and setup geckodriver.

  ```
  wget https://github.com/mozilla/geckodriver/releases/download/v0.18.0/geckodriver-v0.18.0-linux64.tar.gz
  tar -xvzf geckodriver-v0.18.0-linux64.tar.gz
  mv geckodriver /usr/lib64
  ```

  7) Add following lines to ~/.bash_profile.

    `$PATH=$PATH:/usr/lib64`

    And run `source ~/.bash_profile`.

5. Using `ruby add_assertions_to_check_zap_result.rb` to run and check the result.

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/add_assertions_to_check_zap_result.rb).

#### *GRUB VGA Modes*

| Colour Depth | 640x480 | 800x600 | 1024x768 | 1280x1024 | 1400x1050 | 1600x1200 |
|:------------:|:-------:|:-------:|:--------:|:---------:|:---------:|:---------:|
| 8 (256) | 769 | 771 | 773 | 775 |
| 15 (32K) | 784 | 787 | 790 | 793 |
| 16 (65K) | 785 | 788 | 791 | 794 | 834 | 884 |
| 24 (16M) | 786 | 789 | 792 | 795 |
