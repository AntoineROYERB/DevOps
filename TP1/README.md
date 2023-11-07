# [TP1 - Docker](http://school.pages.takima.io/devops-resources/ch1-discover-docker-tp/)


This repository contains a Docker project as part of a DevOps course. The project comprises a web application, a PostgreSQL database, and a backend API, all running within Docker containers.

## Overview of Docker Commands

Here is a list of commonly used Docker commands:

### Docker

The basic command for interacting with Docker. It allows you to manage containers, images, and various aspects of the Docker system.

### Docker build

This command is used to create a Docker image from a Dockerfile. Commonly used options include:

- `-t`: Specifies a name and tag for the image.

### docker run

This command starts a Docker container from an image. Commonly used options include:

- `-d`: Runs the container in detached mode.
- `--name`: Assigns a name to the container.
- `--network`: Connects the container to a specific network.
- `-p`: Maps container ports to host ports.

### docker rm

This command is used to remove a Docker container. Commonly used options include:

- `-f`: Forces removal, even if the container is running.

### docker exec

Allows you to run a command within a running container. Commonly used options include:

- `-it`: Runs the command interactively (useful for interacting with the container's terminal).

# TP1

Link to TP1: [TP1 Discover Docker](http://school.pages.takima.io/devops-resources/ch1-discover-docker-tp/#backend-api_1)

## Getting Started
Note: A PostgreSQL database typically uses port 5432 by default.

Here are the key steps to build a three-tier application:

A Docker network is a virtual infrastructure that allows containers to communicate with each other or with the external world while isolating them from each other. This facilitates the deployment of containerized applications while maintaining security and container separation.

Create three directories within a directory: `back`, `front`, and `database`.

1. **Create a Docker Network**:

   ```bash
   docker network create app-network
    ```
2. **Create a Dockerfile for the database**:

   ```bash
   $ cd ~/Documents/database
   $ docker build -t royerbantoine/database .
    ```

    Pour vizualiser la database:

    ```bash
    $ docker run --network app-network --name adminer -p 8080:8080 adminer
    ```

    Adminer is an open-source database management tool that provides a graphical user interface for interacting with various database management systems, including MySQL, PostgreSQL, SQLite, MS SQL, Oracle, and many others.

3. **Create a Dockerfile for the API** :

   ```bash
    $ cd ~/Documents/back
    $ docker build -t royerbantoine/back .
    ```
4. **Create a Dockerfile for the front** :

   ```bash
    $ cd ~/Documents/front
    $ docker build -t royerbantoine/front .
    ```
5. **Run images** :

   ```bash
    $ docker run -d --name database --network app-network -d royerbantoine/database
    $ docker run -d --name front --network app-network -p 80:80 -d royerbantoine/front
    $ docker run -d --name back --network app-network -d royerbantoine/back
    ```

# Reverse proxy
To copy the Apache2 configuration file to the "front" container:

```bash
docker exec front cp /usr/local/apache2/conf/httpd.conf .
```
After modifying the configuration file, you should place it in the "front" directory and restart the "front" image, specifying the new configuration file to use in the Dockerfile for the "front" container.


# Using docker-compose

After installing Docker Compose and creating the docker-compose.yml, you can launch all three images with a single command.

```bash
$ cd ~/Documents
$ docker-compose build
$ docker-compose up
```

# Publishing on Docker Hub

To share our work with the community, we can upload our created image to Docker Hub. To do this, you need to create an account on Docker Hub and then follow the steps below:

```bash 
$ docker login
$ docker tag tp1 royerbantoine/tp1:1.0
$ docker push royerbantoine/tp1:1.0
```

# Answers to questions
###  1-0 Why should we run the container with a flag -e to give the environment variables?

Using the -e flag when running allows you to configure your container's behavior using environment variables, providing a flexible and secure way to manage your application's settings.

### 1-1 Why do we need a volume to be attached to our postgres container?

Attaching a volume to a PostgreSQL container is crucial for preserving the database's data integrity and ensuring data persistence. Without a volume, data would be lost when the container is recreated or updated. Volumes also enable easy backups, recovery, and data sharing.

### 1-2 Why do we need a multistage build? And explain each step of this dockerfile.

```
# Build
FROM maven:3.8.6-amazoncorretto-17 AS myapp-build
ENV MYAPP_HOME /opt/myapp
WORKDIR $MYAPP_HOME
COPY pom.xml .
COPY src ./src
RUN mvn package -DskipTests

# Run
FROM amazoncorretto:17
ENV MYAPP_HOME /opt/myapp
WORKDIR $MYAPP_HOME
COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar

ENTRYPOINT java -jar myapp.jar
```
A multi-stage build in a Dockerfile is used to create a smaller and more efficient Docker image. It involves using two or more `FROM` instructions, each representing a separate stage in the build process. In this particular Dockerfile, we have two stages:

**Build Stage (`myapp-build`):**
- `FROM maven:3.8.6-amazoncorretto-17 AS myapp-build`: We're using a Maven image based on Amazon Corretto 17 as the base image for this build stage. It's used to build our Java application.
- `ENV MYAPP_HOME /opt/myapp`: This instruction sets an environment variable `MYAPP_HOME` to `/opt/myapp` for the build stage, which is used as the working directory.
- `WORKDIR $MYAPP_HOME`: Here, we set the working directory inside the container to the value of `MYAPP_HOME`.
- `COPY pom.xml .`: We copy the `pom.xml` file (Maven's project configuration file) from our local system into the container's working directory.
- `COPY src ./src`: We copy the source code of our application from our local system into the `src` directory in the container.
- `RUN mvn package -DskipTests`: This command runs the process of building our application inside the container using Maven. The `-DskipTests` flag instructs Maven to skip running tests, which can speed up the build process.

**Run Stage:**
- `FROM amazoncorretto:17`: For the run stage, we use an Amazon Corretto 17 image, which is a distribution of OpenJDK for running our Java application.
- `ENV MYAPP_HOME /opt/myapp`: Once again, we set the `MYAPP_HOME` environment variable for this stage.
- `WORKDIR $MYAPP_HOME`: The working directory is configured to `MYAPP_HOME`.
- `COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar`: This instruction copies the resulting JAR file from the `myapp-build` stage into the run stage. Using `--from=myapp-build` specifies the copy source, ensuring that only necessary files are included in the final image.
- `ENTRYPOINT java -jar myapp.jar`: Finally, we set the container's entry point command to `java -jar myapp.jar`. This means that when the container is started, it will run our Java application using the specified JAR file.

### 1-3 Document docker-compose most important commands.

1. **docker-compose up**: This command creates and starts containers defined in the `docker-compose.yml` file. If the container images don't exist, Docker Compose will build them. Example:

    ```bash
    docker-compose up
    ```

2. **docker-compose down**: Use this command to stop and remove containers created by `docker-compose up`. It can also remove associated volumes if specified in the Compose file. Example:

    ```bash
    docker-compose down
    ```

3. **docker-compose build**: This command builds the services defined in the `docker-compose.yml` file. It's useful when you want to build the images separately from starting containers. Example:

    ```bash
    docker-compose build
    ```

4. **docker-compose ps**: Lists containers associated with services and displays their status (e.g., running, stopped). Example:

    ```bash
    docker-compose ps
    ```

5. **docker-compose logs**: Use this command to view the logs of your services. You can specify a specific service to view logs for that service only. For example:

    ```bash
    docker-compose logs backend
    ```

### 1-4 Document your docker-compose file.

# Docker Compose File Documentation

This Docker Compose file is used to orchestrate a multi-container application. Please refer to the comments within the file for detailed explanations.

## Services

1. **Backend Service**
   - **build**: Location of the Dockerfile for the backend service.
   - **networks**: Associated with the "app-network" for communication.
   - **depends_on**: Depends on the "database" service to be ready before starting.

2. **Database Service**
   - **build**: Location of the Dockerfile for the database service.
   - **networks**: Associated with the "app-network" for communication.
   - **volumes**: Mounts the "db-volume" into the container for storing PostgreSQL data.

3. **HTTPD Service**
   - **build**: Location of the Dockerfile for the HTTPD service.
   - **ports**: Maps port 80 of the HTTPD container to port 80 of the host for external access.
   - **networks**: Associated with the "app-network" for communication.
   - **depends_on**: Depends on the "backend" service to be ready before starting.

#### Volumes

- **db-volume**: Named volume for storing data related to the PostgreSQL database service.

#### Networks

- **app-network**: Defines a network named "app-network" for inter-service communication and networking.

### 1-5 Document your publication commands and published images in dockerhub.


1. **Docker Login**:

   Before publishing an image, you need to log in to your Docker Hub account.

   ```bash
   docker login
   ```
2. **Tag the Image**:

To publish an image to Docker Hub, it should be tagged in the format `username/repository:tag`. Replace `username` with your Docker Hub username, `repository` with the name of your Docker Hub repository, and `tag` with a version tag for your image.

   ```bash
   docker tag local-image username/repository:tag
   ```

3. **Publish the Image**:

Once the image is tagged correctly, you can push it to Docker Hub using the following command:

   ```bash
   docker push username/repository:tag
   ```
# Miscellaneous Information

### Why do we need a reverse proxy?

A reverse proxy serves several crucial purposes in web architecture. It ensures load balancing, enhances security by filtering attacks, handles SSL/TLS encryption, and improves performance through content caching. Additionally, it facilitates request routing, compression, session management, and scalability. In summary, a reverse proxy simplifies web application management while enhancing their efficiency and security.

### Why is docker-compose so important?

Docker Compose is instrumental in managing multi-container applications. It simplifies the orchestration, standardization, and portability of complex application stacks through a single `docker-compose.yml` file. This tool is a cornerstone for developers and DevOps teams, offering quick deployment and seamless integration with the Docker ecosystem, making it indispensable in the world of containerization.

### Difference between JRE and JDK:

The JRE (Java Runtime Environment) and the JDK (Java Development Kit) are two essential components of the Java ecosystem. The JRE is designed to run pre-compiled Java applications, providing a stable runtime environment. In contrast, the JDK is intended for developers, offering a comprehensive set of tools for creating, compiling, and debugging Java applications. The primary distinction lies in their purpose: JRE is for execution, while JDK is for development.
