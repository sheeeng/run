FROM golang:1.15.0-buster as go-builder
WORKDIR /module
COPY . /module/
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -a -tags netgo \
      -ldflags '-w -extldflags "-static"' \
      -mod vendor \
      -o run-integration-backend

FROM alpine:latest as certs
RUN apk --update add ca-certificates

FROM scratch
COPY --from=go-builder /module/run-integration-backend .
COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["/run-integration-backend"]
