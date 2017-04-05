Node.js - hjsonfile
================

Easily read/write HJSON files. Based on [jsonfile](https://www.npmjs.com/package/jsonfile)

<a href="https://github.com/feross/standard"><img src="https://cdn.rawgit.com/feross/standard/master/sticker.svg" alt="Standard JavaScript" width="100"></a>

Why?
----

Writing `JSON.stringify()` and then `fs.writeFile()` and `JSON.parse()` with `fs.readFile()` enclosed in `try/catch` blocks became annoying.



Installation
------------

    npm install --save hjsonfile



API
---

### readFile(filename, [options], callback)

`options` (`object`, default `undefined`): Pass in any `fs.readFile` options or set `reviver` for a [JSON reviver](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse).
  - `throws` (`boolean`, default: `true`). If `JSON.parse` throws an error, pass this error to the callback.
  If `false`, returns `null` for the object.


```js
var jsonfile = require('hjsonfile')
var file = '/tmp/data.json'
jsonfile.readFile(file, function(err, obj) {
  console.dir(obj)
})
```


### readFileSync(filename, [options])

`options` (`object`, default `undefined`): Pass in any `fs.readFileSync` options or set `reviver` for a [JSON reviver](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse). 
- `throws` (`boolean`, default: `true`). If `JSON.parse` throws an error, throw the error.
If `false`, returns `null` for the object.

```js
var jsonfile = require('hjsonfile')
var file = '/tmp/data.json'

console.dir(jsonfile.readFileSync(file))
```


### writeFile(filename, obj, [options], callback)

`options`: Pass in any `fs.writeFile` options or set `replacer` for a [JSON replacer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Can also pass in `spaces`.


```js
var jsonfile = require('hjsonfile')

var file = '/tmp/data.json'
var obj = {name: 'JP'}

jsonfile.writeFile(file, obj, function (err) {
  console.error(err)
})
```

**formatting with spaces:**

```js
var jsonfile = require('hjsonfile')

var file = '/tmp/data.json'
var obj = {name: 'JP'}

jsonfile.writeFile(file, obj, {spaces: 2}, function(err) {
  console.error(err)
})
```

**appending to an existing JSON file:**

You can use `fs.writeFile` option `{flag: 'a'}` to achieve this.

```js
var jsonfile = require('hjsonfile')

var file = '/tmp/mayAlreadyExistedData.json'
var obj = {name: 'JP'}

jsonfile.writeFile(file, obj, {flag: 'a'}, function (err) {
  console.error(err)
})
```

### writeFileSync(filename, obj, [options])

`options`: Pass in any `fs.writeFileSync` options or set `replacer` for a [JSON replacer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). Can also pass in `spaces`.

```js
var jsonfile = require('hjsonfile')

var file = '/tmp/data.json'
var obj = {name: 'JP'}

jsonfile.writeFileSync(file, obj)
```

**formatting with spaces:**

```js
var jsonfile = require('hjsonfile')

var file = '/tmp/data.json'
var obj = {name: 'JP'}

jsonfile.writeFileSync(file, obj, {spaces: 2})
```

**appending to an existing JSON file:**

You can use `fs.writeFileSync` option `{flag: 'a'}` to achieve this.

```js
var jsonfile = require('hjsonfile')

var file = '/tmp/mayAlreadyExistedData.json'
var obj = {name: 'JP'}

jsonfile.writeFileSync(file, obj, {flag: 'a'})
```

### spaces

Global configuration to set spaces to indent JSON files.

**default:** `null`

```js
var jsonfile = require('hjsonfile')

jsonfile.spaces = 4

var file = '/tmp/data.json'
var obj = {name: 'JP'}

// json file has four space indenting now
jsonfile.writeFile(file, obj, function (err) {
  console.error(err)
})
```

Note, it's bound to `this.spaces`. So, if you do this:

```js
var myObj = {}
myObj.writeJsonSync = jsonfile.writeFileSync
// => this.spaces = null
```

Could do the following:

```js
var jsonfile = require('hjsonfile')
jsonfile.spaces = 4
jsonfile.writeFileSync(file, obj) // will have 4 spaces indentation

var myCrazyObj = {spaces: 32}
myCrazyObj.writeJsonSync = jsonfile.writeFileSync
myCrazyObj.writeJsonSync(file, obj) // will have 32 space indentation
myCrazyObj.writeJsonSync(file, obj, {spaces: 2}) // will have only 2
```

