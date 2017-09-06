title: Quick start for integrating ZAP into CI
date: 2017-09-06
categories:
- Testing
tags:
- Security
- ZAP
- CI
- Ruby
- Quick-Start
comments: true
---
##### OWASP ZAP is widely used security testing tool and it's open sourced. Let's see how we can integrate it with our automated functionality testing in CI.

### Prerequisite:

  RVM, Ruby and rubygems are installed.

### Get Started:

1. Download ZAP from [OWASP_Zed_Attack_Proxy_Project](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project) and install it.

2. Install Selenium-WebDriver, IO, Rest-Client and RSpec gems:

  1) Install Selenium-WebDriver

   `gem install selenium-webdriver`

  2) Install IO

   `gem install io`

  3) Install Rest-Client

   `gem install rest-client`

  4) Install RSpec

   `gem install rspec`

3. Write and run a basic Selenium-WebDriver script using Ruby. The script is the same as in my previous Gitbook [BDD with PageObject](http://hy1984427.github.io/BDD-with-PageObject/SimpleWebDriverTestByRuby/SimpleWebDriverTestByRuby.html).

  The script will be like following:

  ```
  require 'selenium-webdriver'

  driver = Selenium::WebDriver.for :firefox

  driver.get "http://www.google.com"
  element = driver.find_element :name => "q"
  element.send_keys "Cheese!"
  element.submit
  p "Page title is #{driver.title}"
  wait = Selenium::WebDriver::Wait.new(:timeout => 10)
  wait.until { driver.title.downcase.start_with? "cheese!" }
  p "Page title is #{driver.title}"

  driver.quit
  ```

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/simple_script.rb).

  Using `ruby simple_script.rb` to run and check the result.

4. Add steps to start ZAP and proxy the testing script using ZAP.

  The script will become as following:

  ```diff
  require 'selenium-webdriver'
  + require 'io/console'

  + system("pkill java") #To close any existing ZAP instance.
  + system("pkill firefox") #To close any existing Firefox instance.
  + IO.popen("/Applications/ZAP\\ 2.6.0.app/Contents/Java/zap.sh -daemon -config api.disablekey=true") #The path here should be the zap.sh path under ZAP package/folder on your machine; with the option -config api.disablekey=true, ZAP will not check the apikey, which is enable by default after ZAP 2.6.0
  + p "OWASP ZAP launch completed"
  + sleep 5 #To let ZAP start completely

  + profile = Selenium::WebDriver::Firefox::Profile.new
  + proxy = Selenium::WebDriver::Proxy.new(http: "localhost:8080") #Normally ZAP will listening at port 8080, if not, please change it to the actual port ZAP is listening
  + profile.proxy = proxy
  + options = Selenium::WebDriver::Firefox::Options.new(profile: profile)
  + driver = Selenium::WebDriver.for :firefox, options: options
  - driver = Selenium::WebDriver.for :firefox

  driver.get "http://www.google.com"
  element = driver.find_element :name => "q"
  element.send_keys "Cheese!"
  element.submit
  p "Page title is #{driver.title}"
  wait = Selenium::WebDriver::Wait.new(:timeout => 10)
  wait.until { driver.title.downcase.start_with? "cheese!" }
  p "Page title is #{driver.title}"

  driver.quit
  ```

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/add_zap_start.rb).

  Using `ruby add_zap_start.rb` to run and check the result.

