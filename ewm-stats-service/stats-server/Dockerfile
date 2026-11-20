FROM maven:3.9-eclipse-temurin-21 AS build
LABEL authors="dvp"
WORKDIR /app
COPY pom.xml .
COPY ewm-stats-service/pom.xml ewm-stats-service/
COPY ewm-stats-service/stats-dto/pom.xml ewm-stats-service/stats-dto/
COPY ewm-stats-service/stats-client/pom.xml ewm-stats-service/stats-client/
COPY ewm-stats-service/stats-server/pom.xml ewm-stats-service/stats-server/
COPY ewm-main-service/pom.xml ewm-main-service/
RUN mvn dependency:go-offline -B
COPY ewm-stats-service/stats-dto ewm-stats-service/stats-dto
COPY ewm-stats-service/stats-client ewm-stats-service/stats-client
COPY ewm-stats-service/stats-server ewm-stats-service/stats-server
RUN mvn clean package -pl ewm-stats-service/stats-server -am -DskipTests

FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /app/ewm-stats-service/stats-server/target/stats-server-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 9090
ENTRYPOINT ["java", "-jar", "app.jar"]
