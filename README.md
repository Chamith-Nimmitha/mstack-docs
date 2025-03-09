# mstack-docs
Documentation for MStack java framework

Below is a comprehensive documentation template for the MStack Java framework, designed to help developers understand and utilize its features effectively. MStack is a versatile framework that allows you to build applications as either a monolith or microservices using the same codebase, providing flexibility, scalability, and resilience.

---

# MStack Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [Architecture](#architecture)
4. [Core Components](#core-components)
5. [Configuration](#configuration)
6. [Usage Examples](#usage-examples)
7. [Advanced Topics](#advanced-topics)
8. [API Reference](#api-reference)
9. [Troubleshooting](#troubleshooting)
10. [FAQ](#faq)
11. [Contributing](#contributing)
12. [License](#license)

---

## Introduction

MStack is a powerful Java framework designed for building applications using a microservices approach from the ground up. It allows developers to create a microservices-ready codebase that can run as a single process (monolith) during early development for rapid prototyping, then seamlessly transition to a fully distributed microservices architecture as the application grows—all without changing the code. This approach accelerates development while ensuring scalability and resilience.

### Why MStack?
MStack provides a customizable, feature-rich foundation for microservices development. It enables rapid iteration by running your microservices-designed application as a monolith initially, then scales effortlessly to a distributed system when needed. The framework abstracts complexity, letting you focus on business logic while delivering production-ready capabilities.

### Key Features
- **Unified Development**: Write microservices-ready code once and run it as a monolith for rapid prototyping, then deploy as microservices as needed.
- **Service Orchestration**: Includes service discovery, load balancing, and routing for effortless scaling.
- **Versatile Connectivity**: Supports RSocket, HTTP, and WebSocket for real-time streaming, REST APIs, and more.
- **Persistent Powerhouse**: Integrated persistence layer with optimized query generation, schema evolution, and transaction management.
- **Distributed Resilience**: Native distributed service discovery and event streaming ensure reliable inter-service communication without single points of failure.
- **Fault Tolerance**: Distributes critical functions across nodes for high availability and uninterrupted operation.

### Use Cases
- Rapid prototyping of microservices apps in a monolithic setup.
- Event-driven systems requiring robust communication.
- Scalable, resilient applications that evolve from prototype to production.

---

## Getting Started

### Prerequisites
- **Java**: Version 8
- **Maven**: Version 3.6 or higher
- **Database**: A supported database (e.g., MySQL, PostgreSQL) for persistence

### Installation
1. **Create a New Maven Project**:
   ```bash
   mvn archetype:generate -DgroupId=com.example -DartifactId=mstack-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   cd mstack-app
   ```

2. **Add MStack Dependencies**:

   Example `pom.xml` snippet for a monolithic setup:
   ```xml
   <dependencies>
       <dependency>
           <groupId>com.mstack</groupId>
           <artifactId>mstack</artifactId>
           <version>1.0.0</version>
       </dependency>
       <dependency>
           <groupId>com.mstack</groupId>
           <artifactId>logger-log4j</artifactId>
           <version>1.0.0</version>
       </dependency>
       <dependency>
           <groupId>com.mstack</groupId>
           <artifactId>datajpa-hibernate</artifactId>
           <version>1.0.0</version>
       </dependency>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.26</version>
       </dependency>
   </dependencies>
   ```


3. **Build the Project**:
      ```bash
      mvn clean install
      ```

---

## Architecture

MStack abstracts the complexities of both monolithic and microservices architectures. Its core philosophy is to provide a consistent set of interfaces (via `mstack-core`) that work seamlessly regardless of deployment type:

- **Monolithic Architecture**: All services run in a single JVM with simplified routing and discovery mechanisms.
- **Microservices Architecture**: Services are distributed across processes or containers, leveraging advanced features like distributed service discovery and complex routing.

The transition between these architectures is managed by swapping dependencies (`mstack-core` vs. `mstack`) and configuring Maven profiles—no application code changes are required.

---

## Core Components

MStack is modular, with libraries that can be mixed and matched based on your needs. Below is an overview of each component:


### `mstack`
- **Purpose**: Core library for microservice applications, providing interfaces for:
  - Logging
  - Data persistence
  - Transport
  - Web servers
  - Authentication
  - Monitoring and tracing
  - Complex routing
  - Load balancing
  - Distributed service discovery
- **Usage**: Use for distributed deployments.

### `logger`
- **Purpose**: Abstract logging layer.
- **Implementations**: `logger-log4j` (Log4j-based).

### `datajpa`
- **Purpose**: Abstract persistence layer using JPA.
- **Implementations**: `datajpa-hibernate` (Hibernate-based).

### `service-discovery`
- **Purpose**: Abstract service discovery mechanism.
- **Implementations**: `service-discovery-scalecube` (ScaleCube-based).

### `transport`
- **Purpose**: Handles communication via RSocket, HTTP, and WebSocket.

### `web-server`
- **Purpose**: Provides HTTP and WebSocket servers for service exposure.

### `event-streams`
- **Purpose**: Enables reliable event-based communication between services.

### `authenticator`
- **Purpose**: Manages inter-service authentication (supports basic and JWT).

### `router`
- **Purpose**: Routes messages between services using service names (monolith-focused).

### `complex-router`
- **Purpose**: Advanced routing for microservices, enhancing scalability and availability.

---

## Usage Examples

You can find complete examples here. [Examples repo](https://github.com/Chamith-Nimmitha/mstack-example.git)

### Building a Simple CRUD Service
1. **Define a Service Interface**:
   ```java
   public interface UserService {
       User getUserById(long id);
   }
   ```

2. **Implement the Service**:
   ```java
   public class UserServiceImpl implements UserService {
       private final UserRepository repository;

       public UserServiceImpl(UserRepository repository) {
           this.repository = repository;
       }

       @Override
       public User getUserById(long id) {
           return repository.findById(id).orElse(null);
       }
   }
   ```

3. **Expose via Web Server**:
   ```java
   // Configure in application setup
   webServer.expose(UserService.class, new UserServiceImpl(repository));
   ```

4. **Run as Monolith or Microservices**:
   - Monolith: `mvn clean install -P monolith && java -jar target/mstack-app.jar`
   - Microservices: `mvn clean install -P microservices && java -jar target/mstack-app.jar`

### Event Streaming Example
Publish and subscribe to events:
```java
// Publish an event
eventStreams.publish("user.created", new UserCreatedEvent(user));

// Subscribe to an event
eventStreams.subscribe("user.created", event -> {
    System.out.println("User created: " + event.getUser().getName());
});
```

---

## Advanced Topics

### Customizing Components
- **Custom Logger**: Implement the `logger` interface for a custom logging solution.
- **Custom Persistence**: Extend `datajpa` for alternative ORMs or databases.

### Performance Tuning
- Adjust transport settings (e.g., connection pools) for high-load scenarios.
- Optimize service discovery intervals for large clusters.

### Integration
- Integrate with frontend frameworks like React Vite using `web-server` for REST or WebSocket APIs.

---

## API Reference

Detailed API documentation is available for each library:
- `mstack-core`: [Link to Javadoc]
- `mstack`: [Link to Javadoc]
- `logger`: [Link to Javadoc]
- ...

---

## Troubleshooting

### Common Issues
- **ClassNotFoundException**: Verify all required dependencies are included.
- **Service Not Found**: Ensure service discovery seeds are correctly configured.
- **Database Errors**: Check datasource properties.

### Debugging Tips
- Enable debug logging: `logger.level=DEBUG`
- Use tracing features in `mstack-core` to monitor requests.

---

## FAQ

- **How does MStack differ from Spring Boot?**
  - MStack focuses on seamless monolith-to-microservices transitions with minimal code changes, while Spring Boot requires more reconfiguration for such shifts.
- **Can I use MStack with existing databases?**
  - Yes, via `datajpa` and appropriate connectors (e.g., `mysql-connector-java`).

---

## Contributing

Contributions are welcome! See the [contribution guidelines](link-to-guidelines) for details on submitting issues or pull requests.

---

## License

MStack is distributed under the [Apache License 2.0](link-to-license).

---

This documentation provides a complete guide to MStack, from setup to advanced usage, ensuring developers can leverage its flexibility to build scalable, resilient applications.
