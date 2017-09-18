title: Configure ServerSpec to run tests against multiple hosts
date: 2017-09-18
categories:
- Testing
tags:
- Infrastructure Testing
- ServerSpec
- Ruby
- Advanced
- Multiple Hosts
comments: true
---
##### We have learnt how to setup tests with ServerSpec in [Quick start for ServerSpec and Testinfra, also comparison of them](http://hy1984427.github.io/2017/09/01/Qucik_Start_For_ServerSpec_and_TestInfra_also_comparison_of_them/), but the folder structure can only support testing against one host. In the real world, we need to reuse the tests to test against multiple hosts, so we will see how we can do that.

### Prerequisite:

  ServerSpec setup is completed according to the steps in [Quick start for ServerSpec and Testinfra, also comparison of them](http://hy1984427.github.io/2017/09/01/Qucik_Start_For_ServerSpec_and_TestInfra_also_comparison_of_them/).

### Configure ServerSpec to run tests against multiple hosts:

  Actually we only need to change the Rakefile in the test folder. And its content should be changed to following:

  ```
  require 'rake'
  require 'rspec/core/rake_task'

  hosts = %w(
    host1
    host2
  )

  task :spec => 'spec:all'

  namespace :spec do

    task :all => hosts.map {|h| 'spec:' + h }
    hosts.each do |host|
      desc "Run serverspec to #{host}"
      RSpec::Core::RakeTask.new(host) do |t|
        ENV['TARGET_HOST'] = host
        t.pattern = "spec/{host_server}/*_spec.rb"
      end
    end

  end
  ```

  Please note, `host1` and `host2` are the hosts we would like to run our tests against, the reason we put `host_server` in above lines is because we have the folder named "host_server" in our test scripts structure.

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/ServerSpecAdvanced.zip).
