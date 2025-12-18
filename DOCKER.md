# Docker Deployment Guide

## Overview

This guide provides instructions for building and deploying the Spring Boot Hello World application as a Docker container.

## Prerequisites

- **Docker**: Version 20.10 or higher
- **Docker Compose**: Version 1.29 or higher (optional, for orchestration)
- **Git**: For cloning the repository (optional)

### Installation

**Docker (Ubuntu/Linux):**
```bash
sudo apt-get update
sudo apt-get install docker.io docker-compose
sudo usermod -aG docker $USER
```

**Docker (macOS/Windows):**
Download Docker Desktop from [docker.com](https://www.docker.com/products/docker-desktop)

## Project Structure

```
.
├── Dockerfile              # Multi-stage Docker build configuration
├── docker-compose.yml      # Docker Compose orchestration
├── pom.xml                 # Maven configuration
├── README.md               # Project README
├── DOCKER.md               # This file
└── src/
    ├── main/
    │   ├── java/
    │   │   └── com/example/
    │   │       ├── HelloWorldApplication.java
    │   │       └── HelloController.java
    │   └── resources/
    │       └── application.properties
    └── test/
        └── java/
            └── com/example/
                └── HelloWorldApplicationTests.java
```

## Building the Docker Image

### Option 1: Manual Docker Build

```bash
# Navigate to project directory
cd /workspaces/test-test

# Build the Docker image
docker build -t helloworld-app:1.0.0 .

# Verify the image was created
docker images | grep helloworld-app
```

### Option 2: Using Docker Compose

```bash
# Navigate to project directory
cd /workspaces/test-test

# Build using docker-compose
docker-compose build
```

## Running the Container

### Option 1: Docker Run

```bash
# Run the container
docker run -d \
  --name helloworld-app \
  -p 8080:8080 \
  helloworld-app:1.0.0

# View container logs
docker logs -f helloworld-app

# Stop the container
docker stop helloworld-app

# Remove the container
docker rm helloworld-app
```

### Option 2: Docker Compose

```bash
# Start the container
docker-compose up -d

# View logs
docker-compose logs -f

# Stop the container
docker-compose down
```

### Option 3: Interactive Mode

```bash
# Run in foreground to see logs
docker run -it \
  --name helloworld-app \
  -p 8080:8080 \
  helloworld-app:1.0.0
```

## Accessing the Application

Once the container is running, access the application at:

- **Home endpoint**: http://localhost:8080/
- **Hello endpoint**: http://localhost:8080/hello
- **Hello with name**: http://localhost:8080/hello?name=Docker

### Test with curl

```bash
# Test home endpoint
curl http://localhost:8080/

# Test hello endpoint
curl http://localhost:8080/hello

# Test with parameter
curl "http://localhost:8080/hello?name=Spring"
```

## Container Management

### View Running Containers

```bash
docker ps
docker ps -a    # All containers including stopped
```

### View Container Logs

```bash
docker logs helloworld-app
docker logs -f helloworld-app      # Follow logs in real-time
docker logs --tail 100 helloworld-app    # Last 100 lines
```

### Execute Commands in Container

```bash
# Open a shell in the container
docker exec -it helloworld-app /bin/sh

# Run a command
docker exec helloworld-app java -version
```

### Monitor Container Stats

```bash
docker stats helloworld-app
```

## Environment Variables

Current configuration uses default values. To customize, modify `docker-compose.yml`:

```yaml
environment:
  - SERVER_PORT=8080
```

Or with docker run:

```bash
docker run -d \
  -p 8080:8080 \
  -e SERVER_PORT=8080 \
  helloworld-app:1.0.0
```

## Docker Image Details

### Dockerfile Stages

**Stage 1: Builder**
- Base image: `maven:3.9.6-eclipse-temurin-17`
- Purpose: Compile and package the application
- Produces: JAR file

**Stage 2: Runtime**
- Base image: `eclipse-temurin:17-jre-noble`
- Purpose: Run the application
- Size: ~300MB (optimized with multi-stage build)

### Multi-stage Build Benefits

- **Reduced Image Size**: Only runtime dependencies included
- **Security**: Build tools not present in final image
- **Performance**: Faster deployment and startup

## Troubleshooting

### Container Won't Start

```bash
# Check logs for errors
docker logs helloworld-app

# Verify image exists
docker images | grep helloworld-app

# Rebuild the image
docker build -t helloworld-app:1.0.0 --no-cache .
```

### Port Already in Use

```bash
# Find process using port 8080
lsof -i :8080

# Use different port
docker run -p 8081:8080 helloworld-app:1.0.0
```

### Container Exits Immediately

```bash
# Run in foreground to see errors
docker run -it helloworld-app:1.0.0

# Check logs
docker logs helloworld-app
```

## Cleanup

### Remove Container

```bash
docker stop helloworld-app
docker rm helloworld-app
```

### Remove Image

```bash
docker rmi helloworld-app:1.0.0
```

### Clean Up All

```bash
# Remove stopped containers
docker container prune

# Remove unused images
docker image prune

# Remove all unused objects (containers, images, networks, volumes)
docker system prune -a
```

## Docker Compose Commands Reference

```bash
# Start services in background
docker-compose up -d

# Start services in foreground
docker-compose up

# Stop services
docker-compose stop

# Stop and remove containers
docker-compose down

# View service status
docker-compose ps

# View logs
docker-compose logs -f

# Rebuild images
docker-compose build --no-cache

# Execute command in service
docker-compose exec helloworld-app /bin/sh
```

## Production Deployment Considerations

1. **Use Specific Tags**: Instead of `latest`, use semantic versioning
   ```bash
   docker build -t helloworld-app:1.0.0 .
   ```

2. **Set Resource Limits**:
   ```yaml
   services:
     helloworld-app:
       resources:
         limits:
           cpus: '1'
           memory: 512M
         reservations:
           cpus: '0.5'
           memory: 256M
   ```

3. **Health Checks**:
   ```yaml
   services:
     helloworld-app:
       healthcheck:
         test: ["CMD", "curl", "-f", "http://localhost:8080/"]
         interval: 30s
         timeout: 10s
         retries: 3
         start_period: 40s
   ```

4. **Logging**: Configure log drivers for centralized logging

5. **Security**: 
   - Use non-root users in Dockerfile
   - Scan images for vulnerabilities
   - Keep base images updated

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Best Practices](https://docs.docker.com/develop/dev-best-practices/)
- [Spring Boot Docker Guide](https://spring.io/guides/gs/spring-boot-docker/)
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)

## Quick Start Summary

```bash
# Clone or navigate to project
cd /workspaces/test-test

# Build image
docker build -t helloworld-app:1.0.0 .

# Run container
docker run -d -p 8080:8080 --name helloworld-app helloworld-app:1.0.0

# Test application
curl http://localhost:8080/

# View logs
docker logs -f helloworld-app

# Stop container
docker stop helloworld-app
```
