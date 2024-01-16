# quickstart

- Thanks to de dependency **Spring Web**, it allowed us to create an endpoint wil very little code, in the HelloWorldController
- QuickStartApplication is the app entry point. Pay attention to the annotation @SpringBootApplication
- application.properties is the place where you could configure the app

## Maven
- it's a tool that helps programmers manage their projects and all the things they need to build their programs 
- maven manage dependencies, builds & runs the project and runs the tests
- "mvnw" stands for "maven wrapper", and it's a script that is used to invoke Apache Maven for a specific project without needing to install Maven globally on your system. 

### diving intomaven
> mvnw [options] [<goal(s)>] [<phases(s)>]

> mvn
>   clean
>   default
>   site

- we can think of a phase as a **lifecycle**
  - a lifecycle may contain one or more goals
    - a goal is something specific to achieve
    - a lifecycle also is the order of the goals you want to achieve

- so, each of the phases (clean, default & site) have their own purpose
  - clean removes temp directories and files
  - default this is the most useful goals live
  - site is where the docs are generated

#### diving maven clean 
> mvnw clean
>   pre-clean -> executed when we want to hook some action before cleaning
>   clean -> does the actual cleaning
>   post-clean -> executed when we want to hook some action after cleaning 

#### diving maven [default]
> mvnw [default]
>   compile -> compiles your code into bytecode
>   test -> runs the unit tests
>   package -> creates a jar or war file
>   verify -> runs checks & integration tests

- keep in mind that this command will happen in an orderly fashion. This means that if you want to run some integration tests, for example, first maven will run the compile, test & package commands  

## using maven project structure
using maven implies using maven project structure

## application layers
- presentation
- service
- persistence

### the persistence layer 
Is there to handle the interactions with our database, which are often thought of entities, at this level.
- The entities are java objects that represent the tables in our database, or if we use a nosql db, the equivalent in those technologies
- we use the entities in this level using one of different patterns, such as 
  - DAO (data access object) 
  - Repositories

- the type of functionality that we can find in this layer is:
  - CRUD operations (Create Read Update Delete)
  - search operations
  - data validation

You want to build all this interactions with some really well defined interfaces, so that you can easily swap the implementation of the persistence layer, without affecting the rest of the application

### the service layer
its goal is to add all of the functionality exposed by the persistence layer and the use it to meet the requirements of the application that's built to handle.
- the functionality in the service layer can be as complicated as it needs to be, or it simply be passed through the persistence layer
- this is still important because we don't want our persistence layer be talking to our presentation layer.
- so, the communication between the persistence layer and the presentation layer must be always via the service layer

### the presentation layer
- the goal of this layer is to use the data and functionality exposed by the service layer to meet the requirements of the application and expose them to the user, normally using a REST API, a graphql api, a websocket api, etc
  - this is achieved by using controllers as an implementation of that

## inversion of control & dependency injection
- IoC is a design software term that refers to a proces to deferring resposibility for the creation of objects to some other agent. In this case, the framework
- For this, dependency injection is the technique that is used to achieve inversion of control. In DI, required dependencies are injected into the object, instead of that same object creating them itself internally.
- this means that we can write out code independntly of how the objects are created, and that we can also swap the implementation of the objects without affecting the rest of the application
- for me to use DI, I need to first understand and create the interfaces, that is, what will be needed to be implemented, and then the concrete classes that implement those interfaces

## introducing beans
- so writing interfaces instead of classes is the way to do things in spring
- we should leave the framework to supply the concrete classes wherever we declare the interfaces via dependency injection
- those classes are called "beans"
- In essence, a Bean is an object that combines both configuration and functionality, and it is managed by the framework to promote modularity, loose coupling, and easier maintenance in your Java application. It's not solely a piece of configuration or a concrete implementation; it's a managed component that serves a specific purpose in your application.
