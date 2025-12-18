# Hello World Spring Boot Application

A simple Spring Boot application that provides REST endpoints returning "Hello World" messages.

## Features

- Spring Boot 3.2.1
- Java 17
- Maven build system
- REST API endpoints

## Endpoints

- `GET /` - Returns "Hello World!"
- `GET /hello?name=YourName` - Returns personalized greeting

## Prerequisites

- Java 17 or higher
- Maven 3.6 or higher

## Build

```bash
mvn clean package
```

## Run

```bash
mvn spring-boot:run
```

Or after building:

```bash
java -jar target/helloworld-app-1.0.0.jar
```

## Test

```bash
mvn test
```

The application will start on `http://localhost:8080`

## API Examples

```bash
curl http://localhost:8080/
curl http://localhost:8080/hello
curl http://localhost:8080/hello?name=Spring
```
