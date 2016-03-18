+++
date = "2016-03-18T10:18:43+01:00"
draft = true
title = "Running parallel end-to-end tests in NightwatchJS with mocha-junit-reporter"

+++

For implementation of our end-to-end tests for frontend applications at [Nordnet](https://www.nordnet.se) we rely on [NightwatchJS](https://nightwatchjs.org). Tests are executed against [Selenium Grid](https://github.com/SeleniumHQ/selenium/wiki/Grid2) which has several nodes running different browsers and operating systems. As the number of tests is growing in each project so does the time it takes to execute those tests. For some projects it might take from half an hour and up to an hour for tests to either succeed or fail. During that time project is stalled in CI environment and cannot be moved further in the delivery pipeline. One obvious solution to this problem was to try to run end-to-end tests in parallel.
