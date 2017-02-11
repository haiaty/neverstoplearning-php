

# Guzzle Http client

Guzzle is a PHP HTTP client that makes it easy to send HTTP requests and trivial to integrate with web services.

Simple interface for building query strings, POST requests, streaming large uploads, streaming large downloads, using HTTP cookies, uploading JSON data, etc...
Can send both synchronous and asynchronous requests using the same interface.
Uses PSR-7 interfaces for requests, responses, and streams. This allows you to utilize other PSR-7 compatible libraries with Guzzle.
Abstracts away the underlying HTTP transport, allowing you to write environment and transport agnostic code; i.e., no hard dependency on cURL, PHP streams, sockets, or non-blocking event loops.
Middleware system allows you to augment and compose client behavior.
