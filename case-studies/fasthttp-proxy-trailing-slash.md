# fasthttp: an HTTP proxy URL ending in "/" broke every request

**Project:** [valyala/fasthttp](https://github.com/valyala/fasthttp) · **PR:** [#2548](https://github.com/valyala/fasthttp/pull/2548) (merged) · **Type:** input format / regression

## Symptom

With `HTTP_PROXY=http://127.0.0.1:3128/` (a common way to write a proxy URL), every request through `fasthttpproxy` failed before any connection was made:

```
strconv.Atoi: parsing "3128/": invalid syntax
```

It worked up to v1.55.0 and broke in v1.56.0.

## Reproduction

```go
dial := fasthttpproxy.FasthttpHTTPDialer("http://127.0.0.1:3128/")
_, err := dial("example.com:443")
// strconv.Atoi: parsing "3128/": invalid syntax
```

## Root cause

The address to dial was built as `host + path`, so the `/` at the end of the URL ended up in the port number. The bug came in with a refactoring in v1.56.0 (the old code dialed only the host). The SOCKS5 branch of the same file and Go's own `net/http` use only the host.

## Test design

Equivalence classes of a proxy URL, crossed with the two code paths:

| Proxy URL | Code path |
|---|---|
| trailing slash | proxy chosen once |
| trailing slash + credentials | proxy chosen once |
| trailing slash | proxy chosen on every dial (`NoProxy` set) |
| trailing slash + credentials | proxy chosen on every dial |

- **Unit test** `TestHTTPProxyURLWithPath` starts a local fake proxy and dials through it in all four cases. It fails on the old code with the `Atoi` error and passes with the fix.
- **End to end:** a real `fasthttp.Client` went through a local CONNECT proxy to a fasthttp server. Before the fix the request got the error, after it `200`. A `net/http` client with the same settings was used as the reference: the fixed behaviour matches it.
- **Regression run:** `go test -race -shuffle=on ./...` passed.

## Fix

Dial `pu.Host` only: a 3-line change in `fasthttpproxy/dialer.go`. A URL without a path gives the same address as before.

## Not covered

A proxy URL without a port (`http://proxy.local`) still fails with `missing port in address`. Choosing a default port is a separate product decision, so I mentioned it in the PR and left it out.
