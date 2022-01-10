type => module in package.json
### Require in node
We have to tell node that we want to access to each of it's c++ features or code written as modules by other developers. We get a built in function to do this, require.

For built in modules
```js
const http = require('http')
```
For npm packagesg
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

Asynchronocity in node
Node has the usual callstack, callback queue and the event loop.
Node also has extra queues as well.



```js
function useImportedTweets(errorData,data){ //// Goes to the IO queue /// Ran later at 505ms 
  const tweets = JSON.parse(data)
  console.log(tweets.tweet1)
}

// Final queue => Check queue => After all IO is done => Ran later at 506 ms
function immediately(){console.log("Run me Last : )}


function printHello(){console.log("Hello")} /// Goes to timer queue // Ran later at 504ms (priority first queue)

function blockFor500ms(){ 
  //blocks for 500ms
}

setTimeout(printHello,0)    // 0ms
fs.readFile('./tweets.json',useImportedTweets)      // 1ms 
blockFor500ms // callstack 500ms                    // 2ms takes //took 500 ms
console.log("Me first")                             // 502ms // logs immediately (not in IO queue)
setImmediate(immediately)                           // 503ms // check queue after all IO (at that moment) has finished

```

no matter the queue, synchronous code gets run first. It is the event loop that takes and puts in queues. for settimeout, event loop checks if enough time has passed and if has, takes the function and puts it in the timer queue.

Timer (min heap) queue = > setTimeout ,setInterval etc features
IO => 95% => fs, network socket

check=>there are some functions that can only be run when the all IO queue at the check of the event loop has finished running. => put it in the `check` queue by setImmediate()


Promises => The Microtask queue (First priority (even before timer queue)) The microtask queue is checked in between checking every other queue.
  The microtask queue has two two queues
    a ) Process.nexttick() // not meant to use anymore
    b ) Promises

  Close queue => Final queue=> for functions executing close events => server.on('close'), fs.createReadStream().on('close') etc


```md
We can build our own loop
First check if there are auto run tasks
while(auto run tasks){
  check micro task queues // process.nexttick() and promises
  check timer queus       // setIntervals and setTimeouts
  check micro task queues 
  check io queue          // io tasks (not including console) fs, db, socket
  check micro task queues
  check check queue       // setImmediate
  check micro task queues
  check close queues      // 'close' event subscribers
}

else quit node
we can use the `queue` data structures or an array of functions
```

# Digging into node notes

POSIX -  A standard for I/O and system integration, system-level task calling
Essentially the way c style programs integrate with linux style operating system.
When we talk about posix, one of the major things we talk about is standard I/O subsystem.
Standard I/O is a set of three streams that model input and output to a program. 0 - stdin, 1 - stdout ,2-stderr
Node made the choise to expose most of the system connections through a posix like interface.
How do we acccess it ?
process

They hang a bunch off of process that are connections to your overall hosting system.

Javascript is agnostic of I/O.
Node was originally designed and likely to run in POSIX environments, they chose to expose I/O in posix interface. In the browser, that's not a POSIX environment, the browser does a direct connection to the developer tools.
In node, we are exposed to posix interface, they are standard I/O streams.
so we can say
```js
process.stdout // to access the stream for standard output
```
Streams are first class cizitens. Actual data structure in node.

To write some content to a stream.
```js
process.stdout.write("Hello world") // Here there is no trailing newline
```
an executing instance of a program is called a program.
console.log is `not` a wrapper around process.stdout.write but we can think of it that way.

even though we removed the abstractions from console.log with process.stdout.write, passing in characters is inefficient. There are still abstractions from running the code to printing the output and it will be a lot faster if instead of characters, it got a binary representation of the string, a buffer.
It will still print out as characters but it will be a lot faster.


Stderr && console.error
```jsp
process.stderr.write("Error message")
console.error("Oops")
```

# CORS
Cors is three parts
the protocol : http/https
the domain without the path , eg: www.google.com
the port: http: 80 and https: 443
