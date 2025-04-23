# Stage 1: Build Go binary
FROM golang:1.19.6-alpine AS builder

RUN apk --no-cache add make build-base git

WORKDIR /app
COPY go.mod go.sum ./

# Cache Go module downloads
RUN --mount=type=cache,target=/go/pkg/mod,id=builder-go-mod-cache go mod download -x

COPY . .

# Cache Go build output
RUN --mount=type=cache,target=/root/.cache/go-build,id=builder-go-build-cache \
    BIN_BUILD_FLAGS="GOOS=linux" make build-api

# Stage 2: Minimal image
FROM alpine

COPY --from=builder /app/api /app/api
WORKDIR /app

ENTRYPOINT ["./api"]
