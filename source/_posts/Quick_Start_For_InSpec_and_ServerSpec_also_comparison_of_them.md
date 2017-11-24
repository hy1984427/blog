title: Quick start for InSpec and ServerSpec also comparison of them
date: 2017-11-24
categories:
- Testing
tags:
- Infrastructure Testing
- InSpec
- ServerSpec
- Ruby
- Quick-Start
- Comparison
comments: true
---
##### InSpec and ServerSpec are Infrastructure Testing tools based on Ruby. InSpec is newly added into [ThoughtWorks Tech Radar](https://www.thoughtworks.com/radar).

### Prerequisite:

  RVM, Ruby (>2.2) and rubygems should be installed.

### Get Started for InSpec:

1. Install InSpec.

  `gem install inspec`

2. Write and run the InSpec script according to its [API document](https://www.inspec.io/docs/reference/resources/).

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/sample_inspec.rb).

  Using `inspec exec inspec.rb` to run and check the result.

  As you can see the script is pretty much the same as the one we used for ServerSpec, you can reference that script [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/ServerSpec.zip). The instruction of running it is in [this article](http://hy1984427.github.io/2017/09/01/Qucik_Start_For_ServerSpec_and_TestInfra_also_comparison_of_them/).

  And there is [another official article](https://www.inspec.io/docs/reference/migration/) talking about the migration from ServerSpec to InSpec (we can also see the differences of resources between the two).

3. To generate a json file as the test result, we run `inspec exec sample_inspec.rb --format json >report`.

  And it will generate the test result named "report" every time the test runs.

### Comparison between InSpec and ServerSpec:

  * InSpec has 98 types of resources but ServerSpec has only 41.

  * InSpec has more comprehensive documents, you can reference [here](https://www.inspec.io/docs/), and it even has a series of [detailed tutorials](https://www.inspec.io/tutorials/).

In general, I would suggest to use InSpec for Infrastructure Testing in new projects.
