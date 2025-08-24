FROM golang:1.25-trixie AS builder

WORKDIR /tmp/build

COPY ./go.mod ./go.sum ./
RUN go mod download

#ENV GOFLAGS=-buildvcs=false
#ENV GOPRIVATE=github.com/paranoidd,gitlab.com/paranoid,gitlab.com/cloudminator

COPY . .

RUN go build -o image-builder ./cli

FROM debian:trixie-slim

RUN groupadd -g 1000 debian && \
    useradd -u 1000 -ms /bin/bash -g debian debian

WORKDIR /tmp

COPY --from=builder /tmp/build/image-builder /bin/

USER debian

ENTRYPOINT ["/bin/image-builder"]
