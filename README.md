# backend-architecture
My notes on how to architect the Backend



Layers
Entry point
routes
controllers
model
logic
helpers - Also as a layer between the app and external libs

Why?
- Describe the problem (view it from the MVC perspective but be flexible)

How?
- Describe the solution
- Layers and why we need them
- 

When
- There are variations and different scenarios


Folder structure

```
auth
config
controllers
db
docs
middleware
models
routes
utils (helpers)
logic
app.js

```


AUTH: 
Authentication and Authorization related logic.

Config:
Will contain all the configuration files, even when configurations are being done using environment variables. Every configuration must go into a specific file and should be imported where needed. This works as a source of truth and helps keep every configuration accounted for across the application.

controllers:
There are collections of functions that are responsible for executing a task and are directly related to the routes. Controller functions are grouped in logical groups that correspond with the domain of the application. These functions receive an HTTP request and are responsible for delivering a response, they are also responsible for calling the correct business logic. So, a controller function won't do any logic by itself, it only works in the context of requests and responses and is responsible for knowing who to invoke with the correct logic, also it will clean up and extract the data from the request and will know how to format the response.

Model:
is the layer that will communicate with the database, if the application is small enough, it can even contain the business logic. The recommended pattern is the [Active Record](https://www.martinfowler.com/eaaCatalog/activeRecord.html).


db: Code to connect to the different databases the application needs, normally we will have a file inside config with the values we need to connect to a database and another file with the actual logic to connect to the database within this folder.
We will use one file per DB.

docs: This folder will contain text files (txt, markdown, etc..) documenting the application and can include the ADRs (Architecture Decision Records) or other information that can be useful. The idea of having the documentation in simple text files is that it can evolve with the code and be tracked using the version control system.

middleware: Express-related middleware that is not the routes and controllers, like for example the errorHandler or any other middleware the app may need.

routes: Is part code part configuration, the task of the routes is to route an HTTP request to the correct controller function. The routes will decide what route and HTTP Verb corresponds with what controller.


utils (helpers) : Can be functions, classes etc. This is a vague category regarding concrete implementation because a utility can be of many types depending on the problem it solves. Utilities should also be used as a layer between third-party libraries and our code (eg hiding Axios or MomentJS) so we don't fully depend on third-party libraries and give the possibility to replace them in the future if needed.

logic: In complex applications, business logic is too big to be held on the model (Active Record classes). Instead, other patterns can be implemented like [Domain Model](https://martinfowler.com/eaaCatalog/domainModel.html) and [Data Mapper](https://martinfowler.com/eaaCatalog/dataMapper.html) but the main objective of this layer is to have a place to store the complex business logic. This layer won't know about the details of the application, like what type of DB is being used. The controllers can invoke them but they will get only the data they need in the format they need it and won't know about the existence of controllers but they can know about the existence of the model classes and will use them to get the data they need.


entry point (index.js | app.js): Main entry point for the application, is the file that will get executed by the node engine and has the responsibility of loading everything the app needs in the correct order.
