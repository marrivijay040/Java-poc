# ===== Stage 1: Build the application =====
FROM maven:3.9.6-eclipse-temurin-17 AS builder

# Set working directory inside the container
WORKDIR /app

# Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn -B dependency:go-offline -ntp

# Copy the entire source code
COPY src ./src

# Build the Spring Boot application (skip tests to speed up)
RUN mvn -B clean package -DskipTests

# ===== Stage 2: Run the application =====
FROM eclipse-temurin:17-jre

# Set working directory for the runtime
WORKDIR /app

# Copy only the built jar from the builder stage
COPY --from=builder /app/target/*.jar /app/app.jar

# Expose the port your Spring Boot app runs on
EXPOSE 8080

# Default command to run the app
ENTRYPOINT ["java", "-jar", "/app/app.jar"]