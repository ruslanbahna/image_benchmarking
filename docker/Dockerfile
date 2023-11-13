# Use a base image with a shell to create the custom JRE
FROM alpine:latest AS jre-builder

# Install the necessary tools
RUN apk add openjdk17-jdk

# Create a custom JRE using jlink
RUN jlink \
    --add-modules java.base \
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=2 \
    --output /javaruntime

# Final image using distroless base image
FROM mcr.microsoft.com/openjdk/jdk:21-distroless

# Copy the custom JRE from the jre-builder stage
COPY --from=jre-builder /javaruntime /javaruntime

# Set the environment variable to point to the custom JRE
ENV JAVA_HOME=/javaruntime
ENV PATH="$PATH:$JAVA_HOME/bin"

# Create a directory for benchmark application
WORKDIR /app

# Copy the benchmark JAR from local directory to the image
COPY renaissance-gpl-0.15.0.jar /app/renaissance-gpl-0.15.0.jar

# Run the benchmark application
ENTRYPOINT ["java", "-jar", "renaissance-gpl-0.15.0.jar", "functional"]

