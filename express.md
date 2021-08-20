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

2. Create a server using the http module

```js
const server = http.createServer()
```
