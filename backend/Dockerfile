
##dockerfile
# Stage 1: Build
FROM golang:1.27.3-alpine AS builder

WORKDIR /app

RUN apk add --no-cache ca-certificates

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux \
    go build -trimpath -ldflags="-s -w" \
    -o devboard-backend .

# Stage 2: Runtime
FROM scratch

WORKDIR /app

COPY --from=builder /app/devboard-backend /devboard-backend
COPY --from=builder /etc/ssl/certs/ca-certificates.crt \
    /etc/ssl/certs/

USER nonroot

EXPOSE 8080

ENTRYPOINT ["./devboard-backend"] 