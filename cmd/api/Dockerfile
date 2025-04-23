# Build stage
FROM golang:1.19.6-alpine AS builder

# Install build tools
RUN apk --no-cache add make build-base git

# Set up work directory
WORKDIR /app

# Copy module files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source and build the API binary
COPY . .
RUN BIN_BUILD_FLAGS="GOOS=linux" make build-api

# Final minimal image
FROM alpine

# Copy the compiled binary
COPY --from=builder /app/api /app/api

# Set working directory and entrypoint
WORKDIR /app
ENTRYPOINT ["./api"]
