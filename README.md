# Native Quarkus telemetry on Azure

This project contains 2 services implemented with Quarkus using que Quarkiverse OpenTelemetry Exporter for Azure, for demonstration purposes. `vegetable`and `superhero`.

The `vegetable` is a CRUD application for Vegetables and each time you create a veggie in there, a REST request is dispatched to `superhero` where a veggie is transformed in a vegetable superhero :) .

This allows us to show tracing across multiple services.

## What is Quarkus?
- Quarkus is a Kubernetes-native Java framework designed to for running Java in cloud environments. It delivers:
- Container-First: Optimized for small container images and fast startup times.
- Developer Joy: Live coding, unified configuration, and smooth integration with popular libraries.
- Kubernetes Native: Built-in integrations and tools for effortless Kubernetes deployment.
- Reactive & Imperative: Flexibility to choose the programming model that suits your needs.
- GraalVM & HotSpot: Native image compilation for even faster startup and lower memory usage (optional).

### Why Quarkus?
- Modernize legacy Java apps: Make them cloud-ready with faster startup and reduced memory footprint.
- Build new microservices: Leverage Quarkus' container-first design and Kubernetes integration.
- Embrace reactive: Combine reactive and imperative programming for maximum flexibility.

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```shell script
mvn compile quarkus:dev
```
In these projects it will automatically download and start a PostgreSQL DB.

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at http://localhost:8080/q/dev/.

## Packaging and running the application

The application can be packaged using:
```shell script
mvn package
```
It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:
```shell script
mvn package -Dquarkus.package.type=uber-jar
```

The application, packaged as an _über-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using: 
```shell script
mvn package -Dnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: 
```shell script
mvn package -Dnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/<app name>-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.


## Enable the telemetry for GraalVM native

To configure the telemetry for GraalVM native, look at [this page](https://docs.quarkiverse.io/quarkus-opentelemetry-exporter/dev/quarkus-opentelemetry-exporter-azure.html).

You can follow [these instructions](./../../Azure-connection-string.md) to create an Application Insights resource and get the connection string in the Azure portal.

## Docker 
You have docker files to deploy each of the Quarkus projects at `/src/main/docker/`:
- For [superhero](superhero/src/main/docker/)
- For [vegetable](vegetable/src/main/docker/)
  
A `Dockerfile.multistage` file is available build a docker image from a multistage dockerfile. 

These commands can be executed to build and run the `superhero` service:
Build on project root: `docker build -f src/main/docker/Dockerfile.multistage --ulimit nofile=5000:5000 -t superhero .`
Run: `docker run -i --rm -p 8081:8080 superhero`

## Docker compose

Each project has a docker compose allowing to start a PostgreSQL DB allong with a container using the previouslu built docker image:
- For [superhero](superhero/docker-compose.yml)
- For [vegetable](vegetable/docker-compose.yml)

## Related Guides

- OpenTelemetry ([guide](https://quarkus.io/guides/opentelemetry)): Use OpenTelemetry to trace services
- Quarkus Opentelemetry Exporter for Microsoft Azure ([guide](https://docs.quarkiverse.io/quarkus-opentelemetry-exporter/dev/quarkus-opentelemetry-exporter-azure.html)).

