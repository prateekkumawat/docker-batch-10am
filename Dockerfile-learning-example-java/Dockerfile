# ===Build the application using Maven ===
FROM maven:3.9.4-eclipse-temurin-17 AS builder

WORKDIR /app

# Copy all files needed for Maven build
COPY pom.xml .
COPY src ./src

# Package the application
RUN mvn clean package -DskipTests

# === Stage 2: Run the application ===
FROM openjdk:17.0.2-jdk-slim

# Copy built jar from builder stage
COPY --from=builder /app/target/demo-0.0.1-SNAPSHOT.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "/app.jar"]

