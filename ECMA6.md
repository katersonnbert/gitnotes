ECMA Script 6
=============

## Difference between `var` and `let`

[Difference between let and var](
http://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var-to-declare-a-variable#11444416)


## Link list:
- [ECMA 6 Language Specification](http://ecma-international.org/ecma-262/6.0/)
- [JavaScript Reference from Mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
- [ECMA Script 6](http://es6-features.org/)
- [ES6 Hacks](https://hacks.mozilla.org/category/es6-in-depth/)
- [jsfiddle playground](https://jsfiddle.net/)
- Article about [problems with promises](https://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html?utm_source=javascriptweekly)
- Article about [iterators and generators](http://macr.ae/article/iterators-and-generators.html)


### JavaScript linters
- [ESlint](http://eslint.org/docs/user-guide/configuring)
- [JSHint](http://jshint.com/docs/)
- [Closure compiler](https://developers.google.com/closure/compiler/?csw=1)


## TypeScript
Think about using [TypeScript](http://www.typescriptlang.org/docs/tutorial.html) instead of any JavaScript.

----------------------------------------

node.js and npm
===============

### Update packages of an npm project

`npm outdated` displays current package versions and newest available version.

### Linklist
- [npm kickstart](https://nodesource.com/blog/an-absolute-beginners-guide-to-using-npm)
- [nodejs package json tutorial](https://dzone.com/articles/the-basics-of-packagejson-in-nodejs-and-npm)


npm and testing with karma
==========================

- [Vuejs testing](https://vuejs.org/v2/guide/unit-testing.html).
- VueJS suggests using [Karma](http://karma-runner.github.io/0.12/index.html) for testing.
- [Jasmin](https://jasmine.github.io/) supposedly helps making Karma play nice [with Travis](
https://www.sitepoint.com/testing-javascript-jasmine-travis-karma/).
- Reading this might also help when [testing GUI stuff on Travis](https://docs.travis-ci.com/user/gui-and-headless-browsers/).
- [Here](https://github.com/apertureless/vue-chartjs) an example of a project that actually uses Vuejs unit testing with Travis.

- [This blog](http://www.bradoncode.com/blog/2015/02/27/karma-tutorial/) gives a decent introduction to setting up a 
test environment with Karma and Jasmin. One should use `npm outdated ` though to get the latest packages.
- After the first introduction [this blog](https://jaredtong.com/2016/01/08/how-to-set-up-mocha-chai-sinon-karma-browserify-istanbul-codecov/) 
shows how to use Mocha and Chai instead of Jasmin and introduces deployment to Travis and code coverage.


JQuery
======

##### Various notes

    $ ... returns JQuery object

e.g.

    $(document.getElementById('someId'))

