# Stage 1: Build stage
FROM mcr.microsoft.com/openjdk/jdk:21-ubuntu AS build-stage

WORKDIR /app
COPY renaissance-gpl-0.15.0.jar .

RUN jlink \
    --add-modules java.base,java.logging \
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=2 \
    --output /javaruntime


# Stage 2: Runtime stage
FROM ubuntu:latest

ENV LANG en_US.UTF-8
ENV JAVA_HOME /usr/lib/jvm/msopenjdk-21-amd64
ENV PATH "${JAVA_HOME}/bin:${PATH}"

COPY --from=build-stage /javaruntime $JAVA_HOME

WORKDIR /opt/app

COPY renaissance-gpl-0.15.0.jar .


CMD ["/java", "-jar", "renaissance-gpl-0.15.0.jar", "mnenonics"]

