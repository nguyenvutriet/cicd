# ---------- Build stage ----------
FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /app

# Cache dependency (chỉ chạy lại khi pom.xml đổi)
COPY pom.xml .
RUN mvn -B -q -Dmaven.wagon.http.retryHandler.count=5 dependency:go-offline

COPY src ./src
RUN mvn -B -q -DskipTests package

# ---------- Run stage ----------
FROM eclipse-temurin:21-jre
WORKDIR /app

# Chạy bằng user thường, không dùng root
RUN useradd -r -u 1001 appuser
COPY --from=build /app/target/*.jar app.jar
USER appuser

EXPOSE 8080
ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75", "-jar", "app.jar"]