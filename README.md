<div align='center' id='top'>

# StellaBank

  ![Java](https://a11ybadges.com/badge?logo=java)
  ![Spring](https://a11ybadges.com/badge?logo=spring)
  ![MySQL](https://a11ybadges.com/badge?logo=mysql)
</div>

StellaBank is an educational Nubank-like project created to practice hexagonal architecture, JWT authentication, transactional transfers, and realistic banking patterns using Java and Spring Boot.

## Table of Contents
- [Overwiew](#overview)
- [Repository Structure](#repositoryStructure)
- [OpenAPI](#openAPI)
- [CI/CD (GitHub Actions)](#CI/CD)
- [Artifacts](#artifacts)
- [Running Locally](#runningLocally)
- [License](#license)

<div id='overview'/>
    
## Overview
- Purpose: provide a study/reference base for architecture patterns (hexagonal), security (JWT), and integration (OpenAPI).
- Main stack: Java 26, Spring Boot, Maven, MySQL (e.g., Railway) and Apache Camel.

<div id='repositoryStructure'/>
    
## Repository structure
- `pom.xml` - Maven build descriptor.
- `src/main/java/` - application source (domain, adapters, ports, infrastructure).
- `src/main/resources/` - configuration, `application.yml`, and `openapi/stellabank-api.yaml`.
- `src/test/java/` and `src/test/resources/` - tests and test configs.
- `target/` - generated artifacts (e.g., `target/stellabank-0.0.1-SNAPSHOT.jar`).
- `.github/workflows/` - GitHub Actions workflows (CI, security scanner).
- `LICENSE`, `HELP.md` - license and quick-help notes.

Notable files:
- `src/main/resources/openapi/stellabank-api.yaml` (OpenAPI spec)
- `.github/workflows/maven.yml` (Maven CI pipeline)
- `.github/workflows/trivy.yml` (Trivy security scan)

<div id='openAPI'/>
    
## OpenAPI
The OpenAPI specification is located at `src/main/resources/openapi/stellabank-api.yaml` (generated). You can open it in the Swagger Editor (https://editor.swagger.io/) or serve it with a Swagger UI to explore endpoints and example requests/responses.

Example endpoints (see the YAML for full details):
- GET /accounts
- POST /auth (authenticate / obtain JWT)
- POST /transfers

<div id='CI/CD'/>
    
## CI/CD (GitHub Actions)
This repository includes workflows under `.github/workflows`:

1) `maven.yml` — Java CI with Maven
- Triggers: `push` and `pull_request` on the `main` branch.
- Main steps:
    - Checkout the repository (`actions/checkout@v4`).
    - Setup JDK 26 using `actions/setup-java@v4` (Temurin) and enable Maven caching.
    - Run `mvn -B clean verify --file pom.xml` to build and run verification/tests.
    - Use `advanced-security/maven-dependency-submission-action` to update the dependency graph.

2) `trivy.yml` — Trivy vulnerability scan
- Triggers: `push` and `pull_request` on the `main` branch.
- Main steps:
    - Checkout the repository.
    - Run `aquasecurity/trivy-action` in filesystem scan mode (`scan-type: fs`) targeting the project directory.
    - The first step generates a table report for severities `CRITICAL` and `HIGH`, with `ignore-unfixed: true`.
    - The second step runs Trivy again with `exit-code: 1` to fail the workflow if `CRITICAL` vulnerabilities are found.

Note: There is no `Dockerfile` in the repository currently. The Trivy workflow scans the filesystem rather than scanning a container image.

<div id='artifacts'/>
    
## Artifacts
- The build produces `target/stellabank-0.0.1-SNAPSHOT.jar` and `generated-sources` (OpenAPI). Coverage and Surefire reports are available under `target/`.

<div id='runningLocally'/>
    
## Running locally
Prerequisites: JDK 26, Maven 3.x, (optional) local MySQL or a database URL in environment variables.

1) Build the project:

```bash
mvn clean package -DskipTests
```

2) Run the application with Spring Boot:

```bash
mvn spring-boot:run
# or
java -jar target/stellabank-0.0.1-SNAPSHOT.jar
```

3) Run tests:

```bash
mvn test
```

Configuration: profiles and settings are available in `src/main/resources/application.yml` and test profiles in `src/test/resources`.

<div id='license'/>
    
## License
This project is licensed under the terms in `LICENSE`.

<div align='right'>
  
  [Back to top of page ⬆️](#top)

</div>
