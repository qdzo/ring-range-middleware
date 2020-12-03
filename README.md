# ring-range-middleware

HTTP Range middleware for the Clojure Ring server

Given a response body and a valid Range header, this middleware fulfills the request's Range header. It works for bodies where the content length is either known or unknown, but uses way less memory when it's known.

The middleware looks for the following to determine the content length:
- if the body is a string, File, or byte-array
- the response Content-Length

Otherwise, this middleware has to buffer the body. There is an option for buffer size you can give to the handler.

## Usage

Leinengen
```
[ring-range-middleware "0.1.0"]
```

You can use it like normal Ring middleware.

```clojure
(require [ring-range-middleware.core :as range-middleware])
...
(-> your-handler
    ...
    (range-middleware/wrap-range-header)
    ...)
```

### Options

You can use the handler with options.

```clojure
(-> your-handler
    ...
    (range-middleware/wrap-range-header opts)
    ...)
```

`opts` is a map with the following keys:
- :boundary-generator-fn
  - a function that generates a boundary string
  - default: an autogenerated 30-character alphanumeric string
- :max-num-ranges
  - the maximum number of ranges to accept in a Range request
  - default: 10
- :max-buffer-size-per-range-bytes
  - when the content length is unknown, the max buffer size for each range. If the buffer size is surpassed, the range is discarded.
  - default: 1MiB

## License

MIT
