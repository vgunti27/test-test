# Hello World Spring Boot Application - Complete Documentation

## Table of Contents

1. [Project Overview](#project-overview)
2. [Quick Start](#quick-start)
3. [Project Structure](#project-structure)
4. [Building the Application](#building-the-application)
5. [Running Locally](#running-locally)
6. [Docker Deployment](#docker-deployment)
7. [API Endpoints](#api-endpoints)
8. [Testing](#testing)
9. [Troubleshooting](#troubleshooting)
10. [Technologies Used](#technologies-used)

## Project Overview

A simple yet fully-functional Spring Boot REST API application that demonstrates basic web service concepts. The application provides endpoints for greeting messages with the ability to customize names.

**Key Features:**
- Spring Boot 3.2.1 framework
- RESTful API endpoints
- Maven-based build system
- Docker containerization
- Unit tests included
- Development-ready configuration

## Quick Start

### Prerequisites

- **Java**: 17 or higher
- **Maven**: 3.6 or higher
- **Docker** (optional): 20.10 or higher for containerization

### Start in 30 seconds

```bash
# 1. Clone or navigate to the project
cd /workspaces/test-test

# 2. Build the project
mvn clean package

# 3. Run the application
mvn spring-boot:run

# 4. Test the API
curl http://localhost:8080/
```

The application will be available at `http://localhost:8080`

## Project Structure

```
/workspaces/test-test/
├── src/
│   ├── main/
│   │   ├── java/com/example/
│   │   │   ├── HelloWorldApplication.java      # Main Spring Boot application class
│   │   │   └── HelloController.java             # REST controller with endpoints
│   │   └── resources/
│   │       └── application.properties           # Application configuration
│   └── test/
│       └── java/com/example/
│           └── HelloWorldApplicationTests.java # Unit tests
├── pom.xml                                      # Maven configuration
├── Dockerfile                                   # Docker build configuration
├── docker-compose.yml                          # Docker Compose orchestration
├── README.md                                    # Basic README
├── DOCKER.md                                    # Docker deployment guide
└── DOCUMENTATION.md                            # This file
```

## Building the Application

### Maven Build

```bash
# Clean and build
mvn clean package

# Output: target/helloworld-app-1.0.0.jar

# Build without running tests
mvn clean package -DskipTests

# Build with test execution
mvn clean package
```

### Build Output

Successful build produces:
- `target/helloworld-app-1.0.0.jar` - Executable JAR file
- `target/` - Directory with compiled classes and dependencies

## Running Locally

### Option 1: Spring Boot Maven Plugin

```bash
mvn spring-boot:run
```
- Starts on http://localhost:8080
- Press `Ctrl+C` to stop

### Option 2: Execute JAR File

```bash
# First build the package
mvn clean package

# Run the JAR
java -jar target/helloworld-app-1.0.0.jar
```

### Option 3: IDE Integration

**VS Code:**
1. Install Extension Pack for Java
2. Open project
3. Run `HelloWorldApplication.java` from the IDE

**IntelliJ IDEA:**
1. Import project as Maven project
2. Right-click `HelloWorldApplication.java` → Run

## Docker Deployment

### Quick Docker Start

```bash
# Build Docker image
docker build -t helloworld-app:1.0.0 .

# Run container
docker run -d -p 8080:8080 --name helloworld-app helloworld-app:1.0.0

# Test
curl http://localhost:8080/
```

### Using Docker Compose

```bash
# Start application
docker-compose up -d

# View logs
docker-compose logs -f

# Stop application
docker-compose down
```

### Detailed Docker Guide

See [DOCKER.md](DOCKER.md) for comprehensive Docker documentation including:
- Multi-stage builds
- Container management
- Environment configuration
- Troubleshooting
- Production deployment

## API Endpoints

### 1. Home Endpoint

**Request:**
```
GET /
```

**Response:**
```
Hello World!
```

**curl example:**
```bash
curl http://localhost:8080/
```

### 2. Hello Endpoint

**Request:**
```
GET /hello
```

**Response:**
```
Hello, World!
```

**curl example:**
```bash
curl http://localhost:8080/hello
```

### 3. Personalized Hello Endpoint

**Request:**
```
GET /hello?name=YourName
```

**Response:**
```
Hello, YourName!
```

**curl examples:**
```bash
curl "http://localhost:8080/hello?name=Spring"
curl "http://localhost:8080/hello?name=Docker"
curl "http://localhost:8080/hello?name=Maven"
```

## Testing

### Run All Tests

```bash
mvn test
```

### Run Specific Test

```bash
mvn test -Dtest=HelloWorldApplicationTests
```

### Test with Coverage

```bash
mvn test jacoco:report
# Coverage report: target/site/jacoco/index.html
```

### Manual API Testing

**Using curl:**
```bash
# Start application
mvn spring-boot:run &

# Wait for startup
sleep 5

# Test endpoints
curl http://localhost:8080/
curl http://localhost:8080/hello
curl "http://localhost:8080/hello?name=Test"
```

**Using Postman:**
1. Import or create request to `http://localhost:8080/`
2. Create GET request
3. Send and verify response

**Using browser:**
Simply navigate to:
- http://localhost:8080/
- http://localhost:8080/hello
- http://localhost:8080/hello?name=Browser

## Configuration

### Application Properties

Edit `src/main/resources/application.properties`:

```properties
# Application name
spring.application.name=helloworld-app

# Server port (default 8080)
server.port=8080

# Additional configuration options
logging.level.root=INFO
logging.level.com.example=DEBUG
```

### Custom Configuration Example

```properties
# Security
server.servlet.session.timeout=30m

# Logging
logging.file.name=logs/helloworld-app.log
logging.level.org.springframework.web=DEBUG

# Performance
server.tomcat.threads.max=200
server.tomcat.threads.min-spare=10
```

## Troubleshooting

### Issue: Port 8080 Already in Use

**Solution 1:** Change port in `application.properties`
```properties
server.port=8081
```

**Solution 2:** Kill existing process
```bash
lsof -i :8080      # Find process
kill -9 <PID>      # Kill process
```

### Issue: Build Fails with Maven

```bash
# Clear Maven cache
rm -rf ~/.m2/repository

# Try build again
mvn clean package
```

### Issue: Java Version Mismatch

```bash
# Check Java version
java -version

# Install Java 17 if needed
sudo apt-get install openjdk-17-jdk
```

### Issue: Docker Build Fails

```bash
# Rebuild without cache
docker build --no-cache -t helloworld-app:1.0.0 .

# Check Docker daemon
docker ps

# View build logs
docker build -t helloworld-app:1.0.0 . --progress=plain
```

### Issue: Application Won't Start

```bash
# Check logs
mvn spring-boot:run 2>&1 | head -50

# Verify pom.xml syntax
mvn clean validate

# Check dependencies
mvn dependency:tree
```

## Technologies Used

| Technology | Version | Purpose |
|-----------|---------|---------|
| Spring Boot | 3.2.1 | Framework |
| Spring Web | 3.2.1 | REST API |
| Java | 17 | Language |
| Maven | 3.9.6 | Build Tool |
| JUnit 5 | Latest | Testing |
| Docker | 20.10+ | Containerization |
| Eclipse Temurin | 17 | JRE |

## Lifecycle Management

### Development Workflow

```bash
# 1. Make changes to code
# 2. Run tests
mvn test

# 3. Build
mvn clean package

# 4. Run locally
mvn spring-boot:run

# 5. Test endpoints
curl http://localhost:8080/hello?name=Dev

# 6. Commit changes
git add .
git commit -m "Feature: Add new endpoint"
```

### Deployment Workflow

```bash
# 1. Build and test
mvn clean package

# 2. Build Docker image
docker build -t helloworld-app:1.0.0 .

# 3. Test Docker image
docker run -it -p 8080:8080 helloworld-app:1.0.0

# 4. Push to registry (optional)
docker tag helloworld-app:1.0.0 myregistry/helloworld-app:1.0.0
docker push myregistry/helloworld-app:1.0.0

# 5. Deploy with docker-compose
docker-compose up -d
```

## Performance Considerations

- **Startup Time**: ~3-5 seconds
- **Memory Usage**: ~150-200MB in Docker container
- **Response Time**: <5ms for API endpoints
- **Concurrent Users**: 200+ with default configuration

## Security Considerations

- Application runs on standard port 8080
- No authentication/authorization implemented (consider adding Spring Security for production)
- CORS not enabled by default
- No HTTPS/SSL configured (configure for production)

## Next Steps & Enhancements

### Potential Improvements

1. **Add More Endpoints**
   ```java
   @GetMapping("/api/greet/{name}")
   public Greeting greetUser(@PathVariable String name) {
       return new Greeting(LocalDateTime.now(), "Hello, " + name);
   }
   ```

2. **Add Database**
   - Spring Data JPA
   - Configure H2 or PostgreSQL

3. **Add Logging**
   - SLF4J with Logback
   - Structured logging

4. **Add Security**
   - Spring Security
   - JWT authentication

5. **Add Validation**
   - Bean Validation (JSR-380)
   - Custom validators

6. **Add Monitoring**
   - Spring Boot Actuator
   - Prometheus metrics
   - Grafana dashboards

## Support & Resources

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Framework Documentation](https://spring.io/projects/spring-framework)
- [Maven Documentation](https://maven.apache.org/)
- [Docker Documentation](https://docs.docker.com/)
- [Java Documentation](https://docs.oracle.com/javase/)

## License

This project is provided as-is for educational and development purposes.

---

**Last Updated:** December 18, 2025
**Version:** 1.0.0
