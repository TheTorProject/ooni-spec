# Specification version number

0.3.0

# Specification name

DNS Spoof Test

# Test preconditions

  * An Internet connection.

  * A DNS server which will return client IP address and ISP Geoip information.

  * A Hostname to resolve and check for hijacking.

# Expected impact

  * The ability to detect interseption of DNS traffic on operator level when DNS requests are not forwarded to requested resolver, but DNS servers of ISP are used to make request to NS server hosting DNS zone.

# Expected inputs

This test expects the following arguments:

  * The address and port of the resolver to test 
  * The hostname to be resolved.
  * expected TXT record output

## Semantics

The resolver to test is passed by argument (-r) with ADDRESS:PORT convention. e.g. 8.8.8.8:53

The hostname to test is passed by argument (-h) as a FQDN, e.g. test.dns.google.com

A known good TXT expected result to test is passed by argument (-e) e.g. "Thanks for using Google Public DNS."  

# Test Description

We perform an TXT DNS query (via UDP) to the test resolver. We then compare
this answer (experiment answer) with the expected answer.

If the DNS payload of the received packets not matches identically, then spoofing
is considered to be occurring.

* Examples with dig utility *

Test against Google NS servers return TXT record only if request is coming from target Google Public DNS servers in case if DNS request is spoofed then it returns nothing. 
If empty answer is returned then it means that request was originated from non Google Public DNS servers.

```
dig +short @8.8.8.8 -p 53 -t txt test.dns.google.com
"Thanks for using Google Public DNS."
```

Test against maxmind.test-ipv6.com NS server returns client IP address and GeoIP data, which must match target resolver ISP data (Google in case of 8.8.8.8)  

```
dig +short @8.8.8.8 -t txt maxmind.test-ipv6.com
"ip='2a00:1450:4001:c11::103' as='15169' isp='GOOGLE' country='US'"
```

Test against maxmind.test-ipv6.com NS server returns client IP address and GeoIP data, which must match client IP address (can be NAT box IP address if used in ISP network).

```
dig +short -t txt maxmind.test-ipv6.com
"ip='2a02:8100:c1:0::7:0001' as='3209' isp='VODANET International IP-Backbone of Vodafone' country='DE'"
```

# Expected output

The output report will contain a boolean entry 'spoofing'.  If spoofing is
occurring, it shall be True. Otherwise, it is False.


## Required output data

  * Whether or not the DNS payload matches (spoofing).

## Semantics

In addition to the data specified in the parent data format, the following
field(s) are added to the report:

Version 0.0.1:

    spoofing: true|false

## Possible conclusions

Whether or not DNS spoofing is occurring for a particular DNS resolver and probe's ISP network.

## Example output sample

```
###########################################
# OONI Probe Report for dns_spoof (0.0.1)
# Wed Sep 25 15:39:32 2013
###########################################
---
input_hashes: []
options: [-r, ‘8.8.8.8:53', -h, 'test.dns.google.com', -e, 'Thanks for using Google Public DNS.']
probe_asn: AS2819
probe_cc: CZ
probe_ip: 127.0.0.1
software_name: ooniprobe
software_version: 1.0.0-rc3
start_time: 1380116372.573729
test_name: dns_spoof
test_version: 0.0.1
...
---
answer_flags: [ipsrc]
answered_packets:
- - raw_packet: !!binary |
      RbgA6OumAAAyEY4+CAgICH8AAAEANQA1ANSshgAAgYAAAQALAAAAAAZnb29nbGUDY29tAAABAAEG
      Z29vZ2xlA2NvbQAAAQABAAAA4AAErcIs5QZnb29nbGUDY29tAAABAAEAAADgAAStwizkBmdvb2ds
      ZQNjb20AAAEAAQAAAOAABK3CLOYGZ29vZ2xlA2NvbQAAAQABAAAA4AAErcIs6QZnb29nbGUDY29t
      AAABAAEAAADgAAStwizoBmdvb2dsZQNjb20AAAEAAQAAAOAABK3CLOcGZ29vZ2xlA2NvbQAAAQAB
      AAAA4AAErcIs4gZnb29nbGUDY29tAAABAAEAAADgAAStwizjBmdvb2dsZQNjb20AAAEAAQAAAOAA
      BK3CLOAGZ29vZ2xlA2NvbQAAAQABAAAA4AAErcIs4QZnb29nbGUDY29tAAABAAEAAADgAAStwizu
    summary: 'IP / UDP / DNS Ans "Thanks for using Google Public DNS." '
sent_packets:
- - raw_packet: !!binary |
      RQAAOAABAABAEfDWfwAAAQrTAAoANQA1ACRhpQAAAQAAAQAAAAAAAAZnb29nbGUDY29tAAABAAE=
    summary: 'IP / UDP / DNS Qry "test.dns.google.com’" '
spoofing: false
```

```
###########################################
# OONI Probe Report for dns_spoof (0.3.0)
# Sun Jun 13 15:40:41 2021
###########################################
---
input_hashes: []
options: [-r, ‘8.8.8.8:53', -h, 'maxmind.test-ipv6.com', -e, 'GOOGLE']
probe_asn: AS2819
probe_cc: CZ
probe_ip: 127.0.0.1
software_name: ooniprobe
software_version: 1.0.0-rc3
start_time: 1380116372.573729
test_name: dns_spoof
test_version: 0.3.0
...
---
answer_flags: [ipsrc]
answered_packets:
- - raw_packet: !!binary |
      RbgA6OumAAAyEY4+CAgICH8AAAEANQA1ANSshgAAgYAAAQALAAAAAAZnb29nbGUDY29tAAABAAEG
      Z29vZ2xlA2NvbQAAAQABAAAA4AAErcIs5QZnb29nbGUDY29tAAABAAEAAADgAAStwizkBmdvb2ds
      ZQNjb20AAAEAAQAAAOAABK3CLOYGZ29vZ2xlA2NvbQAAAQABAAAA4AAErcIs6QZnb29nbGUDY29t
      AAABAAEAAADgAAStwizoBmdvb2dsZQNjb20AAAEAAQAAAOAABK3CLOcGZ29vZ2xlA2NvbQAAAQAB
      AAAA4AAErcIs4gZnb29nbGUDY29tAAABAAEAAADgAAStwizjBmdvb2dsZQNjb20AAAEAAQAAAOAA
      BK3CLOAGZ29vZ2xlA2NvbQAAAQABAAAA4AAErcIs4QZnb29nbGUDY29tAAABAAEAAADgAAStwizu
    summary: 'IP / UDP / DNS Ans "ip='2a00:1450:4001:c11::103' as='15169' isp='GOOGLE' country='US'" '
sent_packets:
- - raw_packet: !!binary |
      RQAAOAABAABAEfDWfwAAAQrTAAoANQA1ACRhpQAAAQAAAQAAAAAAAAZnb29nbGUDY29tAAABAAE=
    summary: 'IP / UDP / DNS Qry "TXT maxmind.test-ipv6.com’" '
spoofing: false
```
