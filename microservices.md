# Microservices

Microservices

Where in monolith, we have all the technologies necessary (server, database etc) setup to implement all the features of the application. In micro services, we have all the technologies setup to implement one feature of the application.

# Rules of microservices

- Each service gets it's own database if needed
- One service does not reach out to the database of another service.

### The real issue with micro servies

- Sharing data between services

# Ways to solve problem sharing

1. Sync
2. Async

## Synchronous method

In Synchronous communication, services talk to each other with direct requests. It could be in http format or json, but they are all direct requests.

### Downsides of this approach

- Requests are only as fast as the slowest direct communication between services
- They introduce dependencies with other services
- It could introduce a web of interconnected services
- If one goes down, it affects the others
- If a request fails in any direct communication between services, the whole transaction might fail.

## Asynchronous Method

In asynchronous communication, services talk to each other using events. There will be an event bus sending and recieving events between services.

### Two ways in asynchronous method

One is similar to synchronous method but with events. Each service might not have it's own db and get's relevant information from other services using events. This has the same downsides as synchronous communication.

The other way is where each service is independent and do not rely on other services. Each service, if it needs data has it's own database. An event bus communicates with each service through events if an event happens. If a request to one service affects another service, the event bus recieves the event from the service that got the request and sends that event to the other service.

# Creating a microservice app from scratch

Steps:
1. Create a service for each resource
2. Describe the responsibilities of each resource
3. Determine dependencies between resources3. Determine dependencies between resources3. Determine dependencies between resources
4. Write out request response goals: what paths, methods, data are required for each goal
  -4.1: The paths might have dependencies posts/:id/comments 
