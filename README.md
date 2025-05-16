# Eureka Service Discovery Server

## Table of Contents
1. Overview
2. High-Level Design (HLD)
3. Low-Level Design (LLD)
4. Technical Details
5. Project Structure
6. Setup Instructions
7. References

---

## 1. Overview

**Eureka Service Discovery Server** is a Spring Boot-based microservice that acts as a registry for other microservices in a distributed system. It enables dynamic discovery and registration of services, facilitating load balancing and failover.

---

## 2. High-Level Design (HLD)

### Architecture Diagram

```
+-------------------+
|                   |
|  Eureka Server    |
|  (Service Reg.)   |
|                   |
+---------+---------+
          |
   +------+------+
   |             |
+--v--+      +---v---+
| MS1 |      |  MS2  |
+-----+      +-------+
(Microservices register with Eureka)
```

- **Eureka Server**: Central registry for all microservices.
- **Microservices (Clients)**: Register themselves with Eureka and discover other services.

### Key Features
- Centralized service registry
- Health check and status monitoring
- Dynamic registration and deregistration

---

## 3. Low-Level Design (LLD)

### Components

1. **Eureka Server Application**
   - Main Spring Boot application with `@EnableEurekaServer`
   - Configuration via `application.properties`
2. **Configuration Files**
   - `application.properties`: Contains server port and Eureka-specific settings.
3. **Dependencies**
   - Spring Boot Starter
   - Spring Cloud Netflix Eureka Server

### Sequence Diagram

```
Microservice ----> Eureka Server: Register
Microservice <---- Eureka Server: Registration Success
Microservice ----> Eureka Server: Heartbeat
Microservice <---- Eureka Server: Acknowledgement
```

---

## 4. Technical Details

- **Language**: Java 17+ (recommended)
- **Framework**: Spring Boot 3.x
- **Service Discovery**: Spring Cloud Netflix Eureka Server 4.1.0
- **Build Tool**: Maven
- **Port**: 8761 (default)
- **Configuration**:
  - `eureka.client.register-with-eureka=false`
  - `eureka.client.fetch-registry=false`

---

## 5. Project Structure

```
ServiceDiscovery-EurekaServer-/
│
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── example/
│       │           └── eurekaserver/
│       │               └── EurekaServerApplication.java
│       └── resources/
│           ├── application.properties
│           └── static/
│
├── pom.xml
├── README.md
└── NOTE.md
```

---

## 6. Setup Instructions

1. **Create Spring Boot Project**  
   Use [Spring Initializr](https://start.spring.io/) or your IDE.

2. **Add Dependency**  
   Add the following to your `pom.xml`:
   ```xml
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
       <version>4.1.0</version>
   </dependency>
   ```

3. **Configure Properties**  
   In `src/main/resources/application.properties`:
   ```properties
   server.port=8761
   eureka.client.register-with-eureka=false
   eureka.client.fetch-registry=false
   ```

4. **Enable Eureka Server**  
   In your main application class:
   ```java
   @SpringBootApplication
   @EnableEurekaServer
   public class EurekaServerApplication {
       public static void main(String[] args) {
           SpringApplication.run(EurekaServerApplication.class, args);
       }
   }
   ```

5. **Run the Application**  
   Start the server and access [http://localhost:8761/](http://localhost:8761/).

---

## 7. References

- [Spring Cloud Netflix Eureka Documentation](https://cloud.spring.io/spring-cloud-netflix/reference/html/)
- [Maven Repository: Eureka Server](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-netflix-eureka-server/4.1.0) 