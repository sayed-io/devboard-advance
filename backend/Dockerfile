# Build stage
FROM golang:1-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -o devboard-backend .

# Runtime stage (minimal, no shell, runs as nonroot)
FROM gcr.io/distroless/static-debian12:nonroot

WORKDIR /app

COPY --from=builder /app/devboard-backend ./devboard-backend

USER nonroot

EXPOSE 8080

# Exec form for proper signal handling
ENTRYPOINT ["./devboard-backend"]

# To switch back to Docker Hardened Images later, run `docker login dhi.io`
# and replace the two FROM lines with:
#   FROM dhi.io/golang:1-alpine AS builder
#   FROM dhi.io/static:<tag from the DHI catalog>