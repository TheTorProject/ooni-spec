# tq-027 Stuffed TLS ClientHello

If a TLS request triggers an unexpected response it is interesting to try
“stuffing” tricks to check if the SNI field placed in a separate packet triggers a
different behavior. Possible fields for "stuffing" are:

- Session ID, 256 bytes
- Cipher Suites, ~16 Kbytes
- Compression Methods, 256 bytes
- Extensions (especially Padding), ~16 Kbytes

The whole ClientHello packet is limited to 16 Kbytes.

This technique is different from [tq-024 TCP Segmentation](./tq-024-TCP-segmentation.md)
as it does not produce undersized packets.
