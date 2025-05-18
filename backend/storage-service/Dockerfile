# Build stage
FROM golang:1.24-alpine AS builder

WORKDIR /app

# Copy go mod and sum files
COPY go.mod go.sum ./

# Download dependencies
RUN go mod download

# Copy source code
COPY . .

# Build the application
RUN CGO_ENABLED=0 GOOS=linux go build -o storage-service .

# Runtime stage
FROM alpine:latest

WORKDIR /app

# Create uploads directory
RUN mkdir -p /app/uploads

# Copy binary from build stage
COPY --from=builder /app/storage-service .

# Define uploads directory as a volume
VOLUME ["/app/uploads"]

# Expose port
EXPOSE 8080

# Run the application
CMD ["./storage-service"]