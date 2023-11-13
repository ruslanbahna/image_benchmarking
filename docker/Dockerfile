# Use a base image with a shell to create the custom JRE
FROM ubuntu:20.04 AS jre-builder

# Install the necessary tools
RUN apt-get update && apt-get install -y openjdk-17-jdk-headless binutils

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

# Create a directory for your benchmark application
WORKDIR /app

# Copy the benchmark JAR from your local directory to the image
# You should use the COPY command here, as you want to include the JAR file in the final image.
COPY renaissance-gpl-0.15.0.jar /app/renaissance-gpl-0.15.0.jar

# Run the benchmark application
CMD ["java", "-jar", "renaissance-gpl-0.15.0.jar"]

