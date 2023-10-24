# Docker DevOps Project

This repository contains a Docker project as part of a DevOps course. The project comprises a web application, a PostgreSQL database, and a backend API, all running within Docker containers.

## Overview of Docker Commands

Here is a list of commonly used Docker commands:

### docker

The basic command for interacting with Docker. It allows you to manage containers, images, and various aspects of the Docker system.

### docker build

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

# Réponses aux questions
1-0 Why should we run the container with a flag -e to give the environment variables?
Using the -e flag when running allows you to configure your container's behavior using environment variables, providing a flexible and secure way to manage your application's settings.

1-1 Why do we need a volume to be attached to our postgres container?

Attaching a volume to a PostgreSQL container is crucial for preserving the database's data integrity and ensuring data persistence. Without a volume, data would be lost when the container is recreated or updated. Volumes also enable easy backups, recovery, and data sharing.

1-2 Why do we need a multistage build?

We need a multistage build because we need to build the app and then run it. We can't do it in one step because we need to build the app in a container and then run it in another container.

1-3 Why do we need a reverse proxy?

Il assure une répartition équilibrée de la charge, renforce la sécurité en filtrant les attaques, gère le chiffrement SSL/TLS, et améliore les performances en mettant en cache le contenu. De plus, il facilite le routage des demandes, la compression, la gestion des sessions, et l'évolutivité. En somme, le reverse proxy simplifie la gestion des applications web tout en renforçant leur efficacité et leur sécurité.

1-4 Why is docker-compose so important?

Docker Compose is instrumental in managing multi-container applications. It simplifies the orchestration, standardization, and portability of complex application stacks through a single `docker-compose.yml` file. This tool is a cornerstone for developers and DevOps teams, offering quick deployment and seamless integration with the Docker ecosystem, making it indispensable in the world of containerization.

# Miscellaneous Information

Difference between JRE and JDK:

The JRE (Java Runtime Environment) and the JDK (Java Development Kit) are two essential components of the Java ecosystem. The JRE is designed to run pre-compiled Java applications, providing a stable runtime environment. In contrast, the JDK is intended for developers, offering a comprehensive set of tools for creating, compiling, and debugging Java applications. The primary distinction lies in their purpose: JRE is for execution, while JDK is for development.
