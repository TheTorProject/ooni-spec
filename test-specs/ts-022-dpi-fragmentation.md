# Specification version number

2017-12-18-001

# Specification name

[XXX] DPI Detection Test?

# Test preconditions

  * An internet connection.
  * A URL we suspect is being blocked by a stateless/non-reassembling DPI box.

# Expected impact

  * If a URL is being blocked by DPI (not by IP or DNS blocking) into the TCP
    stream, we should be able to determine whether that DPI box reassembles
    streams or if it only looks at one packet at a time.

# Expected inputs

  * A list of URLs to be tested (that we already know are blocked in
    some fashion).

## Semantics

The test takes as input a list of URLs, one per line. For example:

    http://torproject.org
    https://ooni.nu

# Test description

For every hostname, we perform two HTTP connections--one with fragmentation
and one without--and compare them to see if the response differs. If the
input scheme is http, we fragment on the HTTP Host header; if the scheme
is https, we fragment on the SNI header in the TLS Client Hello.

# Expected output

## Parent data format

df-001-httpt [XXX?]

## Semantics

[XXX] I think there will be one boolean value for each URL input: whether
or not fragmenting around the plaintext hostname results in a different
HTTP response. Also, we will want to include the DNS requests
and responses, and the full HTTP requests and responses.

## Possible conclusions

Determing whether or not a censorship device reassembles TCP streams can
narrow down what type of technology is being used. For example, an HTTP
proxy like Squid has a stream-level view of the connection, while a DPI
box from Cisco probably does not reassemble lower-level packets into a
stream.

## Example output sample

```
{
}
```

# Privacy considerations

[XXX]
