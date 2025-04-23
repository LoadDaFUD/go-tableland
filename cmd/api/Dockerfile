# Build phase: Set up Go environment
FROM golang:1.19.6-alpine AS builder

RUN apk --no-cache add make build-base git

WORKDIR /app
COPY go.mod go.sum ./
# Cache Go modules
RUN --mount=type=cache,id=go-mod-cache,target=/go/pkg/mod go mod download -x

COPY . .

# Cache Go build output
RUN --mount=type=cache,id=go-build-cache,target=/root/.cache/go-build \
  BIN_BUILD_FLAGS="GOOS=linux" make build-api

# Production phase: Use lightweight alpine image
FROM alpine

# Copy the built binary from the builder image
COPY --from=builder /app/api /app/api
WORKDIR /app

ENTRYPOINT ["./api"]
