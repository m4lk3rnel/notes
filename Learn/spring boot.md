- https://www.youtube.com/watch?v=gJrjgg1KVL4

- spring framework
	- core: dependency injection
	- web: for building web appications
	- data: working with databases
	- aop: aspect oriented programming
	- test: for testing components
- you can choose which modules you wanna work with, this makes spring very modular.
- spring boot, a layer on top of the **spring framework**

- to install maven (globally) you can use the chocolatey (a package manager for windows)
	- macOS: `brew install maven`

- you can create a spring boot application, using the *spring initializr* (https://start.spring.io/) or you can generate project directly from IntellJ Idea (feature only available for Ultimate edition users)
- artifact: the name of the project
- group: used for specifying the organization that owns the project (for example: `com.example`)

#### Project structure
- mvn wrapper - you don't have to install maven globally, mvn is project specific, great for different environments. (just like the gradle wrapper)
- mvnw.cmd - for windows, installs the version specified in `.mvn -> wrapper -> maven.wrapper.properties`
- mvnw - same as mvnw.cmd, but for linux
- `pom.xml` - Project Object Model, the heart of a maven project
- in the `src/main/test` we have the automated tests.
- in the `src/main/java/` we have the actual code.
- in the `src/main/resources` we have non code files, like assets or configuration files.

#### Dependency management
- `spring-boot-starter-web` : curated collection of libraries and frameworks commonly used together
- [Maven Central](https://central.sonatype.com/)
- you can let IntellJ resolve versions for dependencies. (you don't have to specify the `<version></version>` for each dependency)

### Building your first Spring Controller
- Spring MVC, part of the spring framework
- MVC stands for Model View Controller
- MVC makes it easier to separate different parts of our application, and makes it easier to manage and scale
	- *Model*: is usually connected to our database, or other datasources
	- *View*: is what the user sees
	- *Controller*: handles incoming requests from the user, gets data from the Model and tells the view what should be displayed.
- `@Controller` used for marking a controller
- `@RequestMapping`
- `src/resources/static/index.html`

#### Running Spring Boot Applications
- `./mvnw spring-boot:run`
- cool IntellJ shortcuts: 
	- `CTRL + Shift + N`: search files, classes, etc.
	- `ALT + Insert`: generate functionality (for example, generate a block in the `pom.xml` file)
- get value from `application.properties`
```java
@Value("${spring.application.name}")
private String appName;
```

- dependency injection:
- 