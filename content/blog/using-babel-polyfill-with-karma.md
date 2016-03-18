+++
date = "2016-03-17T20:41:55+01:00"
draft = false
title = "Using babel-polyfill with Karma, Webpack and ES6"

+++

# Enabling babel-polyfill when running unit tests using Karma, PhantomJS, Webpack and ES6

Unit tests started failing when [lodash's assign](https://lodash.com/docs#assign) was replaced with [Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign).

```
'undefined' is not a function (evaluating 'Object.assign(...)
```

Tests are bundled with Webpack first and then executed using Karma as test runner and PhantomJS.

Adding the following in `karma.config.js` solved the problem:

```
files: [
  'node_modules/babel-polyfill/dist/polyfill.js',
  ...
]
```

Hint on how to fix this issue was in [karma-babel-preprocessor](https://github.com/babel/karma-babel-preprocessor#polyfill) plugin documentation. Polyfill seems to work fine even though plugin itself is not used in the project.
