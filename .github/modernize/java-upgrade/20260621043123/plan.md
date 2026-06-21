# Upgrade Plan: simple-java-app (20260621043123)

- **Generated**: 2026-06-21 04:35:00
- **HEAD Branch**: main
- **HEAD Commit ID**: N/A

## Available Tools

**JDKs**
- JDK 8: detected on system (base JDK, used for baseline if available)
- JDK 21: **<TO_BE_INSTALLED>** (required for upgrade steps)

**Build Tools**
- Maven: **<TO_BE_UPGRADED>** (required to run build verification)

## Guidelines

> Note: You can add any specific guidelines or constraints for the upgrade process here if needed, bullet points are preferred.

## Options

- Working branch: appmod/java-upgrade-20260621043123
- Run tests before and after the upgrade: true

## Upgrade Goals

- Upgrade the project to Java 21 (latest LTS available in this environment)

## Technology Stack

| Technology/Dependency | Current | Min Compatible | Why Incompatible |
| ---------------------- | ------- | -------------- | ---------------- |
| Java | 8 | 21 | User requested |
| Maven Compiler Plugin | 3.7.0 | 3.11.0 | Recommended for Java 21 builds |
| Maven Surefire Plugin | (not explicitly set) | 3.0+ | Recommended for Java 17+/21 |
| Docker base image | maven:3.6.3-jdk-11-slim | Java 21 runtime/build image | Old JDK runtime/build image |
| GitHub Actions workflow | uses default runner without Java setup | Java 21 setup action | Build should explicitly target Java 21 |

## Derived Upgrades

- Java 21 upgrade requires Maven/compiler settings aligned to Java 21.
- Maven build plugins should be updated to versions compatible with Java 21.
- Container and CI images must run on Java 21-compatible tooling.

## Impact Analysis

### Dependency Changes

| File | Dependency | Current | Action | Target | Reason |
|------|-----------|---------|--------|--------|--------|
| pom.xml | maven-compiler-plugin | 3.7.0 | upgrade | 3.11.0+ | Java 21 build compatibility |
| pom.xml | compiler source/target | 1.8 | upgrade | 21 | User requested |
| pom.xml | maven-surefire-plugin | unset | add/upgrade | 3.2.0+ | Ensure tests run correctly on Java 21 |
| Dockerfile | build/runtime base image | maven:3.6.3-jdk-11-slim | replace | maven:3.9.9-eclipse-temurin-21 and eclipse-temurin:21-jre | Align container build/runtime with Java 21 |
| .github/workflows/actions.yml | Java setup | none | add | setup-java 21 | Ensure CI uses correct JDK |

### Source Code Changes

| File | Location | Current | Required Change | Reason |
|------|----------|---------|----------------|--------|
| src/main/java/com/mycompany/app/App.java | class implementation | no Java-version-specific logic | no functional rewrite expected | Existing code should compile under Java 21 |
| src/test/java/com/mycompany/app/AppTest.java | test imports | JUnit 4 assertions | keep as-is unless compilation issues appear | Existing tests should remain valid |

### Configuration Changes

| File | Property/Setting | Current | Required Change | Reason |
|------|------------------|---------|----------------|--------|
| pom.xml | compiler configuration | source/target 1.8 | set release 21 or equivalent | Java 21 compatibility |
| .github/workflows/actions.yml | build step | mvn clean package | use setup-java and verify step | CI should run with correct JDK |
| Dockerfile | build command/base image | old Maven/JDK image | update to Java 21 image | container runtime/build compatibility |
| README.md | build instructions | generic | mention Java 21 requirement | documentation drift |

### CI/CD Changes

| File | Location | Current | Required Change |
|------|----------|---------|----------------|
| .github/workflows/actions.yml | workflow setup | no Java setup | add `setup-java` step with Java 21 |
| Dockerfile | build runtime | Java 11 image | switch to Java 21 compatible image |
| Jenkinsfile | pipeline definition | malformed/legacy | fix stage syntax and run with Java 21-compatible build |

### Risks & Warnings

- **Build tool availability**: Maven is not currently available in the environment. **Mitigation**: install a compatible Maven distribution before verification.
- **Java runtime mismatch in CI/container**: old images may still reference Java 8/11. **Mitigation**: update both Docker and workflow configs together.
- **Jenkinsfile syntax**: current file appears malformed. **Mitigation**: replace with a valid declarative pipeline using a Java 21-compatible build step.

## Upgrade Steps

- Step 1: Setup Environment
  - **Rationale**: Required JDK/build-tool versions must be installed before any verification.
  - **Changes to Make**: Install JDK 21 and a compatible Maven distribution; confirm tool availability.
  - **Verification**: Run `java -version` and `mvn -version` with the installed tools.

- Step 2: Setup Baseline
  - **Rationale**: Capture the project's current behavior before changing versions.
  - **Changes to Make**: Run baseline compile/test with the current project configuration.
  - **Verification**: Run `mvn clean test-compile` and `mvn clean test` (if possible with the available JDK).

- Step 3: Upgrade Maven Build Settings for Java 21
  - **Rationale**: Update compiler and test plugin settings so the project builds correctly on Java 21.
  - **Changes to Make**: Apply all Dependency Changes and Configuration Changes for the build file.
  - **Verification**: Run `mvn clean test-compile`.

- Step 4: Update Container and CI Configurations
  - **Rationale**: Ensure Docker and CI pipelines use the same Java 21 runtime/build environment.
  - **Changes to Make**: Apply all CI/CD and container image changes.
  - **Verification**: Run the relevant build command(s) or validate the updated config files.

- Step 5: Final Validation
  - **Rationale**: Confirm the upgrade is complete and all tests pass.
  - **Changes to Make**: Fix any remaining issues uncovered during verification.
  - **Verification**: Run `mvn clean test` with the Java 21 toolchain.
