title: Quick start for Mocha + Chai + SuperTest in API Testing with Tips and Mock Server (moco)
date: 2017-07-14
categories:
- Testing
tags:
- API
- Automation
- Mocha
- Chai
- Supertest
- Mock Server
- moco
- Quick-Start
comments: true
---
##### There are many combinations to test APIs, but here we choose a commonly used combination for demonstration, the rests will be pretty much the same or likely. And we will use moco to quickly simulate a server.

### Environment Settings:

1. Install Homebrew

 `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

2. Install NodeJS

  `brew install node`

3. Install Mocha, Chai, SuperTest, Exoress and Body-Parser

  ```
  npm install -g mocha
  npm install chai
  npm install supertest
  npm install express
  npm install body-parser
  ```

4. Download the moco standalone file from [Github](http://central.maven.org/maven2/com/github/dreamhead/moco-runner/0.11.1/moco-runner-0.11.1-standalone.jar)

5. Since Mocha will only run scripts under "test" folder, so we have to create a folder named "test".

### Start a simple server and test on it:

1. Create a json file named "user1.json", and add following content to it:

  ```
  [
    {
    "response" :
      {
        "status" : "200"
      }
    }
  ]
  ```

  *Please note: we only define the HTTP status code here for now.*

2. Start the Mock server using the command when under the "test" folder:

  `java -jar moco-runner-0.11.1-standalone.jar http -p 3000 -c user1.json`

  And you should see following lines in your Terminal:

  ```
  10 Jul 2017 16:17:43 [main] INFO  Server is started at 3000
  10 Jul 2017 16:17:43 [main] INFO  Shutdown port is 56123
  10 Jul 2017 16:17:45 [nioEventLoopGroup-3-2] INFO  Request received:
  ```
3. You can open the URL http://localhost:3000/ in browser to check the response of the API, but currently since we havn't set and response value except the status code, so you won't see anything in the browser.

4. Create a script file named "user1_test.js", and add following to it:

  1) Add the alias to shorten the names for requiring libs:

  ```
  var should = require('chai').should(),
  expect = require('chai').expect,
  supertest = require('supertest'),
  ```
  2) Setup the URL we want to test against:

  `api = supertest('http://localhost:3000');`

  3) Define a function to describe the feature we want to test:

  ```
  describe('User', function() {

  });
  ```

  4) Define a detailed test step to execute the test script, by adding it in the content of function above:

  ```
  it('should return a response with HTTP code 200', function(done) {
    api.get('').expect(200, done);
  });
  ```

5. Run the test to check the result by using:

  `mocha` or `mocha user1_test.js`

  And you should see the result like:

  ```
  User
    ✓ should return a response with HTTP code 200

  1 passing (28ms)
  ```

  Great! It works.

6. Now, let's make sure it works as expected by change the expected HTTP code to 404, and run it again. You should see the following:

  ```
  User
    1) should return a response with HTTP code 200

  0 passing (32ms)
  1 failing

  1) User should return a response with HTTP code 200:
     Error: expected 404 "Not Found", got 200 "OK"
  ```

  Awesome! It means the test really works. Now, revert the change.

7. It's time to add more parameters to make it a little complex.

  1) Add the formatter of HTTP request header in test script, and change the line of API test to following:

  `api.get('').set('Accept', 'application/json').expect(200, done);`

  2) Change the response of server to point to a specific URL, by adding the URI above the HTTP status code in "user1.json", and the entire file should looks like:

  ```
  [
    {
    "request" :
      {
        "uri" : "/users/1"
      },
    "response" :
      {
        "status" : "200"
      }
    }
  ]
  ```

  *Please note: don't bother to restart the Mock Server. Once you save the json file with correct format moco provided, it will restart itself automatically. But if you save with any error, you have to fix it before start it, otherwise it won't response correctly. For detailed moco usage and API docuemnts, please refer to the Documents section on [moco Github page](https://github.com/dreamhead/moco).*

  3) Now, it's time to change the test script accordingly. We need to change the line of API test to following:

  `api.get('/users/1').set('Accept', 'application/json').expect(200, done);`

  4) Let's run the test script again, you should see the test pass:

  ```
  User
  ✓ should return a response with HTTP code 200 (177ms)

  1 passing (186ms)
  ```

  5) We can also add some text response into API response, like following:

  ```
  [
    {
    "request" :
      {
        "uri" : "/users/1"
      },
    "response" :
      {
        "status" : "200",
        "json":
        {
          "text": "Not Empty"
        }
      }
    }
  ]
  ```

  6) Then change the test script correspondingly:

  ```
  it('should return a response with HTTP code 200 and the response text should be "Not Empty"', function(done) {
    api.get('/users/1').set('Accept', 'application/json').expect(200).end(function(err,res){
      expect(res.body).to.have.property("text");
      expect(res.body.text).to.equal("Not Empty");
      done(err);
    });
  });
  ```

  The test result should show "Pass".

  7) What if we change the expected HTTP status code ot the expected response text? Let's try.

  After change the expected HTTP status code to 500, once the test run, the result will be:

  ```
  User
    1) should return a response with HTTP code 200 and the response text should be "Not Empty"

  0 passing (44ms)
  1 failing

  1) User should return a response with HTTP code 200 and the response text should be "Not Empty":
     Error: expected 500 "Internal Server Error", got 200 "OK"
  ```

  Revert the change and change the expected response text to "Empty", once the test run, the result will be:

  ```
  User
    1) should return a response with HTTP code 200 and the response text should be "Not Empty"

  0 passing (46ms)
  1 failing

  1) User should return a response with HTTP code 200 and the response text should be "Not Empty":

      Uncaught AssertionError: expected 'Not Empty' to equal 'Empty'
      + expected - actual

      -Not Empty
      +Empty
  ```

    Don't forget to revert the change.

  8) The test script works as expected, so next, we will make it more complex.

### Make the server more complex and of course, test against it:

1. Append the API response to only respond to specific request, change the API response to:

  ```
  [
    {
    "request" :
      {
        "uri" : "/users/1",
        "json": {
          "name": "Yale"
        }
      },
    "response" :
      {
        "status" : "200",
        "json":
        {
          "gender": "Male"
        }
      }
    }
  ]
  ```

2. Change the test script to reflect the changes:

  ```
  it('should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"', function(done) {
    api.get('/users/1').set('Accept', 'application/json').send({name:"Yale"}).expect(200).end(function(err,res){
      expect(res.body).to.have.property("gender");
      expect(res.body.gender).to.equal("Male");
      done(err);
    });
  });
  ```

3. Once run the test script, it will Pass.

4. If we change the value in the request, like change "Yale" to "Yong", once test script runs, it will show:

  ```
  User
      1) should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"

    0 passing (47ms)
    1 failing

    1) User should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name":
       Uncaught AssertionError: expected {} to have property 'gender'
  ```

5. If we change the value in the request, like change the 2 "gender" to "sex", once test script runs, it will show:

  ```
  User
    1) should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"

  0 passing (39ms)
  1 failing

  1) User should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name":
     Uncaught AssertionError: expected {} to have property 'sex'
  ```

6. If we change the value in the request, like change "Male" to "Man", once test script runs, it will show:

  ```
  User
    1) should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"

  0 passing (44ms)
  1 failing

  1) User should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name":
     Uncaught AssertionError: expected {} to have property 'gender'
  ```

7. You may notice that in the test script, we still use GET method rather than PUT or POST, but we still can let the test Pass. Now, let's strict it. In the API response, we need to change it to following:

  ```
  [
    {
    "request" :
      {
        "uri" : "/users/1",
        "method": "get",
        "json": {
          "name": "Yale"
        }
      },
    "response" :
      {
        "status" : "500",
        "json":
        {
          "gender": "Male"
        }
      }
    },
    {
    "request" :
      {
        "uri" : "/users/1",
        "method": "put",
        "json": {
          "name": "Yale"
        }
      },
    "response" :
      {
        "status" : "200",
        "json":
        {
          "gender": "Male"
        }
      }
    }
  ]
  ```

8. Simply change the HTTP method to "put" in test script and check the result:

  In the test script:

  ```
  api.put('/users/1').set('Accept', 'application/json').send({name:"Yale"}).expect(200).end(function(err,res){
    expect(res.body).to.have.property("gender");
    expect(res.body.gender).to.equal("Male");
    done(err);
    });
  });
  ```

  And the test result:

  ```
  User
    ✓ should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"

  1 passing (41ms)
  ```

9. Let's see what if we change the HTTP method to "get" again in test script and check the result:

  In the test script:

  ```
  api.get('/users/1').set('Accept', 'application/json').send({name:"Yale"}).expect(200).end(function(err,res){
    expect(res.body).to.have.property("gender");
    expect(res.body.gender).to.equal("Male");
    done(err);
    });
  });
  ```

  And the test result:

  ```
  User
    1) should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"

  0 passing (39ms)
  1 failing

  1) User should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name":
     Error: expected 200 "OK", got 500 "Internal Server Error"
  ```

### Final stage - more advanced usage:

1. If we want to add more API responses, do we need to add them into one json file? Certainly not, let's see how we can do that.

  1) Create another json file named "user2", and add following content to it:

  ```
  [
    {
    "request" :
      {
        "uri" : "/users/2",
        "method": "post",
        "json": {
          "name": "Yong"
        }
      },
    "response" :
      {
        "status" : "500",
        "text" : "Internal Server Error"
      }
    }
  ]
  ```

  2) Create another js file named "user2_test.js", and add following into its content:

  ```
  var should = require('chai').should(),
    expect = require('chai').expect,
    supertest = require('supertest'),
    api = supertest('http://localhost:3000');
  describe('User', function() {
    it('should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name"', function(done) {
      api.post('/users/2').set('Accept', 'application/json').send({name:"Yong"}).expect(500).end(function(err,res){
        if (err) return done(err);
        expect(res.error.text).to.equal("Internal Server Error");
        done(err);
      });
    });
  });
  ```

  3) Once you run the test script, it will show:

  ```
  User
      ✓ should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name"

    1 passing (41ms)
  ```

  4) If we only change the expected HTTP status code in the test script to 404, the result will be:

  ```
  User
  1) should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name"

  0 passing (45ms)
  1 failing

  1) User should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name":
    Error: expected 404 "Not Found", got 500 "Internal Server Error"
  ```

  5) And if we only change the expected response message to "Not Found", the result will be:

  ```
  User
  1) should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name"

  0 passing (43ms)
  1 failing

  1) User should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name":

      Uncaught AssertionError: expected 'Internal Server Error' to equal 'Not Found'
      + expected - actual

      -Internal Server Error
      +Not Found
  ```

  6) Revert all changes back again, and create another json file named "combined.json", and add following:

  ```
  [
    {
      "include": "user1.json"
    },
    {
      "include": "user2.json"
    }
  ]
  ```

    And start the Mock Server using `java -jar moco-runner-0.11.1-standalone.jar http -p 3000 -g combined.json`

  7) Use Mocha to run all the test scripts:

  `mocha *.js`

  And you should see:

  ```
  User
      ✓ should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"

    User
      ✓ should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name"

    2 passing (49ms)
  ```

  8) If you want to run a single test script to test all steps, you can create a js file named "users_test.js", and add following content:

  ```
  var should = require('chai').should(),
    expect = require('chai').expect,
    supertest = require('supertest'),
    api = supertest('http://localhost:3000');
  describe('User', function() {
    it('should return a response with HTTP code 200, and the response text should be "Male" if request the gender of "Yale" as "name"', function(done) {
      api.put('/users/1').set('Accept', 'application/json').send({name:"Yale"}).expect(200).end(function(err,res){
        expect(res.body).to.have.property("gender");
        expect(res.body.gender).to.equal("Male");
        done(err);
      });
    });

    it('should return a response with HTTP code 500 and its content if request the gender of "Yong" as "name"', function(done) {
      api.post('/users/2').set('Accept', 'application/json').send({name:"Yong"}).expect(500).end(function(err,res){
        if (err) return done(err);
        expect(res.error.text).to.equal("Internal Server Error");
        done(err);
      });
    });
  });
  ```

  9) Run the test using `mocha users_test.js`, it will show the same result as above.

### Later on:

Since you already can run your test scripts smoothly through command line, you can simply add them to CI tools, such as Jenkins and such.

Have fun!

***You can find all the test scripts from [here](https://raw.githubusercontent.com/hy1984427/hy1984427.github.io/master/documents/API_Test_Mocha_Chai_SuperTest.zip).***
