# Patient Management System - Spring Boot Microservices

An enterprise-grade **Patient Management System** built using **Spring Boot Microservices**, **Apache Kafka**, **gRPC**, **PostgreSQL**, **Docker**, **REST APIs**, **JWT Authentication**, and **API Gateway**.

This project demonstrates modern backend engineering concepts such as microservice architecture, event-driven communication, asynchronous messaging, inter-service communication, containerization, API routing, authentication, and integration testing.

## Overview

The Patient Management System is designed as a cloud-native backend application where different business responsibilities are separated into independent microservices.

The system allows patient data to be managed through REST APIs, supports secure authentication through JWT, communicates between services using gRPC, and publishes patient-related events using Apache Kafka for asynchronous processing.

This project is suitable for learning and demonstrating real-world backend architecture used in enterprise applications.

## Features

* Microservices-based backend architecture
* Patient management REST APIs
* API Gateway for centralized routing
* JWT-based authentication and authorization
* Apache Kafka event-driven communication
* gRPC communication between services
* PostgreSQL database support
* Dockerized microservices
* Integration testing module
* AWS CDK / LocalStack infrastructure setup
* OpenAPI / Swagger documentation support
* Clean layered architecture using controller, service, repository, DTO, mapper, and model layers

## Tech Stack

* Java
* Spring Boot
* Spring Web
* Spring Data JPA
* Spring Security
* JWT Authentication
* Apache Kafka
* gRPC
* Protocol Buffers
* PostgreSQL
* H2 Database
* Docker
* Maven
* REST Assured
* JUnit
* AWS CDK
* LocalStack
* API Gateway
* OpenAPI / Swagger

## Microservices

| Service           | Description                                                        | Port           |
| ----------------- | ------------------------------------------------------------------ | -------------- |
| Patient Service   | Handles patient management APIs and patient-related business logic | `4000`         |
| Billing Service   | Handles billing-related operations and exposes gRPC communication  | `4001`, `9001` |
| Analytics Service | Consumes Kafka events and processes analytics-related data         | `4002`         |
| API Gateway       | Routes requests to backend services and applies JWT validation     | `4004`         |
| Auth Service      | Handles user authentication and JWT generation                     | `4005`         |
| Integration Test  | Contains integration tests for validating service behaviour        | N/A            |
| Infrastructure    | Contains AWS CDK and LocalStack deployment configuration           | N/A            |

## Architecture

```txt id="5wkcn4"
Client / API Consumer
        |
        v
API Gateway
        |
        |---------------------> Auth Service
        |
        |---------------------> Patient Service
                                      |
                                      | gRPC
                                      v
                              Billing Service
                                      |
                                      | Kafka Event
                                      v
                              Analytics Service
```

## Project Structure

```bash id="4rztb3"
patient-management-system-springboot-microservices/
├── analytics-service/
│   ├── src/
│   ├── Dockerfile
│   ├── mvnw
│   └── pom.xml
│
├── api-gateway/
│   ├── src/
│   └── Dockerfile
│
├── api-requests/
│
├── auth-service/
│   ├── src/
│   └── Dockerfile
│
├── billing-service/
│   ├── src/
│   ├── Dockerfile
│   └── pom.xml
│
├── grpc-requests/
│   └── billing-service/
│
├── infrastructure/
│   ├── src/
│   ├── localstack-deploy.sh
│   └── pom.xml
│
├── integration-test/
│   ├── src/
│   └── pom.xml
│
├── patient-service/
│   ├── src/
│   ├── Dockerfile
│   ├── mvnw
│   └── pom.xml
│
└── README.md
```

## Service Responsibilities

### Patient Service

The Patient Service is responsible for patient-related operations.

Main responsibilities:

* Create patients
* Update patient details
* Retrieve patient records
* Delete patient records
* Validate patient data
* Publish patient events to Kafka
* Communicate with Billing Service through gRPC

### Billing Service

The Billing Service handles billing-related operations.

Main responsibilities:

* Receive billing requests
* Expose gRPC endpoints
* Process billing communication from Patient Service

### Analytics Service

The Analytics Service listens to Kafka messages and processes patient-related events.

Main responsibilities:

* Consume Kafka events
* Process patient-created or patient-updated events
* Support asynchronous event-driven workflows

### Auth Service

The Auth Service manages authentication and user access.

Main responsibilities:

* User authentication
* JWT token generation
* Role-based access support
* Secure login flow

### API Gateway

The API Gateway acts as the single entry point for external API requests.

Main responsibilities:

* Route requests to backend services
* Protect patient APIs using JWT validation
* Forward authentication requests to Auth Service
* Centralize API access

## Communication Flow

### REST Communication

External clients communicate with the system through REST APIs via the API Gateway.

Example:

```txt id="3mzfcj"
Client -> API Gateway -> Patient Service
```

### gRPC Communication

The Patient Service communicates with the Billing Service using gRPC.

Example:

```txt id="57qt0e"
Patient Service -> Billing Service
```

### Kafka Communication

The Patient Service publishes patient-related events to Kafka.
The Analytics Service consumes those events asynchronously.

Example:

```txt id="y8kcg8"
Patient Service -> Kafka -> Analytics Service
```

## API Gateway Routes

| Route                | Target Service  | Description               |
| -------------------- | --------------- | ------------------------- |
| `/auth/**`           | Auth Service    | Authentication APIs       |
| `/api/patients/**`   | Patient Service | Secured patient APIs      |
| `/api-docs/patients` | Patient Service | Patient API documentation |
| `/api-docs/auth`     | Auth Service    | Auth API documentation    |

