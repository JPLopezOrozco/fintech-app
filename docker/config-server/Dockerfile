FROM eclipse-temurin:21-jdk AS builder
WORKDIR /build

COPY mvnw .
COPY .mvn/ .mvn/

COPY pom.xml .
COPY config-server/pom.xml config-server/pom.xml

RUN chmod +x mvnw && ./mvnw -B -DskipTests -f config-server/pom.xml dependency:go-offline

COPY config-server/ config-server/

RUN ./mvnw -B -DskipTests -f config-server/pom.xml clean package


FROM eclipse-temurin:21-jre
WORKDIR /app

RUN apt-get update \
 && apt-get install -y --no-install-recommends openssh-client git \
 && rm -rf /var/lib/apt/lists/*

COPY --from=builder /build/config-server/target/*.jar app.jar

EXPOSE 8888
ENTRYPOINT ["java","-jar","app.jar"]