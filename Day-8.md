# Docker: A Comprehensive Guide

## Table of Contents
1. [What is Docker?](#what-is-docker)
2. [Why Use Docker?](#why-use-docker)
3. [Real-Life Use Case](#real-life-use-case)
4. [Getting Started with Docker](#getting-started-with-docker)
5. [Conclusion](#conclusion)

---

## What is Docker?

Docker is an open-source platform that automates the deployment, scaling, and management of applications in lightweight, portable containers. A container is a self-contained unit that includes everything needed to run an application: the code, runtime, libraries, and dependencies.

Docker allows developers to package applications and their dependencies into containers, ensuring they run consistently across different environments. This is especially useful in cloud computing and modern application development, where the ability to quickly and reliably deploy applications is key.

---

## Why Use Docker?

### 1. **Portability**
   Docker containers encapsulate everything needed to run an application. This means that if you can run Docker on one machine, you can run the same container on any other machine that supports Docker—whether it's your local laptop, a test server, or a cloud-based server. This drastically reduces issues like "it works on my machine."

### 2. **Consistency Across Environments**
   Docker eliminates the "works on my machine" problem by ensuring that an application will run the same regardless of the environment. Whether you're developing locally, testing, or deploying to production, Docker guarantees that the software behaves consistently.

### 3. **Scalability**
   Docker makes it easy to scale your application. Since containers are lightweight and can start quickly, you can deploy many instances of your application without heavy resource consumption. Docker integrates seamlessly with orchestration tools like Kubernetes, allowing you to manage large-scale applications across multiple servers.

### 4. **Isolation**
   Docker containers provide a level of isolation between applications, allowing multiple containers to run on the same system without interfering with each other. This makes it ideal for microservices architectures, where different components of an application are isolated in separate containers.

### 5. **Version Control and Reproducibility**
   Docker images are versioned, so you can track changes and roll back to previous versions if needed. This makes it easy to reproduce environments, debug, and ensure that developers and operations teams are always working with the same environment.

---

## Real-Life Use Case: **Deploying a Multi-Component Web Application**

Let's imagine you're building a **web application** that consists of several components:
- A **frontend** built with React.js
- A **backend** API built with Node.js
- A **database** using PostgreSQL

### Without Docker:
- You would need to manually install the necessary dependencies (Node.js, PostgreSQL, etc.) on every machine (local or server) where the app runs.
- Every developer or team member must replicate the same setup on their machines, which can lead to differences in versions, conflicting dependencies, and potential errors like "it works on my machine" issues.

### With Docker:
- You can create a Docker container for each component of your app: one for the React.js frontend, one for the Node.js backend, and one for the PostgreSQL database.
- Each container can include only the exact dependencies it needs, ensuring the app works the same way on every developer’s machine, in staging, and in production.
- Docker also allows you to define and manage the relationships between the containers (e.g., backend connecting to the database) via `docker-compose`.

This approach simplifies setup, enhances collaboration, and speeds up development cycles by ensuring consistency between all environments.

---

## Getting Started with Docker

To get started with Docker, follow these steps:

### 1. **Install Docker**
   - Visit the [Docker installation page](https://www.docker.com/get-started) and follow the instructions to install Docker Desktop for your operating system.
   
### 2. **Create a Dockerfile**
   - A `Dockerfile` is a script that defines the environment in which your application will run. Here's an example `Dockerfile` for a simple Node.js application:
   ```Dockerfile
   # Use official Node.js image
   FROM node:14

   # Set the working directory
   WORKDIR /app

   # Copy package.json and install dependencies
   COPY package*.json ./
   RUN npm install

   # Copy the rest of the application code
   COPY . .

   # Expose the port the app will run on
   EXPOSE 3000

   # Start the application
   CMD ["npm", "start"]
```
 **Create a Dockerfile for Your Java Application**

   A `Dockerfile` defines the environment in which your Java application will run. Here's an example of a Dockerfile for a **Spring Boot** application:
   
   ```Dockerfile
   # Use a base image with Java 11
   FROM openjdk:11-jre-slim

   # Set the working directory inside the container
   WORKDIR /app

   # Copy the built JAR file into the container
   COPY target/my-java-app.jar /app/my-java-app.jar

   # Expose the port your application runs on
   EXPOSE 8080

   # Command to run the application
   CMD ["java", "-jar", "my-java-app.jar"]
### Build and Run Your Docker Image
After creating your Dockerfile, you can build and run the Docker image using the following commands:

# Build the Docker image

docker build -t my-java-app .

docker build -t my-node-app .

# Run the container
docker run -p 3000:3000 my-node-app
docker run -p 8080:8080 my-java-app

### Use Docker Compose for Multi-Container Applications
```
```
version: '3'

services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
  backend:
    build: ./backend
    ports:
      - "4000:4000"
    depends_on:
      - db
  db:
    image: postgres
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: mydb
```
# To start all services with Docker Compose, run:
docker-compose up

## To use docker in Jenkins add these :
```
stage('Build Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'c9b058e5-bfe6-41f8-9b5d-dc0b0d2955ac', toolName: 'docker') {
                        sh "docker build -t shopping-cart -f docker/Dockerfile ."
                        sh "docker tag shopping-cart username-docker/shopping-cart:latest"
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'c9b058e5-bfe6-41f8-9b5d-dc0b0d2955ac', toolName: 'docker') {
                        sh "docker push username-docker/shopping-cart:latest"
                    }
                }
            }
        }
        
        stage('Deploy To Docker Container') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'c9b058e5-bfe6-41f8-9b5d-dc0b0d2955ac', toolName: 'docker') {
                        sh "docker run -d --name shopping -p 8070:8070 username-docker/shopping-cart:latest"
                    }
                }
            }
        }
    }
}
```
