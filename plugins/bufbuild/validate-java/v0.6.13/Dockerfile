# syntax=docker/dockerfile:1.4
FROM golang:1.19.2-bullseye AS build
WORKDIR /app
COPY --link . /app
RUN CGO_ENABLED=0 go install -ldflags "-s -w" -trimpath github.com/envoyproxy/protoc-gen-validate@v0.6.13

FROM scratch
COPY --from=build --link /etc/passwd /etc/passwd
COPY --from=build --link --chown=root:root /go/bin/protoc-gen-validate .
USER nobody
ENTRYPOINT [ "/protoc-gen-validate" ]
