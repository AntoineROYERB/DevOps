# [TP2 - Github Actions](http://school.pages.takima.io/devops-resources/ch2-discover-github-actions-tp/)

Maven is a build automation and project management tool used primarily for Java projects. It simplifies the build process by managing project dependencies, compiling source code, running tests, and packaging applications. Maven uses a standardized project structure and relies on a Project Object Model (POM) file to define project configurations. It streamlines the development workflow and automates tasks such as building, testing, and packaging, making it easier for developers to manage and build complex Java applications.
```
$ sudo apt install maven
$ cd TP2/simple-api
$ mvn clean verify
```

`mvn clean verify` is a command used in Apache Maven to clean and verify (build and test) a Java project. It removes files generated in previous builds (the "clean" phase) and performs a full project build, including compilation, running tests, and generating artifacts (the "verify" phase). This is commonly used to ensure a clean and reliable build of a Java project.

# First steps into the CD World

Secrets in GitHub Actions are encrypted environment variables that you can use to store sensitive information and credentials, such as API keys, access tokens, or other confidential data that your workflows might need. These secrets provide a secure way to access and use this sensitive information without exposing it in your workflow files or making it public.

During our recent workshop, we've configured three key secrets to enhance security and functionality in our GitHub Actions workflows:

- DOCKERHUB_USERNAME: Safely stores our Docker Hub username for secure authentication.

- DOCKERHUB_PASSWORD: Securely manages our Docker Hub password, ensuring confidential access for image builds and pushes.

- SONAR_TOKEN: Enables secure access to SonarCloud for code quality analysis.

# Setup Quality Gate

SonarCloud is a cloud-based platform that assesses and improves code quality and security in software development. It offers automated code analysis, identifies issues, and provides valuable insights to help developers and teams maintain high-quality, secure code. With seamless integration into CI/CD pipelines, it ensures that code is continuously checked, promoting best coding practices and reducing vulnerabilities.

` 
run: mvn -B verify sonar:sonar -Dsonar.projectKey=tp-devops-royer_tp2 -Dsonar.organization=tp-devops-royer -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=${{ secrets.SONAR_TOKEN }}  
`

The provided command runs Maven to validate and package the project. It then triggers SonarScanner to analyze the code and send the results to SonarCloud. The `-D` options specify project and organization keys, SonarCloud URL, and authentication token from GitHub secrets.

# Answers to questions

### 2-1 What are testcontainers?

They simply are java libraries that allow you to run a bunch of docker containers while testing. Here we use the postgresql container to attach to our application while testing.

### 2-2 Document your Github Actions configurations.

See `.github/workflows/main.yml `comments.

### 2-3 Document your quality gate configuration.

See `.github/workflows/main.yml` comments.

# Miscellaneous Information

### Why did we put needs: build-and-test-backend on this job?

The provided GitHub Actions workflow defines a job named "build-and-push-docker-image" that is responsible for building and pushing Docker images for a backend application, a database, and an HTTP server.

### For what purpose do we need to push docker images?

Publishing Docker images is essential because it allows for:

- Easy distribution of your application.
- Consistent representation of the environment.
- Isolation and portability.
- Version management and deployability.
- Integration into CI/CD pipelines.
- Enhanced security.
- Improved scalability.


### To commit without making any changes and rerun the tests:

```
$ git commit --amend --no-edit
$ git push -f
```
