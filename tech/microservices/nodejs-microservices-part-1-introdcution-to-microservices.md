# NodeJS Microservices
# Part 1: Introduction to microservices

Microservices based architecture ...buzz
------------------------------
Everybody is talking about microservices...

>_Microservice architecture is cutting edge architecture._  
>_Microservices are better than monolitic system._  
>_Microservice architecture is the best architecture for
>NodeJS applications._  
>_Microservices this, microservices that..._

But...
What is that microservice architecture? 
Are microservices good?
Who are microservices for?
What is the learning curve?
Should I use them?
What are the drawbacks of using microservices?

<!-- todo: link to: NodeJS Microservices -->
NodeJS Microservices series is a result of the exploration of the microservice architecture. This and following articles will attempt to answer above and many other questions from the microservices realm. 

Theory in this guide is supported with examples, code samples, configurations and links to valuable resources, which all together should provide solid foundation to master designing microservices. 


What is microservice?
---------------------

Microservices definition from [Wikipedia](https://en.wikipedia.org/wiki/Microservices)
>In computing, microservices is a software architecture style in which complex applications are composed of small, independent processes communicating with each other using language-agnostic APIs. These services are small, highly decoupled and focus on doing a small task, facilitating a modular approach to system-building.

Think of microservice as a application module extracted from the system into self-contained, isolated unit that encapsulate functionality dealing with single problem (concern separation).

The goal of microservices is application decomposition into manageable components with aim of facilitating development and deployment with intention of following best practices of agile methodologies.

Benefits of using microservices
-------------------------------

### Simplicity
Dividing monolithic application into a set of services focusing on a single concern guarantees simplification of the codebase for each of them. 

Complex functionality of the system as a whole is then delivered by combining those easily manageable chunks together. 

### Technology independence
As each microservice is created independently developer is not limited to any particular piece of technology stack or tools.

### Quick iterations
Concern separation means encapsulating functionality to deliver solution to a single problem, which effectively means less code to work with.
Smaller codebase is easier to work with and test and faster to deploy.

### Reusability 
Microservice isolation, decoupling from other parts of the system makes it reusable in different system configuration.

### Flexibility
Because microservice is a small self-contained unit of the system it is easier to replace/rebuild it with another one that might be build on very different stack.

### Scalability
Microservice system architecture makes to scale each individual service vertically (better hardware) and/or horizontal (more instances). That simply means better use of available resources.


Considerations and challenges
-----------------------------
<!-- todo: images for each section -->

### Functional scope
Should be small and readable to make it easy to manage. Each microservice functionality should be focused to deliver possibly the best solution to a single problem.

### Communication layer
Isolation and decoupling from the system should make the microservice an independent unit without internal dependencies to other microservices. It, however, should be able to communicate with other microservices. That be achieved with crafting layer in the form of an RPC or message-driven APIs.

This is where designing each service boundary is critical part. Good practice is following rule of **Transport Independence**, which is based on following assumptions:
* Services don't know about other services
* Services are defined by message patterns
    - handling received message
    - emitting messages to the system

### Data sharing and partitioning

>Data modeling is critical in any system design. It should be done with special care when it comes to microservices-based architecture. 

All microservices can either share single database or access data via abstracted API layer. There is no simple answer on which is better but there are couple of critical factors to consider: 
* each service should be responsible only manipulating own data 
* way service service access data affects scalability of the whole system

It is common for business transaction to update multiple entries which often means updating multple databases owned by different services. Managing and scaling in partitioned database architecture is a big challenge.


### Availability monitoring
As microservice is a part of service stack in a bigger system it has to be constantly monitored to early detect failures and avoid downtime.

### Logging
Each service generates own logs but it might be good idea to keep them consistent and in one place.
<!-- todo: link to resources -->

### Public APIs
Microservice internal routing interface is often optimized for internal usage and not for public exposure of these services for example via REST API.
It can however be achieved with a plugins such as [chairo](https://github.com/hapijs/chairo) for Hapi or [seneca-web](https://github.com/senecajs/seneca-web) for Express.

It translates HTTP requests with specific URL routes into action pattern calls.

### Graceful failure
<!-- todo -->
Philosophy of microservices is based upon accepting service failure as a part of the process. Designing microservice based architecture with assumption of a failure should involve planning for graceful failure.

Failure can be mitigated by:
 * limiting the time microservice awaits for the response message,
 * auto-scaling overloaded microservice by spinning up new instance(s) to share the load,
 * dropping (not queuing) messages that are crashing the service or those which can not be understood,
 * tracing messages across all microservices by its unique message id,
 * implement kill switches allowing to rapidly shut down groups of microservices to stabilise the system in case of major breakdown
  
> Don't try to build perfect system that doesn't fail.
> Instead try to reduce number of failures by mittigation.

### Deployment
Managing deployment of microservices means deploying number of different services where some (or all) of them have to be deployed into multple instances. Successful deployments depends on good control of deployment methods by both developers, and a high level of automation tools.

Planning for deployment should be included and carefuly considered while designing microservices based architecture.