# Start from the official Golang image as a build stage
FROM golang:1.20 as build

# Set the working directory
WORKDIR /movie-app-backend

# Copy go.mod and go.sum files first to leverage Docker cache
COPY go.mod go.sum ./

# Download dependencies
RUN go mod tidy

# Copy the rest of the application source code
COPY . .

# Build the Go app
RUN go build -o /movie-app-backend/build/myapp ./cmd/api

# Use a minimal Docker image for running the application
FROM ubuntu:22.04

# Install necessary libraries for the binary (if needed)
RUN apt-get update && apt-get install -y ca-certificates

# Set the working directory
WORKDIR /movie-app-backend

# Copy the built binary from the build stage
COPY --from=build /movie-app-backend/build/myapp /movie-app-backend/build/myapp

# Ensure the binary has execute permissions
RUN chmod +x /movie-app-backend/build/myapp

# Expose the port on which the app runs
EXPOSE 8080

# Command to run the executable
CMD ["/movie-app-backend/build/myapp"]
