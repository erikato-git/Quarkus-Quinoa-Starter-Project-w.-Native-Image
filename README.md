# Code with Quinoa in Quarkus

This project provides a way to create a frontend application that runs on Quarkus by Quinoa and boots up in 0.021 seconds, when running it in a docker container from a **native image**. (As well this project provides a way to create a full-stack application, utilizing Quarkus for the backend and a framework of own choice for the frontend: React, Vue, Angular, Next.js, etc.)

![alt text](image.png)


## Notes to get started with Quinoa


Quinoa is an extension in Quarkus to implement a frontend project from various frameworks in a Quarkus project. To run the frontend (located in the folder 'webui') from the server in development mode, navigate to the root and use this command:
```shell script
quarkus dev
```

## Choose a framework to use with Quinoa

To initialize a frontend-framework go to 'src/main' and create the desired framework by terminal from Quinoa's own list and name the folder containing the frontend project 'webui'. Quinoa will detect what framework that has been initialized
- https://docs.quarkiverse.io/quarkus-quinoa/dev/web-frameworks.html


Getting started with Quarkus Quinoa:
- https://docs.quarkiverse.io/quarkus-quinoa/dev/


## How to generate a native image (frontend: Vue.js)

A. Make a build version of the frontend in the folder called 'dist'. Navigate to 'webui' and run: 

```shell script
npm run build
```

B. Navigate to root and generate a "...SNAPSHOT-runner" executable file by running this command in terminal: 

```shell script
./mvnw package -Pnative "-Dquarkus.native.container-build=true"
```

C. Insert in Dockerfile.native:

```dockerfile
FROM registry.access.redhat.com/ubi8/ubi-minimal
WORKDIR /work/
COPY target/*-runner /work/application
RUN chmod 775 /work
CMD ./application -Dquarkus.http.host=0.0.0.0 -Dquarkus.http.port=${PORT}
```

D. Build Dockerfile.Native: 

```shell script
docker build -f src/main/docker/Dockerfile.native -t quarkus/quinoa-image .
```

(E. Run the native image): 

```shell script
docker run -i --rm --name hello_quarkus --env PORT=8081 -p 8081:8081 quarkus/quinoa-image   
```
The frontend applicationen enabled by Quinoa will be displayed at <http://localhost:8081>.


&nbsp;

## Normal Introduction to Quarkus

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: <https://quarkus.io/>.

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:

```shell script
./mvnw compile quarkus:dev
```

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at <http://localhost:8080/q/dev/>.

## Packaging and running the application

The application can be packaged using:

```shell script
./mvnw package
```

It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:

```shell script
./mvnw package -Dquarkus.package.jar.type=uber-jar
```

The application, packaged as an _über-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using:

```shell script
./mvnw package -Dnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using:

```shell script
./mvnw package -Dnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/code-with-quarkus-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult <https://quarkus.io/guides/maven-tooling>.

## Related Guides

- REST ([guide](https://quarkus.io/guides/rest)): A Jakarta REST implementation utilizing build time processing and
  Vert.x. This extension is not compatible with the quarkus-resteasy extension, or any of the extensions that depend on
  it.
- Quinoa ([guide](https://quarkiverse.github.io/quarkiverse-docs/quarkus-quinoa/dev/index.html)): Develop, build, and
  serve your npm-compatible web applications such as React, Angular, Vue, Lit, Svelte, Astro, SolidJS, and others
  alongside Quarkus.

## Provided Code

### Quinoa

Quinoa codestart added a tiny Vite app in src/main/webui. The page is configured to be visible on <a href="/quinoa">
/quinoa</a>.

[Related guide section...](https://quarkiverse.github.io/quarkiverse-docs/quarkus-quinoa/dev/index.html)

### REST

Easily start your REST Web Services

[Related guide section...](https://quarkus.io/guides/getting-started-reactive#reactive-jax-rs-resources)
