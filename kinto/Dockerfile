FROM golang:alpine AS builder
RUN apk update && apk add --no-cache git bash wget curl
WORKDIR /go/src/v2ray.com/core
RUN git clone --progress https://github.com/v2fly/v2ray-core.git . && \
    bash ./release/user-package.sh nosource noconf codename=$(git describe --tags) buildname=docker-fly abpathtgz=/tmp/v2ray.tgz

FROM alpine
ENV CONFIG="https://gist.githubusercontent.com/K-ISSM/77cd1fb4933c11be339cf76d7d511248/raw/5748f3e8e2dc579e338f2af9c0e150bfb1925b9c/config.json"

COPY --from=builder /tmp/v2ray.tgz /tmp
RUN apk update && apk add --no-cache tor ca-certificates && \
    mkdir -p /usr/bin/v2ray && \
    tar xvfz /tmp/v2ray.tgz -C /usr/bin/v2ray && \
    rm -rf /tmp/v2ray.tgz
    
CMD nohup tor & \
    /usr/bin/v2ray/v2ray -config $CONFIG
