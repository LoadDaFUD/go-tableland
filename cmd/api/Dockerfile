FROM golang:1.19.6-alpine AS builder

RUN apk --no-cache add make build-base git

WORKDIR /app
COPY go.mod go.sum ./

RUN --mount=type=cache,id=s/go-mod-cache,target=/go/pkg/mod \
    go mod download -x

COPY . .

RUN --mount=type=cache,id=s/go-build-cache,target=/root/.cache/go-build \
    BIN_BUILD_FLAGS="GOOS=linux" make build-api

FROM alpine

COPY --from=builder /app/api /app/api
WORKDIR /app

ENTRYPOINT ["./api"]
