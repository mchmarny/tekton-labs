FROM golang:1.18.1 as builder

WORKDIR /src/
COPY . /src/

ENV GO111MODULE=on

RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 \
    go build -a -tags netgo -mod vendor -o ./app .

FROM gcr.io/distroless/static:nonroot
COPY --from=builder /src/app .

ENTRYPOINT ["./app"]
