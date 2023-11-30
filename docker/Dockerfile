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

# Debugging: Print the contents of javaruntime
RUN ls -R /javaruntime

# Stage 2: Runtime stage
FROM ubuntu:latest

ENV LANG en_US.UTF-8
ENV JAVA_HOME /usr/lib/jvm/msopenjdk-21-amd64
ENV PATH "${JAVA_HOME}/bin:${PATH}"

COPY --from=build-stage /javaruntime $JAVA_HOME

WORKDIR /opt/app

COPY renaissance-gpl-0.15.0.jar .

# Debugging: Print the classpath
RUN java -XshowSettings:properties -jar renaissance-gpl-0.15.0.jar functional

CMD ["java", "-jar", "renaissance-gpl-0.15.0.jar", "functional"]
