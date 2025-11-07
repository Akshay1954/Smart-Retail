# ===== Stage 1: Build the JAR =====
FROM maven:3.9.6-eclipse-temurin-21 AS builder

# Set working directory
WORKDIR /app

# Copy pom.xml and download dependencies first (cached)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy the full project and build
COPY src ./src
RUN mvn clean package -DskipTests

# ===== Stage 2: Run the JAR =====
FROM eclipse-temurin:21-jdk-alpine

WORKDIR /app

# Copy the JAR from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose port 8080 (Spring Boot default)
EXPOSE 8080

# Use environment variables from runtime
ENV SPRING_PROFILES_ACTIVE=prod

# Run the JAR
ENTRYPOINT ["java", "-jar", "app.jar"]
