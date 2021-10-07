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

### Controllers and the Middleware pattern
Express is basically writing middlewares and controllers.
instead of one giant function that handles the incoming request, we seperate into mini functions.Middlewares and controllers are the same except in their intentions. 
# Controllers
The callbacks you register for different routes and verbs. They are intended to respond to the request.
```js
app.get('/',controller)
```

These functions are called controllers
### Middlewares

Middlewares are the same as controllers but they pass functionality to the next middleware/controller using a third argument called `next`
Middlewares are just functions that execute, in order, before our controllers
Incoming request goes through each of them.

You can pass an argument to next which will be treated as error (the error object). You can pass the error to the next controller/middleware

* Executes in guaranteed order
* Great for authenticating, transforming the request, tracking, error handling

Requests Request <======== Middlewares =======> Controller'sResponse
Middlewares sit between requests and responses and are how we take a request and respond to it. Middleware is essentially what we pass into a route matcher function like ones above as a callback.
Express is basically writing middlewares

We use middlewares using `app.use()`
```js
// Match all routes
app.use(middleware) // matches all routes
app.use("/admin",middleware) // matches /admin
app.get("/",middleware) // matches get method / route
```

App.use matches all verbs and "/" matches all as long as it starts with "/", i.e , "/", "/users/" etc

### controller/middleware erity
in app's verbs, you can register as many controllers/middlewares as we need.
```js
//creating  middlewares
const createLineInConsole = ()=>{}
const logger = ()=>{}
const authenticator = ()=>{}

// pass as many as middlewares as we need
app.use(createLineInConsole,logger,authenticator,createLineInConsole)
// specific route, pass as many middlewares before our controller
app.get("/",createLineInConole,logger,authenticator,function(req,res){})
// you can also pass in an array of middlewares
const logHelpers =  [createLineInConsole,logger]
app.get("/data",logHelpers,function(req,res){})
app.get("/user,[createLineInConsole,logger,authenticator],function(req,res){})
```

Each controller has to call `next` parameter to pass functionality to the next one.

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

### Built in middlewares

#### cors - Cross Origin Resource sharing
Makes our server cors enabled. If we have a server on mydomain.com and we have a website on otherdomain.com, then the browser might not allow to make a request to the server (as it is not in the same domain, or a cross domain). The server has to implement corss domain resource sharing which basically means the server allows other domains, headers etc. It basically allows the API to be used by other domains that are on the browser. Server to server doesn't really respoect CORS, this is mainly for the browser.

#### { json } from body-parser
Parse the stream(chunk) of data in incoming request into a json object.

```js
const json = function(req, res, next) {
  let data = ''
  req.on('data', chunk => {
    data += chunk
  })
  req.on('end', () => {
    req.body = JSON.parse(data)
    next()
  })
}

app.use(json)
```
In express, you have to use `app.use(json())`


### CORS
Cors middleware makes our server CORS enabled. Cross Origin resource sharing. If we have a website on mydomain.com and server on otherdomain.com, the browser might not allow us to interact with the server's domain as it is a cross domain. Server to server does not really respect CORS, this is for the browser. Allows everyone to consume our APIs.
### { urlencoded } from body-parser
This middleware, also like json, transforms the request. UrlencodedeAs per RFC 3986, characters found in a URL must be present in the defined set of reserved and unreserved ASCII characters. However, URL encoding allows characters which otherwise would be not permitted to be represented with help of allowed characters. URL encoding is used mostly for non-ASCII control characters – characters beyond the ASCII character set of 128 characters and reserved characters such as the semicolon, equal sign, space or caret. 
Http forms are usually url encoded, this middleware allows you to view this data (of the type url-encoded such x-forms-url-encoded) and it is attached to req.body
### morgan
It does logging for us. It logs the route and query strings etc to the console

### REST routes with Express
REST api is a combination of a route and an HTTP verb
Express has a robust route matching system that allows for 
exact => "/data"
regex => /data/
glob => "/user/*"
and parameter matching => "/:id"

Stick to exact and parameter matching


# Sub Routers -  Express Router
You can seperate express functionality into seperate routers.

```js
const user = Express.

```
