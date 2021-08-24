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

2. Write a callback to read requests and send responses
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
3. Create a server using the http module and pass in the callback

```js
const server = http.createServer(onRequest)
```

4. Set a port for the server to listen on

```js
server.listen(3000,()=>{console.log("server listening on port 3000")})
```


# EXPRESS

1. Import the module express => An npm package, a framework over the http module.
1. Express is essentially two things
* A router
* Middlewares

### Importing and using express
1. We need to first import express app (Assuming we have it already as a dependency).
```js
import express from express
```
2. Create a server (by creating an express app)
```js
const app = express()
```
3. Set the server to listen to a port
```js
const port = 5500
app.listen(port,()=>{console.log(`port is listening on ${port}`)})
```

### Router
Routes requests to various paths(such as "/","/user") and http methods(verbs such as post,get,put,delete etc).
[Read more on http methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods).

These functions essentially map the request paths to an appropriate functionality

```js
// Respond to a get method
app.get()
// Respond to a post requet
app.post()
// Respond to a put request
app.put()
// Respond to a delete request
app.delete()
// Respond to all requests
app.all()
app.use()
```

### Middlewares
Express is basically writing middlewares.
Requests Request <========Middlewares=======> Response
Middlewares sit between requests and responses and are how we take a request and respond to it. Middleware is essentially what we pass into a route matcher function like ones above as a callback.
```js
// Match all routes
app.use(middleware) // matches all routes
app.use("/admin",middleware) // matches /admin
app.get("/",middleware) // matches get method / route

```


`Middlewares is just a function that uses the req/res and next arguments.`

Before that, let's learn about express's top down approach.

lets say we wrote two functionality which match the same route.

```js
//validate user
app.get("/",function validatesUserEveryTime(req,res){
  // pseudo code inside if statement that checks if it is the correct user
  if (check if correct user){
    req.locals.validated = true
  } else {
    req.locals.validated = false
  }
})

// second middleware/function
app.get("/",function respondWithMarkup(req,res){
  res.send("<h1>Hello world!</h1>")
  res.end()
})

```
The second functionality above will never run. That is because express follows a top down. We can override this and tell the first function to run the next one(second function) by using the third argument. Let's solve this issue :-

```js
app.get("/",function validatesUserEverTime(req,res,next){
  //validate user functionality as above
  next() // call next to pass to the next middleware
})

//second middleware/function and this will run now.
app.get("/",function respondWithMarkup(req,res){
  res.send("<h1>Hello world!</h1>")
  res.end()
})

```
Since we used next() in the above function, the next function will be executed as well.
