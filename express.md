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


