title: Quick start for ServerSpec and Testinfra, also comparison of them
date: 2017-09-01
categories:
- Testing
tags:
- Infrastructure Testing
- ServerSpec
- Testinfra
- Python
- Ruby
- Quick-Start
- Comparison
comments: true
---
##### ServerSpec is to check servers are configured correctly through their actual state using RSpec tests. Testinfra is kind of a Serverspec equivalent in Python and based on Pytest.

### Prerequisite:

1. RVM, Ruby and rubygems are installed.

2. Python and pip are installed.

3. We would expect to run Testinfra and ServerSpec against the server rather than our local machine, so we may need to tweak a little bit by connecting the server with ssh key.

  * Generate ssh key: `ssh-keygen -t rsa`
  * Copy ssh public key to server: `ssh-copy-id -i ~/.ssh/id_rsa.pub username@server`

### Get Started for ServerSpec:

1. Install ServerSpec.

  `gem install serverspec`

2. Initial ServerSpec folder with basic settings. Please note the server will be set in this step.

  `serverspec-init`

3. Write and run the ServerSpec script according to its [API document](http://serverspec.org/resource_types.html).

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/ServerSpec.zip).

  Using `rake spec` under the test folder to run and check the result.

4. To run specific test rather than the entire test suite.

    Using `rake spec spec/host_server/sample_spec.rb` under the test folder to run and check the result.

### Get Started for Testinfra:

1. Install Testinfra.

  `pip install testinfra`

2. Write and run the Testinfra script according to its [API document](http://testinfra.readthedocs.io/en/latest/modules.html).

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/testinfra_test.py).

  Using `testinfra testinfra_test.py` to run and check the result.

3. Some useful arguments we can use to make the test result more clear.

  Instead of using `testinfra testinfra_test.py` directly, we can add some arguments, such as `-q`, `-s`, `--disable-warnings` and `--junit-xml`.

  * The argument `-q` will run Testinfra in quiet mode, with less info exposed
  * The argument `-s` will let Testinfra capture No pre-test info
  * The argument `--disable-warnings` will disable warnings during Testinfra runs
  * The argument `--junit-xml` will export Testinfra test result into a xml file

  After adding those arguments, the command should be look like `testinfra -q -s --disable-warnings testinfra_test.py --junit-xml=report.xml
`

4. Now we can run Testinfra against the server using `testinfra -q -s --disable-warnings --ssh-config=/Path/to/ssh/config --hosts=server testinfra_test.py --junit-xml=report.xml`.

### Comparison between ServerSpec and Testinfra:

##### Advantages of ServerSpec

  * More documents and community support (compared to Testinfra);

  * The scripts, test results and reports are more readable (Testinfra is based on Pytest, and can only export report to XML not JSON);

  * Although Testinfra support “sysctl” and it runs successfully on the server, the command in script can't run, and prompts error "bash: sysctl: command not found"; may also occurs to other commands/resources (potential risk);

  * ServerSpec can support more resources and attributes for resources than Testinfra.

##### Advantages of Testinfra

 * Can support most common resources, such as Docker, File, Group, Service, Socket and etc (equivalent to ServerSpec);

 * Can show the actual value when validating the permission but failed, so the debug will be quicker.

#### Conclusion

  If you can only choose Python as your programming language, you have to use Testinfra; otherwise, I recommend ServerSpec because of the benefits it can bring.
