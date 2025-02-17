# syntax=docker/dockerfile:1.4
FROM debian:bullseye-20221004 AS build

ARG TARGETARCH

RUN apt-get update \
 && apt-get install -y curl git cmake build-essential autoconf clang libc++-dev libtool pkg-config g++ unzip zip
RUN arch=${TARGETARCH}; \
    if [ "${arch}" = "amd64" ]; then arch="x86_64"; fi; \
    curl -fsSL -o /usr/local/bin/bazel https://github.com/bazelbuild/bazel/releases/download/5.3.2/bazel-5.3.2-linux-${arch} \
 && chmod +x /usr/local/bin/bazel

WORKDIR /build

RUN git clone --depth 1 --branch v1.50.1 --recursive https://github.com/grpc/grpc
WORKDIR /build/grpc
RUN bazel build //src/compiler:grpc_plugin_support
RUN bazel build //src/compiler:grpc_python_plugin

FROM debian:bullseye-20221004-slim
COPY --from=build --link --chmod=0755 /build/grpc/bazel-bin/src/compiler/grpc_python_plugin .
USER nobody
ENTRYPOINT ["/grpc_python_plugin"]
