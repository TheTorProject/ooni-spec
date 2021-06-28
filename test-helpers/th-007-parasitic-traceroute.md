# Specification version number

2017-10-29-000

# Specification name

Parasitic Traceroute Test Helper

# Helper description

The parasitic traceroute test helper behaves much the same as the reverse
traceroute helper - it listens for incoming connections on a TCP socket. Once
initiated, the server will respond with a response message modulating the
TTL of the connection until a successful ACK is received from the connecting
client. As such, the traceroute is performed on the initiated connection itself,
potentially revealing more network hops than are present on the public IP
that the remote destination visible to the helper suggests. The result of the
traceroute process is then returned on the same connection.

# Helper preconditions

 * An Internet connection
 * An Internet-Reachable TCP Port
 * Raw socket access for monitoring and injecting packets

# Expected impact

In addition to the core Internet traceroute data, parasitic traceroute has the
ability to shed light on the internal routing of ISPs in a way that is otherwise
difficult for a non-privileged client to obtain. While this data can be
sensitive, initial OONI tests using this helper can provide insights like the
number of hops within the local network, along with potential indicators of
multiple NAT devices in play, where RFC-1917 networks are observed, or when hops
after the public IP are seen, but do not trace all the way to the client.
Knowledge of the presence of these configurations within a network can allow
the overall OONI system do a better job of understanding the representativeness
of submitted reports.

# Expected inputs

 * The initiation of a connection.

# Expected output

 * The results of a traceroute giving the IP address of each hop and, if
   available, the ping times to each hop.

The result will be encoded using JSON, with an array where each element
represents a successive probe response when present. Each entry in the array
will be a JSON object, containing keys for the TTL of the probe, the IP
address(es) seen responding, and observed latencies.

# Possible conclusions

A client will be able to better understand the network condition that it is in.
Submitted conclusions will allow (in addition to the conclusions from other
forms of traceroute) the conclusion that a client is in a multi-NAT
configuration, and the Internal IPs of some ISP gateways, which may not be
otherwise visible.

# Notes

This test helper attempts to formalize the semantics of the Parasitic Traceroute
(https://github.com/david415/ParasiticTraceroute) program by David415, and the
Traceroute as a Service (https://github.com/willscott/traas) program by Will
Scott.
