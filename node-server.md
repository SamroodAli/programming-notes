# Background

Read [Internet notes](internet.md)
Server and Client share data in packets.
### Our responsibility

When someone opens up netflix in their browser, the browser(a client here) makes a request (data packets) to netflix's servers which responds (also in data packets). A server is responsible for handling these requests, these data packets.
Our responsibility is to ingest this data (request in data packets) and respond (send response in data packets).

### The capability.

Are we capable of handling this responsibility ?
Yes, Node has the ability to do this. Google's software known as the `v8 Engine` in chrome can handle this and Node uses this software.

### A server

Every computer has the ability to talk to each other on a network using http protocols.There are 2^16 network ports made available in a machine. Ports application can attach themselves to and listen for requests and respond.

Our computer listens to http requests and responses in port 80.
[Read more on port in wikipedia](https://en.wikipedia.org/wiki/Port_(computer_networking))


### Creating a server using in built Node http module

1. Import http module
The inbuilt http module lets Node share data using the HTTP protocol and helps us create requests and responses. It also has the `req` and `res` object which we will use to. 

`These are not the same as Request and Response objects in window`. To respond to clients(browsers), we need an application constantly listening to requests(data packets).

```js
const http = require("http");
```


2. Create a server using the http module (we will come back to edit this line later ).
The `createServer` method in the `http` does two things
  * Opens a network channel (socket) in the computer (by invoking an http feature in node which opens a network connection in the computer's internals)
  * Store the callback passed in as argument and call it once for every HTTP request that's made against the server. ( we will learn this later)

```js
const myServer = http.createServer()
```

This is not going to do much in javascript other than assigning the `server object` returned from `createServer()` method to a variable called `myServer`. The server object is an object full of methods/APIs to help us manage our server.
Internally though, with the help of `libuv` (c++ code to access any computer's (mac,linux etc) internal structure), Node will setup and open an open socket/an open channel to the internet, ready to recieve http messages. One issue though, there are 64000 entry points (ports) to the computer. Which entry point should the http messages come at ?

The default entry point(port) that all http messages come in is port 80. So we need to specify that the  channel to listen to http messages is port 80. We do that with the listen method available in the `server object` stored in the `server variable`.

1. Set a port for the server to listen on.y6
All http requests default to port 80. There are exceptions such as https which is to a different entry point.The `listen` method in http sets the port number of the socket to specified port (node does this with the help of the c++ library libuv.)

```js
myServer.listen(80,()=>{console.log("server listening on port 80")})
```

Now if we send a http message to this machine, it will arrive in Node.
We inspect the incoming http messages(requests) and then send an http message back(a response).
From here on, essentially server is going to act as the middleman between requests and responses.
It is going to inspect the incoming requests and send the responses.
request recieved => Node => sent response.
Sometimes we might have different responses for different requests. We will have to inspect the incoming request and send the right responses.
The psuedo code being:

```js
if (incoming request === this){
  send that
}
```

So we need to write functionality that knows how to handle requests and what responses to send. It will have to be called everytime a request comes. How do we bundle code and use it multiple times. The answer is by packaging the code as a function.

4. Write a function to read requests and send responses. We will complete this function later.
```js
function onRequest() {
// a function that runs on every request
}

1. Setup server to respond on every request.
```
There is one final issue though. Javascript is a single threaded language that is just going to run line by line till the end. We can only call this function when the request comes in. What if the request comes late, there will not be any javascript code to run! We cannot keep calling the javascript function forever, keeping our code running. How do we solve this ?

Who does know when an http messages comes? The answer is Node. So we have to tell Node every time a http message(request) comes, call the above function. So the above function will be passed into Node as a callback that runs every time a request comes in.

Let's tell Node that.We will have to edit the code we wrote above in step 2, i.e, when we created the server to take in this callback.

```js
http myServer = http.createServer(onRequest)
```

We now have a written a server using Node.

### Running the callback

Previously, we created a function which was passed in as a callback to http.createServer method which auto runs on every request.
function `onRequest` above will be called by Node. Node will invoke the function `onRequest` (add parenthesis at the end) and passes some arguments as well. It is similar to DomElement.addEventListener("click",callback) where a callback will be run everytime a user event (click event here) happens. The browser collects information about the event (click event here) and bundles up that information as an object and passes it as an argument commonly known as `event` to the event listener's callback.

Node too will insert whatever the relevant data (from request) is as arguments.

When a request comes in, it is in a format we cannot deal with in javascript.It is a string of characters, an HTTP message (a text message). So we got an incoming message (request http message).Node also prepares an http message to send back(response http message). But we cannot access these two http messages directly,instead Node will automatically parse these messages and auto create two javascript objects for us just like parsing an html document and creating the `document` object so that we can manipulate the DOM, javascript gives us two objects and will pass them as arguments to the callback we passed in to `http.createServer` method

We will edit the above `onRequest` function to take in these two arguments inserted by node. Parameters variables can be any name. We just have to reference the same parameter names inside the function. But the common convention is `req` and `res`. These placeholders/parameters will be assigned the two auto created objects. The first object (assigned to first argument `res`) has methods and properties (APIs) to access the parsed http request message that came in and the second object (assigned to second  parameter `res`) has methods and properties (APIs) to access the http response message that we will send back.

We will edit the function onRequest above to take in these two objects as arguments.

```js
function onRequest(req,res){
    // Sample code
    //  The route is in req.url
  if (req.url === "/") {
    // user wants the home page
    // writing to http header
    res.writeHead(200, { "Content-type": "text/html" });
    // reading from a html file
    const homePagehtml = fs.readFileSync(__dirname + "/index.html");
    // writing the content of the html file as response
    res.write(homePagehtml);
    // ending the connection
    res.end();
  } else if (req.url.match(/^.*\.css/)) { // else if req.url match any css files
    // write the header and set mime type to css 
    res.writeHead(200, { "Content-type": "text/css" });
    // get css file
    const stylesheet = fs.readFileSync(__dirname + "/style.css");
    // write the content of the css file as response
    res.write(stylesheet);
    // end connection
    res.end();
  } else if (req.url.match(/^.*\.(jpg|JPG|gif|GIF|png)$/)) { // if request path match any images // dynamic route example
    let image;
    // try to load the image if available
    try {
      //load the image
      image = fs.readFileSync(__dirname + req.url);
      // set mime type to image/png
      res.writeHead(200, { "Content-type": "image/png" });
    } catch (e) {
      // if no image, write reponse content
      image = `<h1>Image Not found</h1>`;
      // set status code to 404=> content not found
      res.writeHead(404, { "Content-type": "text/html" });
    }
    // write response (image or 404 response content)    
    res.write(image);
    // end connection
    res.end();
  } else {
    // else respond with 404 page
    res.writeHead(404, { "Content-type": "text/html" });
    const homePagehtml = fs.readFileSync(__dirname + "/index.html");
    res.write(homePagehtml);
    res.end();
  }
}

```
1. request(text based http messsage) => parses this http message => create an object with APIs to read this http message data
2. Node creates another http message=> response=> node creates an object with APIs to access and write this http message.

Node => onRequest(the first object, the second object)
### HTTP message format
has three parts
1. Starting line / Request line => GET /users/3
2. Headers: Meta data (data about data) about the client requesting
3. Body of the message (Optional)

Our return message is also in HTTP format

We can use the `body` to send the data and the `headers` to send important metadata.
In the headers, we can include infor on the format of the data being sent, eg: it's html so it can load it as a webpage.

### Error handling

The server object returned above is an `event emitter` object ([Read about event emitters here](https://nodejs.dev/learn/the-nodejs-event-emitter)).

The eventEmiiter instance is like an event in the browser. We can store callbacks that runs on specific events. Just like a click listener, we can have our own listeners for custom events.
Every emitter has an `on` method and an `emit` method. The `on` method takes in an event name as well as callback. The emit takes in an event name and invokes the callback saved via the `on` method.
```js
const EventEmitter = require("events")
const eventEmitter = new EventEmitter()
// create a custom event 'userEvent' and a callback to run on this 'userEvent'
eventEmitter.on("userEvent",function runOnUserEvent(){})
//emmit that event which invokes runOnUserEvent
eventEmitter.emit("userEvent")
```

So instead of passing in the onRequest function that auto runs on every request, we can do the same using the `on` method and the event `request`.
```js
function onRequest(req,res){//code to run on every request}
const server = http.createServer()
server.on('request',onRequest)
```
The server object also takes another callback for client errors, i.e errors in the incoming request http message using the same `on` method and an event called `clientError`.

```js
function OnError(err){
  console.error(err)
}
server.on('clientError',onError)
```

So when a request comes into node, it does not directly run the `request` callback but checks if it an error first.

```md
if error=> server.emit('clientError')
else server.emit('request')
```

There are a list of ways we can get corrupted client HTTP request ( one example is misformated json data). If that happens, node immediately knows and emits the event 'clientError' and the run the callback passed to server object via `on` with event'clientError'. Node collects info about the error and bundles it up in an object called `error` and passes it as the argument when invoking the onError callback.
There is also a second argument which is an access to the socket directly. There is no autocreated HTTP message (with access to write it in an object (res)). We have to add text in HTTP format that will turn this into an HTTP format if we want.

```js
function onError(err,socket){
  console.error(err)
  socket.end('HTTP/1.1 400 Bad Request\r\n\r\n');
}
```
# Request and Response are readable streams
req.pipe(res) is used to pipe the request readable stream to res writable stream
