#FROM openjdk:8-jdk-alpine
#VOLUME /tmp
#COPY target/client-service-1.0-SNAPSHOT.jar app.jar
#ENV JAVA_OPTS=""
#ENTRYPOINT exec java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=9999 -jar /app.jar


FROM openjdk:8-alpine
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} /app.jar
ENTRYPOINT [ "java", "-jar", "/app.jar" ]