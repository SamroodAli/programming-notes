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

### Who creates a thread ?
Some node features (IO) , libuv will handle it by creating a persisting thread to execute that task (like fs tasks). Some tasks libuv and node rely on the operating system to handle it. Like keeping the socket open and listening for inbound requests. Libuv and node only sets it up, the operating system will create the thread waiting requests.


### FS

#### Reading a file

We can use the buit in fs module's readFile function which takes in a path and a function to excute on completion of reading the file. The function will be auto run later on completion with two arguments, error data and the data read. If it was a successful read, the first argument in place of error data will be `null`. This is called error first pattern of node.
fs.readFile does not return anything.


The data returned from reading a file is in buffer format which is a pieces of zeros and ones (representation of the  file). It can be update it and we can take out pieces or add more pieces to it

```js
fs.readFile('./pathToFile',function(err,data){// handle error or data})

```

Streams in node
Instead of deadling with data at once, we can deal with it (read/write) in streams.(actually chunks of data).
To transform data while reading data, we can use `fs` module's `createReadStream` function to read a file in chunks (by default a chunk is 64kb (known as it's high watermark)).
First we create en event emitter that can subscribe to 'data' and 'end' events. 'data' events are fired for every chunk and the 'end' event is emitted at the end of reading the file. 
The 'close' event also works( like 'end' event).

```js
//import fs
const fs = require('fs')
// create the stream emitter
const streamEmitter = fs.createReadStream('./pathToFile')
//function to do on each chunk
function doONChunk(chunk){
  //handle chunk  (in Buffer format)
}
// subscribe to every chunk
streamEmitter.on('data',doOnChunk)

//function to run on end
function onEnd(){}
//subscribe to end,if needed
streamEmitter.end('end',onEnd) // close instead of end also work.
```
