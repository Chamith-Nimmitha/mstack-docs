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

MStack is a powerful Java framework that enables developers to create applications with a single codebase that can seamlessly operate as a monolithic application or a distributed microservices architecture. This flexibility eliminates the need for code rewrites when scaling from a simple prototype to a production-grade distributed system.

### Why MStack?
MStack provides a highly customizable and feature-rich foundation for building applications. Start with a monolithic architecture for simplicity and speed, then scale to microservices as your application grows—all without changing your implementation code. This adaptability is achieved through modular dependencies and Maven profiles, allowing you to focus on business logic while MStack handles the underlying infrastructure.

### Key Features
- **Dynamic Deployment**: Toggle between monolith and microservices deployments using Maven profiles, with no code modifications.
- **Service Orchestration**: Includes service discovery, load balancing, and routing for effortless scaling.
- **Versatile Connectivity**: Supports RSocket, HTTP, and WebSocket for real-time streaming, REST APIs, and more.
- **Persistent Powerhouse**: Integrated persistence layer with optimized query generation, schema evolution, and transaction management.
- **Distributed Resilience**: Native distributed service discovery and event streaming ensure reliable inter-service communication without single points of failure.
- **Fault Tolerance**: Distributes critical functions across nodes for high availability and uninterrupted operation.

### Use Cases
- Rapid prototyping with a monolithic setup, scalable to microservices.
- Real-time, event-driven applications requiring robust communication.
- Resilient, distributed systems with minimal configuration overhead.

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
   - For a **monolith**, use `mstack-core` and required implementations.
   - For **microservices**, use `mstack` and additional dependencies.

   Example `pom.xml` snippet for a monolithic setup:
   ```xml
   <dependencies>
       <dependency>
           <groupId>com.mstack</groupId>
           <artifactId>mstack-core</artifactId>
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

3. **Set Up Maven Profiles**:
   Define profiles in `pom.xml` to switch between architectures:
   ```xml
   <profiles>
       <profile>
           <id>monolith</id>
           <dependencies>
               <dependency>
                   <groupId>com.mstack</groupId>
                   <artifactId>mstack-core</artifactId>
                   <version>1.0.0</version>
               </dependency>
           </dependencies>
       </profile>
       <profile>
           <id>microservices</id>
           <dependencies>
               <dependency>
                   <groupId>com.mstack</groupId>
                   <artifactId>mstack</artifactId>
                   <version>1.0.0</version>
               </dependency>
           </dependencies>
       </profile>
   </profiles>
   ```

4. **Build the Project**:
   - Monolith: `mvn clean install -P monolith`
   - Microservices: `mvn clean install -P microservices`

---

## Architecture

MStack abstracts the complexities of both monolithic and microservices architectures. Its core philosophy is to provide a consistent set of interfaces (via `mstack-core`) that work seamlessly regardless of deployment type:

- **Monolithic Architecture**: All services run in a single JVM with simplified routing and discovery mechanisms.
- **Microservices Architecture**: Services are distributed across processes or containers, leveraging advanced features like distributed service discovery and complex routing.

The transition between these architectures is managed by swapping dependencies (`mstack-core` vs. `mstack`) and configuring Maven profiles—no application code changes are required.

---

## Core Components

MStack is modular, with libraries that can be mixed and matched based on your needs. Below is an overview of each component:

### `mstack-core`
- **Purpose**: Core library for monolithic applications, providing interfaces for:
  - Logging
  - Data persistence
  - Transport
  - Web servers
  - Authentication
  - Inter-service communication
  - Monitoring and tracing
- **Usage**: Foundation for all MStack applications.

### `mstack`
- **Purpose**: Extends `mstack-core` with microservices features:
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

## Configuration

### Maven Profiles
Switch between architectures by activating profiles:
- **Monolith**: `-P monolith`
- **Microservices**: `-P microservices`

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
