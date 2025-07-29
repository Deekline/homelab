FROM golang:1.24-alpine as builder

WORKDIR /app

# Install dependencies
RUN apk add --no-cache git ca-certificates gcc musl-dev

# Install xk6
RUN go install go.k6.io/xk6/cmd/xk6@latest

# Build k6 with only the InfluxDB extension
RUN xk6 build --with github.com/grafana/xk6-output-influxdb@latest

# Use the browser-enabled base image
FROM grafana/k6:latest-with-browser

# Replace k6 binary with our custom build
COPY --from=builder /app/k6 /usr/bin/k6

ENTRYPOINT ["k6"]
