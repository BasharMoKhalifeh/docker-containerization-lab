# Docker Containerization Lab

A hands-on Docker lab focused on containerizing applications, managing images and containers, networking, port mapping, resource limits, and running .NET applications in containers.

## Overview

This project documents the Docker fundamentals and practical tasks completed during my DevOps training. The goal was to understand how applications are packaged, isolated, connected, and deployed using containers.

## What I Implemented

- Built Docker images using Dockerfiles
- Used multi-stage Docker builds for .NET applications
- Created and managed Docker containers
- Configured port mapping between host and container
- Created custom Docker networks for service-to-service communication
- Used Docker volumes for persistent container data
- Applied container resource limits
- Tested running services with `curl`
- Worked with private container images and a local Docker registry
- Troubleshot common container and networking issues

## Technologies

- Docker
- Docker Compose
- .NET 8
- Linux
- Nginx
- REST APIs
- Docker Networking
- Docker Volumes

## Project Structure

```text
.
├── Dockerfile
├── docker-compose.yml
├── .dockerignore
└── README.md
```

## Dockerfile Concepts

The Dockerfile demonstrates a typical multi-stage build:

1. **Build stage** - uses the .NET SDK image to restore dependencies and publish the application.
2. **Runtime stage** - uses the smaller .NET runtime image to run the published application.
3. Only the published output is copied into the final image, reducing unnecessary build-time dependencies.

## Build and Run

Build the image:

```bash
docker build -t docker-containerization-lab:1.0 .
```

Run a container:

```bash
docker run -d \
  --name docker-lab-app \
  -p 8080:8080 \
  docker-containerization-lab:1.0
```

Check running containers:

```bash
docker ps
```

Check application logs:

```bash
docker logs docker-lab-app
```

Test the service:

```bash
curl http://localhost:8080
```

Stop and remove the container:

```bash
docker stop docker-lab-app
docker rm docker-lab-app
```

## Docker Networking

A custom bridge network was used to allow containers to communicate using container/service names instead of relying on host networking.

```bash
docker network create lab-network

docker network ls
```

Inspect the network:

```bash
docker network inspect lab-network
```

## Port Mapping

Docker port publishing maps a host port to a container port:

```text
Host                    Container
8080  ----------------> 8080
```

Example:

```bash
docker run -d -p 8080:8080 docker-containerization-lab:1.0
```

The application listens on port `8080` inside the container while users access it through port `8080` on the host.

## Resource Limits

Container resource limits were tested to understand how Docker controls resource consumption.

Example:

```bash
docker run -d \
  --name nginx-test \
  --memory=100m \
  -p 8081:80 \
  nginx:latest
```

The `--memory=100m` option limits the container's memory allocation to 100 MB.

## Docker Volumes

Named volumes provide persistent storage independently from a container's lifecycle.

```bash
docker volume create app-data
docker volume ls
docker volume inspect app-data
```

## Docker Compose

The included Compose configuration demonstrates how application services and their networking can be defined declaratively.

Start the services:

```bash
docker compose up -d
```

View services:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs -f
```

Stop the stack:

```bash
docker compose down
```

## Private Registry

As part of the broader Docker work, I also worked with a local Docker Registry behind Nginx with TLS. This introduced practical experience with:

- Image tagging
- `docker push` / `docker pull`
- Registry authentication/trust considerations
- TLS certificates
- Nginx reverse proxy configuration
- SELinux-related file access troubleshooting

The private registry work is documented separately in the `private-docker-registry` project.

## Troubleshooting Experience

During the lab I worked through issues involving:

- Container port accessibility
- Docker network connectivity
- Image pull failures
- TLS certificate trust problems
- Nginx access to certificate files under SELinux
- Container resource limits
- Application startup and runtime configuration

## Key Learning Outcomes

This lab strengthened my understanding of Docker as a core DevOps technology. I learned how to package applications consistently, isolate workloads, connect services through Docker networking, persist data with volumes, control resources, and prepare container images for use in CI/CD pipelines and private registries.

## Future Improvements

- Add automated image building through GitLab CI/CD
- Push images to the private registry automatically
- Add container health checks
- Add image vulnerability scanning
- Integrate Prometheus monitoring
- Add automated testing to the container build pipeline
