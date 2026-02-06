# Stage 1: Build frontend
FROM node:20-alpine AS frontend-builder
WORKDIR /app/frontend
COPY webui/frontend/package*.json ./
RUN npm ci
COPY webui/frontend/ ./
RUN npm run build

# Stage 2: Build Go backend
FROM golang:1.24-alpine AS backend-builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY pkg/ ./pkg/
COPY webui/server.go ./webui/
WORKDIR /app/webui
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o gebug-server server.go

# Stage 3: Distroless runtime
FROM gcr.io/distroless/static-debian12:nonroot
WORKDIR /app
COPY --from=backend-builder /app/webui/gebug-server /app/gebug-server
COPY --from=frontend-builder /app/frontend/dist /app/frontend/dist
ENV VUE_APP_PORT=3030
EXPOSE 3030
ENTRYPOINT ["/app/gebug-server"]