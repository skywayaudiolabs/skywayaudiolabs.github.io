---
layout: post
title: IMOW - Promises (in coffeescript)
---
![image]({{ site.baseurl }}/images/promises.png)

Some useful articles

* http://www.2ality.com/2014/10/es6-promises-api.html
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
* http://www.html5rocks.com/en/tutorials/es6/promises/
* http://javascriptplayground.com/blog/2015/02/promises/


A simple example that reads in the contents of a file using async `fs.readfile` and prints to console.

```coffee
fs = require 'fs'
{Promise} = require 'es6-promise'
```

Import `Promise` (using destructured assignment) from the [`es6-promise`](https://github.com/jakearchibald/es6-promise) module.

```coffee
readFile = (file) ->
  new Promise (resolve, reject) ->
    fs.readFile file, (err, data) ->
      if data?
        resolve data.toString()
      else
        reject err
```

Create a function with a new Promise object. The object returns two methods - resolve (call with data) and reject (call with an error). Use `fs.readFile`'s callback to return data, by calling `resolve data` and for errors, `reject err`.
To run the function once...

```coffee
readFile 'test.txt'
.then (response) ->
  console.log response
.catch (err) ->
  console.log err
```

The Promise.then method will be called when fs.readFile calls `resolve` method (with data as a parameter) and the Promise.catch method is invoked when `reject` is called (with err as a parameter).

Let's extend this example to a list of files where the `Promise.all` method runs when all of the Promises are completed.

```coffee
files = ['test.txt', 'test2.txt', 'test3.txt']
allReadFile = files.map readFile

Promise.all(allReadFile)
.then (response) ->
  console.log response
.catch (err) ->
  console.log err
```

In this case, the `Promise.all` method is called when all of the Promise objects have been resolved. A `reject` on any of the promises will trigger `.catch` and exit as well.

Functions can be chained together with `.then`

```coffee
myReadFile 'test.txt'
.then (response) ->
  console.log "myReadFile is complete, now calling func2"
  func2 response  # call func2
.then (response) ->
  console.log response
  func3 response  # call jump function
.then (response) ->
  console.log response
.catch (err) ->
  console.log err
```

The return value is passed from one function to the next. The functions can be synchronous or async using promises.

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
