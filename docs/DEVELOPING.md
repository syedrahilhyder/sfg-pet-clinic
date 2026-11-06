# Developing SFG Pet Clinic

SFG Pet Clinic is a Spring Boot 2.1 web application for managing owners, pets, vets and visits. The root `pom.xml` is a Maven aggregator over two modules. `pet-clinic-data` holds the domain model and the services, with both in-memory map and Spring Data JPA implementations, plus H2 and MySQL drivers. `pet-clinic-web` holds the Thymeleaf controllers and views and is the runnable application. The project targets Java 11.

## Building

Use JDK 11, which is what CI uses. The bundled Maven wrapper means you don't need a local Maven install. From the repository root:

```bash
./mvnw clean install -DskipTests     # Windows: mvnw.cmd clean install -DskipTests
```

This builds `pet-clinic-data` first and then `pet-clinic-web`. During the build, the wro4j plugin compiles the LESS sources in `pet-clinic-web/src/main/less` into CSS. To start the application after building:

```bash
java -jar pet-clinic-web/target/pet-clinic-web-0.0.5-SNAPSHOT.jar
```

## Running tests

```bash
./mvnw test                          # unit tests in both modules
./mvnw verify                        # also runs the Failsafe integration-test phase, as CI does
./mvnw -pl pet-clinic-web -am test   # one module plus its dependency
```

The tests use JUnit 5 and Mockito. `pet-clinic-data` covers the map and JPA services. `pet-clinic-web` covers the Owner, Pet and Visit controllers and includes a Spring context-load test. Surefire and Failsafe run with `--illegal-access=permit`. On JDK 17 or later the JVM ignores that flag with a warning, so use JDK 11 to match CI.
