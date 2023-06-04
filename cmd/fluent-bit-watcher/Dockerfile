FROM golang:1.16 as buildergo
ENV GORPXOY=https://goproxy.cn,direct
RUN mkdir -p /fluent-bit
RUN mkdir -p /code
COPY . /code/
WORKDIR /code
RUN echo $(ls -al /code)
RUN CGO_ENABLED=0 go build -ldflags '-w -s' -o /fluent-bit/fluent-bit /code/cmd/fluent-bit-watcher/main.go

FROM fluent/fluent-bit:1.9.10
LABEL Description="Fluent Bit docker image" Vendor="KubeSphere" Version="1.0"

COPY conf/fluent-bit.conf conf/parsers.conf /fluent-bit/etc/
COPY --from=buildergo /fluent-bit/fluent-bit /fluent-bit/bin/fluent-bit-watcher

# Entry point
ENTRYPOINT ["/fluent-bit/bin/fluent-bit-watcher"]
