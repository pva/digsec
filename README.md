# digsec

[![CircleCI](https://circleci.com/gh/metebalci/digsec/tree/master.svg?style=svg)](https://circleci.com/gh/metebalci/digsec/tree/master)

dig like command line utility to understand DNSSEC.

# Install

`pip install digsec`

# Usage

Just run digsec to see options and help, or see this blog post https://metebalci.com/blog/a-minimum-complete-tutorial-of-dnssec/ .

# Hints

- digsec do not add DNS flags implicitly. You might need to use +rd (recursive desired) often.

- see `scripts/validate.py` to see a full validation and run for example `scripts/validate.py metebalci.com A`.

```
./validate.py metebalci.com A

--- querying ---
saving _root.DS (trust anchor)
digsec download +save-ds-anchors=/tmp/_root.IN
digsec v0.8.0
Trust-Anchor contains keytags: 19036-8, 20326-8
saving _root.DNSKEY
digsec query @8.8.8.8 . DNSKEY +rd +cd +do +udp_payload_size=2048 +save-answer +save-answer-dir=/tmp
digsec v0.8.0
saving com.DS
digsec query @8.8.8.8 com DS +rd +cd +do +udp_payload_size=2048 +save-answer +save-answer-dir=/tmp
digsec v0.8.0
saving com.DNSKEY
digsec query @8.8.8.8 com DNSKEY +rd +cd +do +udp_payload_size=2048 +save-answer +save-answer-dir=/tmp
digsec v0.8.0
saving metebalci.com.DS
digsec query @8.8.8.8 metebalci.com DS +rd +cd +do +udp_payload_size=2048 +save-answer +save-answer-dir=/tmp
digsec v0.8.0
saving metebalci.com.DNSKEY
digsec query @8.8.8.8 metebalci.com DNSKEY +rd +cd +do +udp_payload_size=2048 +save-answer +save-answer-dir=/tmp
digsec v0.8.0
saving metebalci.com.A
digsec query @8.8.8.8 metebalci.com A +rd +cd +do +udp_payload_size=2048 +save-answer +save-answer-dir=/tmp
digsec v0.8.0

--- validating ---
validating _root.DNSKEY with _root.DS (trust anchor)
digsec validate /tmp/_root.IN.DNSKEY /tmp/_root.IN.RRSIG.DNSKEY /tmp/_root.IN.DS
digsec v0.8.0
OK RRSIG (DNSKEY, RSASHA256) with DNSKEY (20326, RSASHA256)
OK DNSKEY (20326, RSASHA256) with DS (SHA-256)
validating com.DS with _root.DNSKEY
digsec validate /tmp/com.IN.DS /tmp/com.IN.RRSIG.DS /tmp/_root.IN.DNSKEY
digsec v0.8.0
OK RRSIG (DS, RSASHA256) with DNSKEY (18733, RSASHA256)
validating com.DNSKEY with com.DS
digsec validate /tmp/com.IN.DNSKEY /tmp/com.IN.RRSIG.DNSKEY /tmp/com.IN.DS
digsec v0.8.0
OK RRSIG (DNSKEY, RSASHA256) with DNSKEY (30909, RSASHA256)
OK DNSKEY (30909, RSASHA256) with DS (SHA-256)
validating metebalci.com.DS with com.DNSKEY
digsec validate /tmp/metebalci.com.IN.DS /tmp/metebalci.com.IN.RRSIG.DS /tmp/com.IN.DNSKEY
digsec v0.8.0
OK RRSIG (DS, RSASHA256) with DNSKEY (53929, RSASHA256)
validating metebalci.com.DNSKEY with metebalci.com.DS
digsec validate /tmp/metebalci.com.IN.DNSKEY /tmp/metebalci.com.IN.RRSIG.DNSKEY /tmp/metebalci.com.IN.DS
digsec v0.8.0
OK RRSIG (DNSKEY, ECDSAP256SHA256) with DNSKEY (2371, ECDSAP256SHA256)
OK DNSKEY (2371, ECDSAP256SHA256) with DS (SHA-256)
validating metebalci.com.A with metebalci.com.DNSKEY
digsec validate /tmp/metebalci.com.IN.A /tmp/metebalci.com.IN.RRSIG.A /tmp/metebalci.com.IN.DNSKEY
digsec v0.8.0
OK RRSIG (A, ECDSAP256SHA256) with DNSKEY (34505, ECDSAP256SHA256)
```

# Known Issues

- `scripts/validate.py` does not work with 2+ level domains e.g. www.metebalci.com

- some algorithms are not fully tested

# Release History

0.8:
  - pylint added to build process, but only important and easy to fix errors are fixed.
  - default timeout value of 1s is removed. now it defaults to system default. if needed, it can be set with +timeout=X_in_seconds_float flag.
  - tcp support with +tcp flag, default is udp
  - non-53 port support with @server_ip:server_port, default is 53
  - validate script is replaced with new scripts/validate.py
  - rsa dependency updated to 4.9, ecdsa dependency updated to 0.18.0

0.7.1:
  - rsa update in 0.7 broke the build, this version fixes the issue.

0.7:
  - required packages (rsa and ecdsa) are updated to latest version

0.6:
  - Socket timeout support and +timeout flag.

0.5:
  - Preliminary support for ECDSAP384SHA384, RSA-512, SHA-384.
  - Server the DNS packet is sent is written under NETWORK COMMUNICATION line.
  - digsec version is written at first line in the output as digsec vX.

0.4: 
  - ECDSAP256SHA256 implemented. 
  - @server option added. 
  - validate_second_level_domain.sh script added.
