# TCP SYN Flood DoS Attack – Network Incident Analysis

## Project Overview

This project documents the investigation of a simulated Denial-of-Service (DoS) attack targeting a company's web server. The investigation was completed as part of the Google Cybersecurity Professional Certificate.

The objective was to analyze TCP/HTTP network traffic, identify suspicious activity, determine the type of attack, and assess its impact on website availability.

## Scenario

A company's web server experienced connection timeouts, preventing employees from accessing its sales webpage.

Network traffic analysis revealed repeated TCP SYN requests originating from an unknown IP address. As the traffic increased, the server began experiencing connection failures and became unable to respond reliably to legitimate users.

## Objectives

- Identify the type of network attack.
- Analyze TCP/HTTP traffic logs.
- Understand the TCP three-way handshake.
- Identify indicators of a SYN flood attack.
- Assess the impact on website availability.
- Recommend security measures to prevent similar incidents.

## Tools and Concepts

- Wireshark TCP/HTTP logs
- TCP/IP
- TCP three-way handshake
- SYN flood attacks
- Denial-of-Service (DoS)
- Network traffic analysis
- Incident investigation

## Section 1 – Attack Identification

The investigation identified a suspected TCP SYN flood Denial-of-Service (DoS) attack.

The web server was overwhelmed by an unusually high number of incoming TCP SYN requests, affecting its ability to respond to legitimate users.

The logs show that the IP address `203.0.113.0` repeatedly sent SYN packets to the web server at `192.0.2.1` on port `443`.

This behavior, combined with connection failures and HTTP timeout errors, indicates that the server was experiencing a SYN flood attack.

## Section 2 – Technical Analysis

### TCP Three-Way Handshake

Under normal conditions, a TCP connection is established through three steps:

1. **SYN:** The client sends a connection request to the server.
2. **SYN-ACK:** The server acknowledges the request and responds.
3. **ACK:** The client acknowledges the server's response, completing the connection.

Once established, the connection allows the client to communicate with the web server.

### Attack Mechanism

During a SYN flood attack, a malicious actor sends an excessive number of TCP SYN requests to the target server.

This traffic can overwhelm the server's connection-handling resources, reducing its ability to process legitimate requests.

As the attack continues, users may experience connection resets, slow responses, and website unavailability.

### Log Analysis

The provided TCP/HTTP logs contain evidence of suspicious network activity.

| Packet | Observation | Interpretation |
|---|---|---|
| 47–51 | Successful TCP handshake and HTTP 200 OK | Normal website communication |
| 52 | SYN from 203.0.113.0 | Initial observed suspicious connection |
| 57, 59, 61 | Repeated SYN requests | Abnormal connection activity |
| 73 | TCP RST-ACK | Connection reset |
| 77 | HTTP 504 Gateway Time-out | Website response failure |
| 78 | Additional SYN request | Suspicious traffic continues |

The repeated SYN packets from `203.0.113.0`, combined with failed legitimate connections, support the conclusion that the server was affected by a SYN flood attack.

The provided evidence shows one apparent attacking source, making a direct DoS attack the most appropriate classification. The logs do not establish a distributed attack.

## Business Impact

The attack negatively affected the availability of the company's website.

Potential consequences included:

- Employees being unable to access the sales webpage.
- Disruption of daily business operations.
- Delays in responding to customer requests.
- Reduced website reliability.
- Potential financial and reputational damage.

## Security Recommendations

To reduce the risk of future SYN flood attacks, the organization should consider:

1. Enabling SYN cookies or equivalent SYN flood protection.
2. Implementing connection rate limiting.
3. Monitoring abnormal TCP connection patterns.
4. Configuring firewall rules to filter suspicious traffic.
5. Using upstream DDoS mitigation services.
6. Establishing incident response procedures for service availability attacks.

Blocking a single suspicious IP address may provide temporary relief but is not sufficient protection against IP spoofing or attacks originating from multiple sources.

## Conclusion

The investigation identified a TCP SYN flood as the likely cause of the network interruption.

Analysis of the provided traffic logs revealed repeated SYN requests, connection resets, and HTTP timeout errors affecting legitimate users.

This project demonstrates foundational skills in network traffic analysis, attack identification, incident documentation, and security risk assessment.

## Disclaimer

This project is based on a fictional scenario and sample network logs provided through the Google Cybersecurity Professional Certificate. No real organization or production network was investigated.
