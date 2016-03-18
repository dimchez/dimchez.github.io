+++
date = "2016-03-18T10:18:43+01:00"
draft = true
title = "Running parallel end-to-end tests in NightwatchJS with mocha-junit-reporter"

+++

For implementation of our end-to-end tests for frontend applications at [Nordnet](https://www.nordnet.se) we rely on [NightwatchJS](https://nightwatchjs.org). Tests are executed against [Selenium Grid](https://github.com/SeleniumHQ/selenium/wiki/Grid2) which has several nodes running different browsers and operating systems. As the number of tests is growing in each project so does the time it takes to execute those tests. For some projects it might take from half an hour and up to an hour for tests to either succeed or fail. During that time project is stalled in CI environment and cannot be moved further in the delivery pipeline. One obvious solution to this problem was to try to run end-to-end tests in parallel.

[NightwatchJS](https://nightwatchjs.org) allows parallel execution of tests in multiple environments (e.g. against different browsers), see [Parallel Running](http://nightwatchjs.org/guide#parallel-running) section in [NightwatchJS Developer Guide](http://nightwatchjs.org/guide). Though this is something we need to have as well, it won't increase speed of execution of tests in general. End-to-end tests will still be executed 30+ minutes, just in different browsers at the same time. Since `v0.7` however there is a new feature that allows the tests to be run in parallel. From [NightwatchJS Parallel Running](http://nightwatchjs.org/guide#parallel-running) documentation:

> ...the test runner will launch a configurable number of child processes and then distribute the loaded tests over to be ran in parallel.

> Test concurrency is done at the file level. Each test file will fill a test worker slot. Individual tests/steps in a test file will not run concurrently.

In the simplest case test workers can be enabled by adding the following in `nigthwatch.json` config

```
"test_workers": true
```

Number of test workers can also be configured. See [NightwatchJS docs](http://nightwatchjs.org/guide#parallel-running) for details on how to do it.

One obvious requirement for implementation of end-to-end tests for this feature to work efficiently is not to put all tests in one file, but to split them up in multiple test files. Then individual test files could be executed by separate test workers.

This looked promising and I've started testing this feature on one of our projects. An unfortunate outcome of this testing was that we can't use this feature out-of-the-box with the way our tests are setup, executed and how test results are processed. The problem is that we've decided to run NightwatchJS tests [using Mocha](http://nightwatchjs.org/guide#using-mocha). Decision was based on a fact that we are using [Mocha](http://mochajs.org) to structure our unit tests and it would be good to have same structure in our end-to-end tests as well. For automated build and deployment processes we are using [Jenkins](https://jenkins-ci.org/). To know whether to fail a specific build or mark it as successful Jenkins checks generated JUnit XML test results reports. We are using [mocha-junit-reporter](https://github.com/michaelleeallen/mocha-junit-reporter) for generating XML reports for NightwatchJS tests. And this is where the problem is - test results reports are written in the same test-results.xml file. Essentially, the last test worker to complete overrides all test results of previous tests. This makes it impossible to track the status of end-to-end tests.

What can be done about it? Well, I'm working on PR for [mocha-junit-reporter](https://github.com/michaelleeallen/mocha-junit-reporter) to be able to specify a hash in a file name. That way each test worker would write test results in its own XML file and then Jenkins would have to evaluate multiple files. Another alternative is to detect that file already exists and append test results instead of overriding file contents. That would be a better option in case Jenkins (or Jenkins JUnit plugin) can't process multiple test results files.

Stay tuned for more updates on the topic!
