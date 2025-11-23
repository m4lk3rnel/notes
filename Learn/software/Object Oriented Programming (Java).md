
### Singleton

- create a simple thread-safe singleton (**Enum Singleton**):
```java
public enum Singleton {
	INSTANCE;
}
```

- has an implicit empty constructor. make it explicit:
```java
public enum Singleton {
	INSTANCE;
	
	private Singleton() {
		System.out.println("Constructor");
	}
}
```

- also **reflection-safe** and **serialization-safe**.


### Data Transfer Objects

- **DTO**s are used to transfer data between layers in Spring (e.g. between the controller and the service)
- In spring **DTO**s can be used instead of Entities when exposing data through the **API**. This can avoid exposing sensitive data like passwords to the client.
- You choose what to expose to the client. You reshape the data (**combine fields**, **omit some**, etc.)
- **Separation of concerns**: **entities** (like JPA `@Entity` classes) represent the database structure, while **DTOs** represent the data you want to expose through your **API**.

### SOLID design principles
- [SOLID - DigitalOcean](https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design) 
- 