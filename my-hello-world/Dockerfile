FROM bellsoft/liberica-openjdk-alpine

ARG JAR_FILE
ADD target/$JAR_FILE /opt/maven-docker/my-hello-world.jar

ENTRYPOINT ["java", "-jar", "/opt/maven-docker/my-hello-world.jar"]