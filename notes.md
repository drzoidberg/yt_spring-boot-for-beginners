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

## way of declaring beans
### using a config file
- the config file will be a java class that will contain the beans that we want to use in our application
- so writing interfaces instead of classes is the way to do things in spring
- we should leave the framework to supply the concrete classes wherever we declare the interfaces via dependency injection
- those classes are called "beans"
- In essence, a Bean is an object that combines both configuration and functionality, and it is managed by the framework to promote modularity, loose coupling, and easier maintenance in your Java application. It's not solely a piece of configuration or a concrete implementation; it's a managed component that serves a specific purpose in your application.

### using components (and friends)
#### @Component annotation
- you'd only had to remove the current file and add the @Component annotation to the classes you'd want to be beans

#### @Service annotation
- this annotation is a specialization of the @Component annotation, and it's used to annotate classes that perform service tasks, often you'd use this annotation in the service layer
- it does essentially the same as a component, but it's more specific and descriptive

##### WHAT I'VE DONE FOR NOW
- I've created the project with its initial state, which was a simple hello world that was somewhat coupled. So, my main goal is to decouple it.
- Instead of creating the instance in the controller, I've created a service layer, and I've created an interface for it, and a concrete class that implements it. I've also created a bean for the service layer, and I've injected it in the controller.
- I left one of the services as a bean, and the others one as a component, just to see the difference between them
- Keep in mind that if you use the @Component/@Service annotation, no this specific case, you need to trace the interface in the controller, and not the concrete class, because the interface is the one that is being injected, and not the concrete class.
- That will reveal you the components the interface needs to be implemented by. Those will be the ones that will be annotated with @Component/@Service, and if there are more than one annotated with @Component/@Service at compilation time, you'll get an error, because the framework won't know which one to inject.

## configuring the application
### component scanning
- This is a way to tell spring to look for beans in a specific package, and for those beans are needed.
- For the EnglishRedPrinter, the framework asks if this bean need any dependency, that is a bean annotated with @Component/@Service. we did this before by declaring them in our constructor
- So, the component scanning phase will create an instance of all declared classes and place them as beans in the application context, which is a container that holds all the beans that are created in the application
- this beans then will be able to be plucked out and placed when and where needed
- so! having said all of this, this process is also called **DEPENDENCY INJECTION**, or **AUTO-WIRING**, which a is more Spring specific term

### @SprinBootApplication
@SpringBootApplication is an alias for a number of different annotations, such as:
#### @Configuration
This one identifies a configuration class that contains bean definitions
#### @ComponentScan
This one tells Spring to look for other components, configurations, and services in the package and registering where and when they are needed
#### @EnableAutoConfiguration
This one tells Spring to automatically configure the application based on the dependencies that are added to the project
- Autoconfiguration is the process spring boot uses when the application starts up, that provide sensible defaults and creates all those dependencies.
- Spring boot starters are a collections of dependencies that are grouped together to provide a specific set of functionality
##### the application.properties file
this file is used to configure the application, and it's located in the resources folders
- we need to go to the **spring boot documentation** to see what properties are available for us to use
- keep in mind that there are other ways to configure the application than using this file: 
- we can use the command line
- we can use environment variables
- we can use system properties
- a yaml file
- we can apply config files also in tests. This come in handy for cases like for telling the tests to use a mocked database than the one used in production
###### env variables
This is another way to configure the application. Is most important to know becasue when you use the docker image, you'll need to use env variables to configure it.
- as a geenral rule, the env variables are nemade the same as the application properties, but they are in uppercase and with underscores instead of the usual delimiter, which is a dot
- this can be configured in intellij by creating a .env file or in the edit configurations panel, and selecting 'environment variables'
- you can also put the env variable before the mavenwrapper command
- you can also put the env var in temp memory of that terminal using the 'export' command

### configuration properties
