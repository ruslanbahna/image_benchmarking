# Stage 1: Build stage
FROM mcr.microsoft.com/openjdk/jdk:21-ubuntu AS build-stage

WORKDIR /app
COPY renaissance-gpl-0.15.0.jar .

RUN jlink --add-modules java.base,java.logging \  # Add java.logging module\
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

# Continue with your application deployment
RUN mkdir /opt/app
WORKDIR /opt/app

# Adjust the source path for the HelloWorld.class file
COPY --from=build-stage /app/renaissance-gpl-0.15.0.jar .

CMD ["java", "-jar", "renaissance-gpl-0.15.0.jar", "functional"]

