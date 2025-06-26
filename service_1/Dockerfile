FROM golang:1.22-alpine AS build

WORKDIR /app


COPY main.go .

RUN go build -o service_1 main.go

# --- Final Stage ---
FROM alpine:latest AS runtime

WORKDIR /app
RUN apk add --no-cache curl
COPY --from=build /app/service_1 .

EXPOSE 8001
CMD ["./service_1"]
