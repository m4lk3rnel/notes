### Spring in the real world
- an *application framework* is a set of common software functionalities that provides a foundation structure fro developing an application.
- Spring is a framework.
- *business logic code* - the code that implements the business requirements of the application.

### Spring ecosystem
- "Spring is an ecosystem of frameworks"

### The spring context: defining beans
- beans - objects that get injected into the Spring Context?
- the context can be seen a place in the app's memory in which we add all the object instances we want the framework to manage.
- object instances = "beans"
- [Maven](https://maven.apache.org/) is one of the most used building tools for Spring projects.
- A build tool is software we use to build apps more easily.
- Recommendation: [Introducing Mave: A Build Tool for Today's Java Developers](https://learning.oreilly.com/library/view/introducing-maven-a/9781484254103/)
- You change the "pom.xml" file to manage the Maven project configuration.
- Unit tests go into the "test" folder
- Your application code goes into the "main" folder
- Add external dependencies using the pom.xml file

- Add a new dependency (pom.xml):

```
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
    </dependency>
</dependencies>
```

- to add Beans to the Spring Context, the @Bean annotation can be used.