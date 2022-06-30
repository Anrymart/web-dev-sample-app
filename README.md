# web-dev-sample-app

## Backend

Guide: https://spring.io/guides/topicals/spring-boot-docker/

### Dockerfile

Simple Dockerfile, assumes that the .jar file has been already built:

```dockerfile
FROM openjdk:12-alpine
VOLUME /tmp
COPY target/*.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

Docker multi-stage build:

```dockerfile
FROM openjdk:12-alpine as build
WORKDIR /workspace/app

COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

RUN ./mvnw install -DskipTests

FROM openjdk:12-alpine
VOLUME /tmp
COPY --from=build /workspace/app/target/*.jar app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

### Building and running

Build Docker image:

```shell
docker build -t anrymart/backend .
```

Run Docker image:

```shell
docker run -p 8080:8080 anrymart/backend
```

Port exposure syntax:

```shell
docker run -p {HOST_PORT}:{CONTAINER_PORT} anrymart/backend
```

