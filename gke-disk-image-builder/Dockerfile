FROM golang:1.25-trixie AS builder

WORKDIR /tmp/build

COPY ./go.mod ./go.sum ./
RUN go mod download

COPY . .

RUN go build -o image-builder ./cli

FROM debian:trixie-slim

RUN groupadd -g 1000 debian && \
    useradd -u 1000 -ms /bin/bash -g debian debian && \
    mkdir -p /app/script

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
        ca-certificates \
    && \
    apt-get autoclean && \
    apt-get autoremove -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

COPY script/startup.sh /app/script/
COPY script/verify.sh /app/script/

COPY --from=builder /tmp/build/image-builder /bin/

WORKDIR /app

USER debian

ENTRYPOINT ["/bin/image-builder"]