## Prerequisites

Before running this project, make sure you have installed:

* Java 21
* Maven
* Docker
* Docker Desktop
* PostgreSQL
* Apache Kafka or Kafka Docker container
* IntelliJ IDEA or another Java IDE

## Getting Started

### 1. Clone the repository

```bash id="37r0zv"
git clone https://github.com/DANUCKSAN/patient-management-system-springboot-microservices.git
```

### 2. Navigate to the project folder

```bash id="8k4vfp"
cd patient-management-system-springboot-microservices
```

## Running Services Manually

Each microservice can be run separately from its own folder.

### Run Patient Service

```bash id="dz7h1o"
cd patient-service
./mvnw spring-boot:run
```

Patient Service runs on:

```txt id="9y2e8i"
http://localhost:4000
```

### Run Billing Service

```bash id="sp6jki"
cd billing-service
mvn spring-boot:run
```

Billing Service runs on:

```txt id="cvv0rn"
http://localhost:4001
```

gRPC server runs on:

```txt id="dtj0o8"
localhost:9001
```

### Run Analytics Service

```bash id="6314ic"
cd analytics-service
./mvnw spring-boot:run
```

Analytics Service runs on:

```txt id="uzg3mi"
http://localhost:4002
```

### Run API Gateway

```bash id="jmj5yf"
cd api-gateway
mvn spring-boot:run
```

API Gateway runs on:

```txt id="vuacx8"
http://localhost:4004
```

### Run Auth Service

```bash id="u9h4h7"
cd auth-service
mvn spring-boot:run
```

Auth Service runs on:

```txt id="03m3o7"
http://localhost:4005
```

## Docker Build

Each service includes a Dockerfile.

### Build Patient Service Docker Image

```bash id="a0eec6"
cd patient-service
docker build -t patient-service .
```

### Build Billing Service Docker Image

```bash id="nyzlpg"
cd billing-service
docker build -t billing-service .
```

### Build Analytics Service Docker Image

```bash id="3vndq5"
cd analytics-service
docker build -t analytics-service .
```

### Build Auth Service Docker Image

```bash id="bhfxay"
cd auth-service
docker build -t auth-service .
```

## Kafka Configuration

The Patient Service publishes Kafka messages using:

```txt id="7d9cn4"
kafka:9092
```

The Analytics Service consumes messages from Kafka using the same Kafka bootstrap server.

Make sure Kafka is running and reachable before starting the Patient Service and Analytics Service.

## Authentication

The system uses JWT authentication. Auth Service is responsible for validating user credentials and generating JWT tokens.

A default test user is inserted through the Auth Service database seed file.

Example test user:

```txt id="bvkx23"
Email: testuser@test.com
Role: ADMIN
```

Use the Auth Service login endpoint to generate a JWT token, then pass the token when calling secured Patient Service APIs through the API Gateway.

## API Documentation

Swagger / OpenAPI documentation is available for supported services.

Example API documentation routes through the gateway:

```txt id="hehpv3"
http://localhost:4004/api-docs/patients
http://localhost:4004/api-docs/auth
```

## Integration Testing

The project includes an `integration-test` module using:

* JUnit
* REST Assured

Run integration tests from the `integration-test` folder:

```bash id="cxbfrc"
cd integration-test
mvn test
```

## Infrastructure

The `infrastructure` module includes AWS CDK and LocalStack-related setup.

Main purpose:

* Simulate AWS infrastructure locally
* Deploy infrastructure using LocalStack
* Practice cloud-native deployment workflows

Run the LocalStack deployment script:

```bash id="99ugjo"
cd infrastructure
./localstack-deploy.sh
```

## Important Ports

| Tool / Service       | Port   |
| -------------------- | ------ |
| Patient Service      | `4000` |
| Billing Service REST | `4001` |
| Billing Service gRPC | `9001` |
| Analytics Service    | `4002` |
| API Gateway          | `4004` |
| Auth Service         | `4005` |
| Kafka                | `9092` |

## Key Learning Outcomes

This project demonstrates practical knowledge of:

* Designing microservices with Spring Boot
* Building REST APIs
* Implementing service-to-service communication with gRPC
* Using Apache Kafka for asynchronous messaging
* Securing APIs with JWT authentication
* Routing requests through an API Gateway
* Dockerizing Spring Boot applications
* Running integration tests with REST Assured
* Structuring enterprise-grade backend projects
* Working with AWS CDK and LocalStack

## Future Improvements

* Add Docker Compose for all services
* Add Kubernetes deployment files
* Add centralized logging with ELK or Grafana Loki
* Add monitoring using Prometheus and Grafana
* Add service discovery using Eureka or Consul
* Add circuit breaker using Resilience4j
* Add distributed tracing with OpenTelemetry
* Add CI/CD pipeline using GitHub Actions
* Add frontend dashboard for patient management
* Improve API documentation with complete Swagger examples

## Repository Description

Enterprise-grade Patient Management System built with Spring Boot Microservices, Apache Kafka, gRPC, PostgreSQL, Docker, JWT Authentication, API Gateway, and REST APIs.

## Topics

```txt id="47a5f2"
spring-boot
microservices
java
kafka
grpc
protobuf
postgresql
docker
api-gateway
spring-security
jwt-authentication
rest-api
junit
rest-assured
integration-testing
aws
localstack
cloud-native
backend-development
healthcare-application
```

## Author

**Danucksan**

GitHub: DANUCKSAN

## License

This project is open-source and available for learning, portfolio, and demonstration purposes.
