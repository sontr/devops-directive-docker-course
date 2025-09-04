FROM golang:1.25-trixie AS build

LABEL org.opencontainers.image.authors="sontr"

RUN useradd -u 1001 nonroot

WORKDIR /app

COPY go.mod go.sum ./

RUN --mount=type=cache,target=/go/pkg/mod \
  --mount=type=cache,target=/root/.cache/go-build \
  go mod download

COPY . .

RUN go build \
  -ldflags="-linkmode=external -extldflags=-static" \
  -tags netgo \
  -o api-golang

FROM scratch

ENV GIN_MODE=release

WORKDIR /

COPY --from=build /etc/passwd /etc/passwd
COPY --from=build /app/api-golang api-golang

USER nonroot

EXPOSE 8080

CMD ["/api-golang"]
