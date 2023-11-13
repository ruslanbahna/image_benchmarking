# Use a suitable base image that includes a Java runtime
FROM mcr.microsoft.com/openjdk/jdk:21-distroless

# Set the working directory
WORKDIR /app

# Download the JAR file from the specified URL
ADD https://github.com/renaissance-benchmarks/renaissance/releases/download/v0.15.0/renaissance-gpl-0.15.0.jar /app/renaissance-gpl-0.15.0.jar

# Create a custom JRE with jlink (modify as needed)
RUN jlink --add-modules java.base \
         --strip-debug \
         --no-man-pages \
         --no-header-files \
         --compress=2 \
         --output /javaruntime

# Set the environment variable for the custom JRE
ENV PATH="$PATH:/javaruntime/bin"

# Specify the command to run your benchmark
CMD ["java", "-jar", "renaissance-gpl-0.15.0.jar", "functional"]
