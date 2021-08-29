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

### Middleware pattern

instead of one giant function that handles the incoming request, we seperate into mini functions where the incoming request goes through each of them.
These functions are called middlewares
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

### Built in middlewares

