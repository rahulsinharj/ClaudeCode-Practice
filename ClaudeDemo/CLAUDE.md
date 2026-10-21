# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Spring Boot 4.1 (Java 21) application scaffolded from Spring Initializr. It is currently a skeleton: just the `ClaudeDemoApplication` entry point and a context-load test, with no entities, repositories, services, or controllers yet. Dependencies: Spring Web MVC, Spring Data JPA, MySQL connector, and Lombok.

## Commands

Use the Maven wrapper (`mvnw.cmd` on Windows/PowerShell, `./mvnw` in Bash):

```bash
./mvnw clean package                 # build the jar (runs tests)
./mvnw clean package -DskipTests     # build without tests
./mvnw spring-boot:run               # run the app (port 8080 by default)
./mvnw test                          # run all tests
./mvnw test -Dtest=ClaudeDemoApplicationTests             # single test class
./mvnw test -Dtest=ClaudeDemoApplicationTests#contextLoads # single test method
```

## Architecture and conventions

- **Root package is `com`** (not `com.something`). `@SpringBootApplication` on `com.ClaudeDemoApplication` component-scans everything under `com`, so new classes must live in `com` or a subpackage (e.g. `com.controller`, `com.entity`, `com.repository`, `com.service`).
- **Database**: MySQL at `jdbc:mysql://localhost:3306/demo_db`, configured in `src/main/resources/application.properties`. `spring.jpa.hibernate.ddl-auto=update` means Hibernate creates/alters tables from `@Entity` classes automatically; SQL is logged (`show-sql`, `format_sql`).
- **Tests need a live MySQL.** `ClaudeDemoApplicationTests` is a full `@SpringBootTest`, so it boots the real datasource; it fails if MySQL isn't running with `demo_db` available. For isolated tests use slice annotations (`@DataJpaTest`, `@WebMvcTest`) from the `spring-boot-starter-data-jpa-test` / `spring-boot-starter-webmvc-test` dependencies.
- **Lombok** is wired as an annotation processor explicitly in `maven-compiler-plugin` (for both main and test compile). If you add another annotation processor, add it to both `annotationProcessorPaths` lists, or Lombok processing breaks.
- The `pom.xml` contains intentionally empty `<licenses>`, `<developers>`, and `<scm>` elements to block inheritance from the Spring Boot parent POM (see `HELP.md`); leave them in place.
