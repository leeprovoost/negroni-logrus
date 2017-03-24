# negroni-logrus

[![GoDoc](https://godoc.org/github.com/meatballhat/negroni-logrus?status.svg)](https://godoc.org/github.com/meatballhat/negroni-logrus)
[![Build Status](https://travis-ci.org/meatballhat/negroni-logrus.svg?branch=master)](https://travis-ci.org/meatballhat/negroni-logrus)

logrus middleware for negroni

## Usage

Take a peek at the [basic example](./examples/basic/example.go) or the [custom
example](./examples/custom/example.go), the latter of which includes setting a
custom log level on the logger with `NewCustomMiddleware`.

If you want to reuse an already initialized `logrus.Logger`, you should be using
`NewMiddlewareFromLogger` (see [existinglogrus](./examples/existinglogrus/example.go)).

## Note on client IP spoofing

This package tries to figure out the client IP address when sitting behind a proxy
or load-balancer (e.g. AWS ELB). It first inspects the `X-Real-IP` header, followed
by inspecting the `X-Forwarded-For` header.

These headers are typically added by your proxy or load-balancer. The `X-Forwarded-For`
header can contain multiple IP addresses because every proxy or load-balancer will add
another entry. This means that a client can try to spoof the original IP address by
calling your service with an `X-Forwarded-For` header present.

If this is a genuine concern for you, then you might want to implement your own
Before Hook to deal with this. This package can't do this for you because everyone's
environment will look differently.