5. Read the test results from ZAP.

  The script will become as following:

  ```diff
  require 'selenium-webdriver'
  require 'io/console'
  + require 'rest-client'

  system("pkill java") #To close any existing ZAP instance.
  system("pkill firefox") #To close any existing Firefox instance.
  IO.popen("/Applications/ZAP\\ 2.6.0.app/Contents/Java/zap.sh -daemon -config api.disablekey=true") #The path here should be the zap.sh path under ZAP package/folder on your machine; with the option -config api.disablekey=true, ZAP will not check the apikey, which is enable by default after ZAP 2.6.0
  p "OWASP ZAP launch completed"
  sleep 5 #To let ZAP start completely

  profile = Selenium::WebDriver::Firefox::Profile.new
  proxy = Selenium::WebDriver::Proxy.new(http: "localhost:8080") #Normally ZAP will listening at port 8080, if not, please change it to the actual port ZAP is listening
  profile.proxy = proxy
  options = Selenium::WebDriver::Firefox::Options.new(profile: profile)
  driver = Selenium::WebDriver.for :firefox, options: options

  driver.get "http://www.google.com"
  element = driver.find_element :name => "q"
  element.send_keys "Cheese!"
  element.submit
  p "Page title is #{driver.title}"
  wait = Selenium::WebDriver::Wait.new(:timeout => 10)
  wait.until { driver.title.downcase.start_with? "cheese!" }
  p "Page title is #{driver.title}"

  + JSON.parse RestClient.get "http://localhost:8080/json/core/view/alerts" #To trigger ZAP to raise alerts if any
  + sleep 5 #Give ZAP some time to process
  + response = JSON.parse RestClient.get "http://localhost:8080/json/core/view/alerts", params: { zapapiformat: 'JSON', baseurl: "http://clients1.google.com", start: 1 } #Get the alerts ZAP found, please note the baseurl will exact match from the beginning

  driver.quit
  + RestClient.get "http://localhost:8080/JSON/core/action/shutdown" #Close ZAP instance
  ```

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/read_zap_result.rb).

  Using `ruby read_zap_result.rb` to run and check the result.

6. Set up assertions to check the Low Risks.

  The script will become as following:

  ```diff
  require 'selenium-webdriver'
  require 'io/console'
  require 'rest-client'
  + require 'rspec/expectations'
  + include RSpec::Matchers

  system("pkill java") #To close any existing ZAP instance.
  system("pkill firefox") #To close any existing Firefox instance.
  IO.popen("/Applications/ZAP\\ 2.6.0.app/Contents/Java/zap.sh -daemon -config api.disablekey=true") #The path here should be the zap.sh path under ZAP package/folder on your machine; with the option -config api.disablekey=true, ZAP will not check the apikey, which is enable by default after ZAP 2.6.0
  p "OWASP ZAP launch completed"
  sleep 5 #To let ZAP start completely

  profile = Selenium::WebDriver::Firefox::Profile.new
  proxy = Selenium::WebDriver::Proxy.new(http: "localhost:8080") #Normally ZAP will listening at port 8080, if not, please change it to the actual port ZAP is listening
  profile.proxy = proxy
  options = Selenium::WebDriver::Firefox::Options.new(profile: profile)
  driver = Selenium::WebDriver.for :firefox, options: options

  driver.get "http://www.google.com"
  element = driver.find_element :name => "q"
  element.send_keys "Cheese!"
  element.submit
  p "Page title is #{driver.title}"
  wait = Selenium::WebDriver::Wait.new(:timeout => 10)
  wait.until { driver.title.downcase.start_with? "cheese!" }
  p "Page title is #{driver.title}"

  JSON.parse RestClient.get "http://localhost:8080/json/core/view/alerts" #To trigger ZAP to raise alerts if any
  sleep 5 #Give ZAP some time to process
  response = JSON.parse RestClient.get "http://localhost:8080/json/core/view/alerts", params: { zapapiformat: 'JSON', baseurl: "http://clients1.google.com", start: 1 } #Get the alerts ZAP found
  + response['alerts'].each {|x| p "#{x['alert']} risk level: #{x['risk']}"} #Extract the risks found
  + events = response['alerts']
  + low_count = events.select{|x| x['risk'] == 'Low'}.size #Count the Low Risks
  + expect(low_count).to equal(1) #Expecxt only one Low Risk

  driver.quit
  RestClient.get "http://localhost:8080/JSON/core/action/shutdown" #Close ZAP instance
  ```

  You can reference the script  [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/add_assertions_to_check_zap_result.rb).

  Using `ruby add_assertions_to_check_zap_result.rb` to run and check the result.

7. Now we can trigger this script in any CI using command above.
