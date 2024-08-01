FROM golang:1.22 as builder
WORKDIR /go/src

COPY go.mod go.sum ./
RUN go mod download
COPY ./main.go ./
ARG CGO_ENABLED=0
ARG GOOS=linux
ARG GOARCH=amd64
RUN go build \
  -o /go/bin/main \
  -ldflags '-s -w'

FROM busybox:1.36
WORKDIR /app
ENV APP_VERSION="v2"
COPY --from=builder /go/bin/main /app/main
ADD public/index.html /app/public/index.html
ADD public/script.js /app/public/script.js
ADD public/style.css /app/public/style.css
ADD public/jquery.min.js /app/public/jquery.min.js

ENTRYPOINT ["/app/main"]
EXPOSE 3000
