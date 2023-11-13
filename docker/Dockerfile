# Stage 1: Create Custom JRE Image
FROM openjdk:21 as jre-builder

# Run jlink to create a custom JRE image
RUN jlink \
    --add-modules java.base \
    --strip-debug \
    --no-man-pages \
    --no-header-files \
    --compress=2 \
    --output /javaruntime

# Stage 2: Create the Final Distroless Image
FROM mcr.microsoft.com/openjdk/jdk:21-distroless

# Copy the custom JRE from the previous stage
COPY --from=jre-builder /javaruntime /javaruntime

# Add your benchmark JAR (assuming it's already available locally)
ADD https://github.com/renaissance-benchmarks/renaissance/releases/download/v0.15.0/renaissance-gpl-0.15.0.jar /app/renaissance-gpl-0.15.0.jar

# Define the default command for running the benchmark
CMD ["java", "--module-path", "/javaruntime", "-m", "app/renaissance-gpl-0.15.0.jar", "functional"]

