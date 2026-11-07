# Network Traffic Evidence – TCP SYN Flood Attack

## Overview

This document presents the network traffic evidence analyzed during a simulated TCP SYN Flood Denial-of-Service (DoS) incident.

The analysis is based on TCP/HTTP logs provided in the Google Cybersecurity Professional Certificate.

## Evidence 1 – Normal TCP Communication

Before the attack affected the server, legitimate clients were able to establish TCP connections and access the company's website.

| Packet | Source | Destination | Protocol | Information |
|---|---|---|---|---|
| 47 | 198.51.100.23 | 192.0.2.1 | TCP | 42584 → 443 [SYN] |
| 48 | 192.0.2.1 | 198.51.100.23 | TCP | 443 → 42584 [SYN, ACK] |
| 49 | 198.51.100.23 | 192.0.2.1 | TCP | 42584 → 443 [ACK] |
| 50 | 198.51.100.23 | 192.0.2.1 | HTTP | GET /sales.html HTTP/1.1 |
| 51 | 192.0.2.1 | 198.51.100.23 | HTTP | HTTP/1.1 200 OK |

**Analysis:**

Packets 47–49 demonstrate a successful TCP three-way handshake. Packets 50–51 show that the client successfully requested the sales webpage and received an HTTP 200 OK response.

This represents normal communication between a legitimate client and the web server.

## Evidence 2 – Suspicious TCP SYN Traffic

The logs show repeated TCP SYN packets from the suspicious IP address `203.0.113.0` targeting the web server at `192.0.2.1` on port 443.

| Packet | Time | Source | Destination | Protocol | Information |
|---|---|---|---|---|---|
| 57 | 3.664863 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |
| 59 | 3.795332 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |
| 61 | 3.939499 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |
| 66 | 4.256159 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |
| 68 | 5.236023 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |
| 70 | 5.237887 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |
| 72 | 6.229638 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |
| 78 | 7.351323 | 203.0.113.0 | 192.0.2.1 | TCP | 54770 → 443 [SYN] |

**Analysis:**

The repeated SYN packets indicate abnormal connection activity originating from a single apparent source.

The suspicious traffic continued while legitimate clients were attempting to communicate with the server.

This pattern is consistent with a TCP SYN flood attack, which can overwhelm the server's connection-handling resources and reduce its ability to process legitimate requests.

## Evidence 3 – Server Connection Failures

As the suspicious traffic continued, legitimate clients began experiencing connection failures.

| Packet | Time | Source | Destination | Protocol | Information |
|---|---|---|---|---|---|
| 69 | 5.236955 | 198.51.100.16 | 192.0.2.1 | TCP | 32641 → 443 [SYN] |
| 73 | 6.230548 | 192.0.2.1 | 198.51.100.16 | TCP | 443 → 32641 [RST, ACK] |
| 75 | 6.330885 | 198.51.100.7 | 192.0.2.1 | TCP | 42584 → 443 [SYN] |
| 77 | 7.330577 | 192.0.2.1 | 198.51.100.5 | TCP | HTTP/1.1 504 Gateway Time-out |

**Analysis:**

Packet 73 shows a TCP connection reset affecting a legitimate client.

Packet 77 shows an HTTP 504 Gateway Time-out error, indicating that a gateway did not receive a timely response from the server.

These events demonstrate that legitimate website traffic was being disrupted during the suspected SYN flood attack.

## Attack Identification

**Attack Type:** Denial-of-Service (DoS)

**Technique:** TCP SYN Flood

**Suspicious Source IP:** `203.0.113.0`

**Target Server IP:** `192.0.2.1`

**Target Port:** TCP 443

**Affected Service:** Company sales webpage (`/sales.html`)

**Observed Impact:** Connection resets, HTTP timeout errors, and reduced website availability.

## Conclusion

The network logs show repeated TCP SYN requests from a suspicious source, followed by connection failures affecting legitimate users.

The observed traffic is consistent with a TCP SYN Flood DoS attack.

The incident demonstrates how excessive connection requests can overwhelm a web server and disrupt access to business-critical services.

## Source

Google Cybersecurity Professional Certificate – Networks and Network Security.

The network traffic data was provided as part of a simulated educational incident. This analysis does not represent an investigation of a live production network.
