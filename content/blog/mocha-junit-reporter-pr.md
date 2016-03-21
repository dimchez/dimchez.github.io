+++
date = "2016-03-19T09:43:30+01:00"
draft = false
title = "mocha-junit-reporter pull request"

+++

As follow up to [my previous post](http://dimchez.com/blog/running-parallel-end-to-end-tests-in-nightwatchjs-with-mocha-junit-reporter/), I've added [PR](https://github.com/michaelleeallen/mocha-junit-reporter/pull/29) for `mocha-junit-reporter`. Hopefully it will be approved. Otherwise I can create a small wrapper around `mocha-junit-reporter` that allows writing test results in different files when executed in parallel. Basically, the only thing that should be modified is `filePath` argument in [writeXmlToDisk](https://github.com/michaelleeallen/mocha-junit-reporter/blob/master/index.js#L233) function. This should be a rather easy fix.

> UPDATE (21/03/2016): My PR was accepted and version `1.10.0` of `mocha-junit-reporter` was published. Hopefully during our next sprint I will get some time to setup our Selenium grid to run end-to-end tests in parallel.
