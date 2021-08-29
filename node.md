type => module in package.json
### Require in node
We have to tell node that we want to access to each of it's c++ features or code written as modules by other developers. We get a built in function to do this, require.

For built in modules
```js
const http = require('http')
```
For npm packages
```js
const express = require('express')
```
for our own modules, we use relative path
```js
const dom = require('./../dom.js')
```


#### Event emitters
[Read about event emitters here](https://nodejs.dev/learn/the-nodejs-event-emitter)
