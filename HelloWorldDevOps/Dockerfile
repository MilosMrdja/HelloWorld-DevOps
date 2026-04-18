# --- Stage 1: Build ---
# Using an official Maven image with Java 21
FROM maven:3.9-eclipse-temurin-21 AS build

WORKDIR /app

# 1. Copy pom.xml first to cache dependencies
# This is a DevOps Pro-Tip: Docker will cache this layer so you don't
# re-download all libraries every time you change your code.
COPY pom.xml .
RUN mvn dependency:go-offline

# 2. Copy the source code and build the JAR
COPY src ./src
RUN mvn clean package -DskipTests

# --- Stage 2: Runtime ---
# Using a slim JRE image (no compiler, no Maven)
FROM eclipse-temurin:21-jre-jammy

WORKDIR /app

# Copy only the compiled artifact from the build stage
COPY --from=build /app/target/HelloWorldDevOps-0.0.1-SNAPSHOT.jar app.jar

# Inform Docker that this container listens on port 8080
EXPOSE 8080

# The command to start the application
ENTRYPOINT ["java", "-jar", "app.jar"]