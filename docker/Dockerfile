# Use a base image with a shell to create the custom JRE
FROM ubuntu:20.04 AS jre-builder

# Install the necessary tools
RUN apt-get update && apt-get install -y openjdk-17-jdk-headless

# Create a custom JRE using jlink
RUN jlink \
    --add-modules java.base \
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=2 \
    --output /javaruntime

# Create the final image using the distroless base image
FROM mcr.microsoft.com/openjdk/jdk:21-distroless

# Copy the custom JRE from the jre-builder stage
COPY --from=jre-builder /javaruntime /javaruntime

# Set the environment variable to point to the custom JRE
ENV JAVA_HOME=/javaruntime
ENV PATH="$PATH:$JAVA_HOME/bin"

# Create a directory for your benchmark application
WORKDIR /app

# Download the benchmark JAR from the given URL
RUN curl -Lo renaissance-gpl-0.15.0.jar https://github.com/renaissance-benchmarks/renaissance/releases/download/v0.15.0/renaissance-gpl-0.15.0.jar

# Run the benchmark application
CMD ["java", "-jar", "renaissance-gpl-0.15.0.jar"]

