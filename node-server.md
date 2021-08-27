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


### Creating a server using in built node http module

1. Import http module
The inbuilt http module lets node share data using the HTTP protocol and helps us create requests and responses. It also has the `req` and `res` object which we will use to. 

`These are not the same as Request and Response objects in window`. To respond to clients(browsers), we need an application constantly listening to requests(data packets).

1. Import http module
```js
const http = require("http");
```


1. Create a server using the http module (we will come back to edit this line later ) 

```js
const server = http.createServer()
```
This is not going to do much in javascript other than assigning the `server object` to server variable. The server object is an object full of methods/APIs to help us manage our server.
Internally though, with the help of `libuv` (c++ code to access any computer's (mac,linux etc) internal structure), node will setup and open an open socket/an open channel to the internet, ready to recieve http messages. One issue though, there are 64000 entry points (ports) to the computer. Which entry point should the http messages come at ?

The default entry point(port) that all http messages come in is port 80. So we need to specify that the  channel to listen to http messages is port 80. We do that with the listen method available in the `server object` stored in the `server variable`.

3. Set a port for the server to listen on
```js
server.listen(80,()=>{console.log("server listening on port 80")})
```
Now if we send an http message to this machine, it will arrive in node.

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

1. Write a function to read requests and send responses.
```js
function onRequest(req, res) {
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
There is one final issue though. Javascript is a single threaded language that is just going to run line by line till the end. We can only call this function when the request comes in. What if the request comes late, there will not be any javascript code to run! We cannot keep calling the javascript function forever, keeping our code running. How do we solve this ?

Who does know when an http messages comes? The answer is node. So we have to tell node every time a http message(request) comes, call the above function. So the above function will be passed into node as a callback that runs every time a request comes in.

Let's tell node that.We will have to edit the code we wrote above in step 2, i.e, when we created the server to take in this callback.

```js
http server = http.createServer(onRequest)
```

We now have a written a server using node.
