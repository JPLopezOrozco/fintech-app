FROM eclipse-temurin:21-jdk AS builder

WORKDIR /build

COPY mvnw .
COPY .mvn/ .mvn/

COPY pom.xml .
COPY eureka-server/pom.xml eureka-server/pom.xml

RUN chmod +x mvnw && ./mvnw -B -DskipTests -f eureka-server/pom.xml dependency:go-offline

COPY eureka-server/ eureka-server/

RUN ./mvnw -B -DskipTests -f eureka-server/pom.xml clean package

FROM eclipse-temurin:21-jre

WORKDIR /app

COPY --from=builder /build/eureka-server/target/*.jar app.jar

EXPOSE 8761
ENTRYPOINT ["java","-jar","app.jar"]