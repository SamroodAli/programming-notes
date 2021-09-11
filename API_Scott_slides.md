Notes and data from [Scott's course on frontendmasters](https://slides.com/scotups/api-design-in-node-with-express-v3/#/5/0/5)

# API

* Applicaton programming interface
* Usually a server on some remote machine that dictates how another application can interact with with some data
Basic data operations like, Create, Read, Update, Destroy (CRUD)

## REST
* An API design that combines DB resources, route paths, and HTTP verbs to allow applications
* describe what action they are trying to perform.
* Popularized when SaaS products starting offering APIs for integrations
* Very hard for graph data models with lots of nodes
  
### Nodejs and API
Built for high concurrent APIs that are not CPU intensive

* Node.js is JavaScript, it's async and event driven.
* Single threaded (can optimize)
* When kept async, Node can handle a high amount of concurrent request
* Not great for CPU intensive work (data crunching, ML, big maths)
* So many open source tools to help build APIs

### Express

* Handles all the tedious tasks like managing sockets, route matching, error handling, and more 
* Open source
* Has a huge community and support from anything that has to do with APIs in Node.js
* Not going anywhere anytime soon
* Really simple to use


### MongoDB

* Non-relational document store that is easy to get started and scales well
* Open source and backed by a big company
* Tons of hosting solutions
* ORM / ODM and other libs are some of the best for any DB
* Does not do everything well. Just like any DB


### Mongoose
#### ORM/ODM
Object representation of data in database.


### Dependencies used

```md
    "bcrypt": "^3.0.2", // used for hashing passwords
    "body-parser": "^1.18.3", // middleware to get
    "cors": "^2.8.5",         // 
    "cuid": "^2.1.4",         // testing
    "dotenv": "^6.1.0",       // testing
    "express": "^4.16.4",     // framework we use
    "jsonwebtoken": "^8.4.0", // helps us build all of our authentication
    "lodash": "^4.17.11",     // utility libray
    "mongoose": "^5.3.13",    // orm for mongo
    "morgan": "^1.9.1",       // api plugin for server
    "validator": "^10.9.0"    //  tools to authenticate our schema
```
