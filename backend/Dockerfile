# ---------- Этап 1: сборка ----------
FROM golang:1.22 AS builder
WORKDIR /build

# Сначала зависимости — слой кэшируется, пока не меняется go.mod
COPY go.mod go.sum ./
RUN go mod download

# Потом исходники
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /out/app ./main.go

# ---------- Этап 2: исполнение ----------
FROM gcr.io/distroless/static-debian12:nonroot
WORKDIR /app
COPY --from=builder /out/app /app/app

EXPOSE 8080
ENTRYPOINT ["/app/app"]