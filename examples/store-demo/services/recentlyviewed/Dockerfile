# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0
#
# Base images are pinned by digest per devdocs/dependency-integrity-policy.md.
# They match the ones used by internal/test/integration/components/javaaerospike.
FROM maven:3.10-eclipse-temurin-21@sha256:4c892edc136f819d71397deee8cc0073de01bb3f8e0421fe79e603f021e59364 AS builder

WORKDIR /build

COPY pom.xml pom.xml
COPY src src/

RUN mvn -q -e clean package

FROM eclipse-temurin:21-jre@sha256:d7051a45dd955e4d5d1db4d3f4269fe13d1c6dff8cc6b7ef89fc8577b96c1982

EXPOSE 8080

COPY --from=builder /build/target/recentlyviewed-1.0.0.jar /recentlyviewed.jar

USER 1000:1000
CMD ["java", "-jar", "/recentlyviewed.jar"]
