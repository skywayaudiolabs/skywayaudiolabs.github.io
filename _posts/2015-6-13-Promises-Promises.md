i---
layout: post
title: promises, promises
categories: []
tags: []
published: True

---

OMG - I think I am grokking it!!! Very useful articles

* http://www.2ality.com/2014/10/es6-promises-api.html
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
* http://www.html5rocks.com/en/tutorials/es6/promises/


Ok, I did a simple little example. In this example, I am going to read in the contents of a file using async `fs.readfile`.

```coffee
fs = require 'fs'
{Promise} = require 'es6-promise'
```

Ok, so we are using the `es6-promise` module - we need to import `Promise` (which we are doing with destructured assignement).

```coffee
myReadFile = (file) ->
  new Promise (resolve, reject) ->
    fs.readFile file, (err, data) ->
      if data?
        resolve data.toString()
      else
        reject err
```

Create a function with a new Promise object. The object returns two methods to use - resolve (call with data) and reject (call with an error). To passback data, call `resolve data`. Similarly, for errors, `reject err`.

To run the function once...

```coffee
myReadFile 'test.txt'
.then (response) ->
  console.log response
.catch (err) ->
  console.log err
```

The Promise.then method will be called when fs.readFile calls `resolve` method (with data as a parameter) and the Promise.catch method is invoked when `reject` is called (with err as a paramter).

Let's extend this example to a list of files where the `Promise.all` method will run when all of the Promises are completed.

```coffee
files2 = ['test.txt', 'test2.txt', 'test3.txt']
allresponses = files.map myReadFile

Promise.all(allresponses)
.then (response) ->
  console.log response
.catch (err) ->
  console.log err
```

In this case, the `Promise.all` method is called when all of the Promise objects have been resovled.

Ok, let's chain some functions together.

```coffee
myReadFile 'test.txt'
.then (response) ->
  console.log 'single run test with chainables'
  console.log response
  # return response
  print response  # call print function
.then (response) ->
  console.log response
  jump response  # call jump function
.then (response) ->
  console.log response
.catch (err) ->
  console.log err
```

This is the most simple promise

```coffee
theMostSimplePromise = (phrase) ->
  new Promise (resolve) ->
    resolve phrase

theMostSimplePromise('print this')
.then (response) ->
  console.log response
```


when you create a new Promise, the constructor returns two functions, resolve and reject. The object exposes several methods to call, .then, .catch, .all, .race.
