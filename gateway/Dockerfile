# Stage 1: Build the application
FROM maven:3.8.4-openjdk-17-slim AS builder
WORKDIR /app

# Copy only pom.xml first to cache dependencies
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source code
COPY src/ ./src/

# Build the application
RUN mvn clean package -DskipTests

# Stage 2: Runtime environment
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Create a non-root user
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring

# Copy the JAR file from the builder stage
COPY --from=builder --chown=spring:spring /app/target/*.jar app.jar

# Set environment variables
ENV JAVA_OPTS="-Xms512m -Xmx512m"

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -q --spider http://localhost:8888/actuator/health || exit 1

# Run the application
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]