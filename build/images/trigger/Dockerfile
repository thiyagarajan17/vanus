FROM --platform=$BUILDPLATFORM golang:1.18 as builder
WORKDIR /workspace

COPY . .
RUN go mod download

ARG TARGETOS
ARG TARGETARCH
RUN GOOS=$TARGETOS GOARCH=$TARGETARCH make build-trigger && \
    wget https://github.com/etcd-io/etcd/releases/download/v3.5.7/etcd-v3.5.7-linux-amd64.tar.gz >/dev/null 2>&1 && \
    tar -zxvf etcd-v3.5.7-linux-amd64.tar.gz 1>/dev/null && \
    mv etcd-v3.5.7-linux-amd64/etcdctl /workspace/bin/etcdctl

FROM ubuntu:22.10
WORKDIR /vanus
COPY --from=builder /workspace/bin/trigger bin/trigger
COPY --from=builder /workspace/bin/etcdctl bin/etcdctl
ENTRYPOINT ["bin/trigger"]

