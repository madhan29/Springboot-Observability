# Stage 1: Download OpenTelemetry Java Agent JAR
FROM alpine AS download
ARG OTEL_AGENT_VERSION="1.33.2"
ADD "https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v1.33.2/opentelemetry-javaagent.jar" /opentelemetry-javaagent.jar

# Stage 2: Build the Java application using Maven
FROM maven:3.8.6-openjdk-8 AS build
WORKDIR /build
COPY . .
RUN mvn package --quiet

# Stage 3: Create the final image
FROM openjdk:8-jre-slim
WORKDIR /app
COPY --from=build /build/target/*.jar /app/app.jar
COPY --from=download /opentelemetry-javaagent.jar /app/opentelemetry-javaagent.jar
ENTRYPOINT ["java", \
  "-javaagent:/app/opentelemetry-javaagent.jar", \
  "-jar", "/app/app.jar" \
  ]
