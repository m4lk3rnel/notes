- `sudo docker pull image mongo`
- `sudo docker run -d --name "mongodb-server" -p 27017:27017 mongo`
- `sudo docker start "mongodb-server"`

- add users in the mongodb database from MongoDB Compass:
- `Insert Document` -> paste this:
```json
[
  { "email": "alice@example.com", "displayName": "Alice Johnson" },
  { "email": "bob@example.com", "displayName": "Bob Smith" },
  { "email": "carol@example.com", "displayName": "Carol Martinez" }
]
```

- `sudo docker exec -it microservices-with-springboot-mongodb-1 mongosh`
### postgresql

- create a docker network: `docker network create pgnet`
