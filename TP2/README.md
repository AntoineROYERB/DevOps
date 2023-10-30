# TP2

sudo apt install maven

```
$ cd simple-api
$ sudo apt install maven
```

What is it supposed to do?

`mvn clean verify` is a command used in Apache Maven to clean and verify (build and test) a Java project. It removes files generated in previous builds (the "clean" phase) and performs a full project build, including compilation, running tests, and generating artifacts (the "verify" phase). This is commonly used to ensure a clean and reliable build of a Java project.

à la racine du projet:




# Questions

2-1 What are testcontainers?

They simply are java libraries that allow you to run a bunch of docker containers while testing. Here we use the postgresql container to attach to our application while testing.

2-2 Document your Github Actions configurations.

See .github/workflows/main.yml comments