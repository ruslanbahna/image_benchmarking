# Use a base image that includes jlink and the necessary modules (e.g., OpenJDK base image)
FROM openjdk:17 AS builder

# Create a custom JRE image using jlink
RUN jlink \
    --add-modules java.base,java.sql,java.naming,java.desktop \
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=2 \
    --output /jre

# Create a minimal final image
FROM debian:bullseye-slim

# Copy the custom JRE from the builder stage
COPY --from=builder /jre /jre

# Set the JAVA_HOME and PATH environment variables
ENV JAVA_HOME=/jre
ENV PATH="$PATH:$JAVA_HOME/bin"

# Set the default command for the container
CMD ["java", "-version"]
