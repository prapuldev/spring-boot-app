# Use a lightweight JRE image
FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

# Copy the built jar from the target folder into the image
COPY target/*.jar app.jar

# Expose the port your Spring Boot app runs on
EXPOSE 8085

# Command to run the app
ENTRYPOINT ["java", "-jar", "app.jar"]
