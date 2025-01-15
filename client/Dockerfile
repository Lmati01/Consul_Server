# Stage 1: Build the application
FROM maven:3.8.4-openjdk-17 AS builder
WORKDIR /app

# Copy source code and Maven descriptor
COPY ./src /app/src
COPY ./pom.xml /app

# Build the application
RUN mvn clean package -DskipTests

# List the files in the target directory for debugging
RUN ls -l /app/target

# Stage 2: Runtime environment
FROM openjdk:17-jdk-alpine
WORKDIR /app
VOLUME /tmp

# Copy the JAR file from the builder stage
COPY --from=builder /app/target/*.jar /app/service-client.jar

# Set the entry point
ENTRYPOINT ["java", "-jar", "/app/service-client.jar"]