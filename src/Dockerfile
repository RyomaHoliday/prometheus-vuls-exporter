FROM golang:1.22 AS build-stage

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . ./

RUN CGO_ENABLED=0 GOOS=linux go build  -o /prometheus-vuls-exporter

# Deploy the application binary into a lean image
FROM gcr.io/distroless/base-debian12 AS build-release-stage

WORKDIR /

COPY --from=build-stage /prometheus-vuls-exporter /prometheus-vuls-exporter

EXPOSE 8080

USER nonroot:nonroot

ENTRYPOINT ["/prometheus-vuls-exporter"]