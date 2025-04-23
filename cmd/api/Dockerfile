# Stage 1: Build the Go binary
FROM golang:1.19.6-alpine AS builder

RUN apk --no-cache add make build-base git

WORKDIR /app
COPY go.mod go.sum ./

# Properly formatted Go module cache
RUN --mount=type=cache,id=go-mod-cache,target=/go/pkg/mod \
    go mod download -x

COPY . .

# Properly formatted Go build cache
RUN --mount=type=cache,id=go-build-cache,target=/root/.cache/go-build \
    BIN_BUILD_FLAGS="GOOS=linux" make build-api

# Stage 2: Create minimal production image
FROM alpine

COPY --from=builder /app/api /app/api
WORKDIR /app

ENTRYPOINT ["./api"]
