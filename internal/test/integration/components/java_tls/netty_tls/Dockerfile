FROM maven:3.10.0-eclipse-temurin-21@sha256:4c892edc136f819d71397deee8cc0073de01bb3f8e0421fe79e603f021e59364 AS javabuilder

# Install tar and gzip to extract the Maven binaries
RUN apt install -y tar gzip

# Install Maven
ARG USER_HOME_DIR="/cache"

# Set the working directory to /build
WORKDIR /build

# Copy the source code into the image for building
COPY internal/test/integration/components/java_tls/netty_tls/src src/
COPY internal/test/integration/components/java_tls/netty_tls/pom.xml pom.xml

RUN java -version
RUN mvn -version

# Build
RUN mvn clean package

# The App Image
FROM eclipse-temurin:21.0.10_7-jre@sha256:34a58218d838035428163eb35abb629944c5906d1bedcfef8bc8864cce11dfe5

EXPOSE 8080

# Copy the native executable into the containers
COPY --from=javabuilder /build/target/http-client-demo-1.0.0.jar .
CMD [ "java", "-jar", "/http-client-demo-1.0.0.jar" ]