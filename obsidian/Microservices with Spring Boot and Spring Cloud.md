## Introduction to Microservices

- The **microservice architecture** is about splitting up monolithic applications into **smaller components**.
- A **microservice** is essentially a software component that is independently upgradeable, replaceable and scalable.

- [resilience patterns in Microservices](https://www.geeksforgeeks.org/system-design/microservices-resilience-patterns/)
	- resilient -> fault-tolerant
##### Design Patterns
 - **Service discovery**: 
	 - microservices are typically assigned dynamically allocated IP addresses when they start up. how can the client find microservices and their instances?
	 - add a new component, a service discovery service which keeps track of currently available microservices and the IP addresses of its instances.
	 - [Netflix/eureka: AWS Service registry for resilient mid-tier load balancing and failover.](https://github.com/Netflix/eureka)
	 - **client-side routing**: the client uses a library that communicates with the service discovery service to find out the proper instances to send the requests to.
	 - **server-side routing**: the infrastructure of the service discovery service also exposes a **reverse proxy** that all requests are sent to. The reverse proxy forwards the requests to a proper microservice instance on behalf of the client.
	 
- **Edge server**:
	- in many cases it is desirable to expose some of the microservices to the outside of the system landscape and hide the remaining micorservices from external access.
	- add a new component, an **edge server** that all incoming requests will go through (acts like a reverse proxy). it can also be implemented with a **service discovery** service.
	- protect external services from **malicious** requests using standard protocols and best best practices such as OAuth, OIDC, JWT tokens, and API keys to ensure the clients are trustworthy.

- **Reactive microservices**
	- blocking I/O communication (system threads) can cause the server to crash in microservice architecture because the server might run out of threads.
	- solution: using non-blocking I/O to ensure that no OS threads are allocated by using reacting programming model such a as **Project Reactor** or using virtual threads.

- **Central configuration**
	- add a new component, a **configuration server**. you don't have to configure each microservice manually.

- **Centralized log analysis**
	- add a new component that can manage **centralized logging**. collect logs from microservices, store them in a structured and searchable way in a central database.

- **Distributed tracing**
	- mark all messages and requests with a correlation ID and make sure that the correlation ID is part of all log events. using the **centralized logging** service we can find all related logs.
	- timestamps in requests are useful for analyzing delays between microservices.

- **Circuit breaker
	- **cascading failure** - a phenomenon where the failure of one component in an interconnected system triggers the failure of other components, creating a domino effect.
	- the **Circuit Breaker** pattern can prevent cascading failures.
	- add a circuit breaker between services.
	- it can have 3 states:
		- **closed**: requests flow normally
		- **open**: requests are immediately blocked or failed fast
		- **half-open**: a limited number of test requests are allowed through. if they succeed the breaker resets to **closed**

- **Control loop**
	- "is a pattern that helps microservices to automatically adjust their behaviour in response to changes in the system"
	- it is difficult to **manually** detect and correct problems (unresponsive or crashed service)
	- add a new component, a **control loop** to the system landscape.
	- the control loop will constantly observe the **actual state** of the system landscape, comparing it with a **desired state**, as specified by the operators.
	![[control_loop.png]]
	- *Kubernetes* which is a *container orchestrator* is typically used to implement this pattern.

- **Centralized monitoring and alarms**
	- for example, we need to be able to analyze hardware resource consumption per microservice.
	- add a **monitor service** which is capable of collecting metrics about hardware resource usage for each microservice instance level.
	
	![[prometheus_grafana.png]]
	
	- [Prometheus - Monitoring system & time series database](https://prometheus.io/)
	- [Grafana: The open and composable observability platform | Grafana Labs](https://grafana.com/)
	- [Semantic Versioning 2.0.0 | Semantic Versioning](https://semver.org/)
	- [The Twelve-Factor App](https://12factor.net/)

## Introduction to Spring Boot
- *Reactive programming* focuses on non-blocking I/O operations.
- a fat `JAR` file contains not only the classes and resource files of the application itself but also all the `JAR` files the application depends on. 
- `@SpringBootApplication` : auto-configuration mechanism, annotation used on the application class

```java
@SpringBootApplication
public class MyApplication {
	public static void main(String[] args) {
		SpringApplication.run(MyApplication.class, args);
	}
}
```

- it enables *component scanning*, that is, looking for Spring components and configuration classes in the package of the application class and all its subpackages.
- the application class itself becomes a configuration class.
- it enables autoconfiguration. for example, if you have tomcat in the classpath, spring boot will automatically configure Tomcat as an embedded web server.
- `springdoc-openapi` (API docs for Spring)
	- auto-generates **OpenAPI 3** JSON at `/v3/api-docs` from your spring MVC/WebFlux controllers and model annotations.
	- serves interactive **Swagger UI** at `/swagger-ui.html` (or `/swagger-ui/index.html`)
	- gradle:
```
dependencies {
	implementation("org.springdoc:springdoc-openapi-starter-webmvc-ui:2.6.0") // or webflux
}
```


## Creating A Set Of Cooperating Microservices
- the composite microservice aggregates all the information from the other microservices.
- you can generate a spring project by using the `spring init` command line tool.
	- you can also use [Spring Initializr](https://start.spring.io/)
	![[spring_init_generate_project.png]]
- dependencies are fetched from the Maven Central repository.
- list the files we created: `find microservices/product-service -type f`
- [Spring Initializr](https://start.spring.io/) creates a `.gitignore` by default.
- `RestTemplate` - a synchronous client to perfom HTTP requests. (blocks the calling thread)
- `WebClient` - reactive client to perform HTTP requests. (non-blocking)

### Deploying our microservices using Docker
- `docker run -it --rm ubuntu`
	- `--it` so we can interact with the container using the terminal
	- `--rm` the container gets removed once we exit
	- `cat /etc/os-release | grep "PRETTY_NAME"
	
