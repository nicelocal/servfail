# servfail

A coredns plugin that returns a SERVFAIL with the Authoritative and RecursionAvailable flags set.

Usage:

Add the following to `plugins.cfg`, before `debug`:

```
servfail:github.com/nicelocal/servfail
```

And use the following example Corefile:
```
. {
    servfail
}
```

Example dockerfile:

```
FROM golang

RUN git clone -b v1.12.0 --depth 1 https://github.com/coredns/coredns /coredns && \
    cd /coredns && \
    sed '/bind:bind/a servfail:github.com/nicelocal/servfail' plugin.cfg -i && \
    make

FROM scratch

COPY --from=0 /coredns/coredns /coredns

ENTRYPOINT ["/coredns"]
```