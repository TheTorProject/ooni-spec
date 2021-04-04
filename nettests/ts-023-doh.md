# Specification version number

0.1.0

# Specification name

DNS over HTTPS Test

# Test preconditions

  * An internet connection

# Expected impact

Result of resolving domain names using a specific DoH server.

# Expected inputs

## Import document or import data format

A list of domains for which to perform a DoH resolution.

## Data specification version number

## Semantics

The input document will contain a list of zero or more domains to
resolve using DoH. The following

```
www.google.com
www.facebook.com
```

are all valid entries.

You should also specify the DoH URL to use. We will by default use the
`https://mozilla.cloudflare-dns.com/dns-query` URL, if none is given.

# Test description

For every input domain we perform a DoH resolution.

# Expected output

The key `test_keys` key object will be added to the report.

The `test_keys.failure` scalar will be `null` if there was no error or
a string indicating the error that occurred.

## Example output sample

```
{
  "data_format_version": "0.2.1",
  "input": "www.google.com",
  "measurement_start_time": "2019-10-08 12:04:17",
  "test_runtime": 0,
  "probe_asn": "AS30722",
  "probe_cc": "IT",
  "probe_ip": "127.0.0.1",
  "report_id": "20191008T120417Z_AS30722_N4ZDqONxMM76DlDnaEyQfu1DNhE5yz7WxmlmnbxYlJb05Qpm0a",
  "resolver_ip": "173.194.170.111",
  "software_name": "miniooni",
  "software_version": "0.1.0-dev",
  "test_keys": {
    "failure": "",
    "url": "https://mozilla.cloudflare-dns.com/dns-query",
    "x-addresses": [
      "216.58.205.132",
      "2a00:1450:4002:808::2004"
    ]
  },
  "test_name": "doh",
  "test_start_time": "2019-10-08 12:04:17",
  "test_version": "0.1.0"
}
```
