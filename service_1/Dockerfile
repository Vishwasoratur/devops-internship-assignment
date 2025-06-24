# service_1/Dockerfile

# --- Stage 1: Build the Go application ---
# Use the official Go image as a builder
FROM golang:1.22 AS builder

# Set the working directory inside the container
WORKDIR /app

# Copy the Go application source code into the container
# This assumes your Go app's main file and go.mod/go.sum are in the service_1 directory
COPY . .

# Build the Go application
# CGO_ENABLED=0 disables CGO, making the binary statically linked and smaller
# -o main specifies the output executable name as 'main'
# ./main.go specifies the entry point of your application, based on the provided README
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix nocgo -o main ./main.go

# --- Stage 2: Create a minimal runtime image ---
# Use a minimal Alpine Linux image for the final runtime
FROM alpine:latest

# Set the working directory inside the container
WORKDIR /app

# Install curl for health checks in the Alpine image
# 'apk add --no-cache curl' is used for Alpine Linux
RUN apk add --no-cache curl

# Copy the built executable from the builder stage
COPY --from=builder /app/main .

# Expose the port your Go application listens on
# IMPORTANT: This is port 8001 based on your Go API README.md
EXPOSE 8001

# Command to run the application when the container starts
CMD ["./main"]


