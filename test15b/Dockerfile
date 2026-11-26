# ---- Etapa 1: compilar el jar dentro del build ----
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /build

# Cachear dependencias antes de copiar el codigo fuente
COPY pom.xml .
RUN mvn -B -q dependency:go-offline || true

COPY src ./src
RUN mvn -B -q clean package -DskipTests

# ---- Etapa 2: runtime liviano (solo JRE) ----
FROM eclipse-temurin:21-jre
WORKDIR /app
COPY --from=build /build/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
