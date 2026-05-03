# Incident Report: SSH Brute Force Attack Detection

## Date: 2026-05-03
## Analyst: Shiva
## Incident Type: SSH Brute Force Attack

## 1. Summary
A brute force attack targeting the SSH service (port 22) was detected on a local system. The attack involved repeated login attempts using multiple passwords against the root account.

## 2. Detection Method
The activity was identified through network traffic analysis using Wireshark. A display filter tcp.port == 22 was applied to isolate SSH traffic. Suspicious behavior was observed in the form of repeated connection attempts.

## 3. Affected System
- Host IP: 127.0.0.1
- Service Targeted: SSH
- Username Targeted: root

## 4. Attack Details
- Tool Used: Hydra (automated brute force tool)
- Attack Type: Password brute force
- Behavior Observed:
  - Multiple TCP 3-way handshakes in rapid succession
  - Each connection attempt followed by immediate termination (FIN packets)
  - High frequency of attempts indicating automation, not human activity
  - Consistent source and destination IP (localhost lab simulation)

## 5. Evidence Collected
- Packet Capture File: brute_force_capture.pcapng
- Key Indicators:
  - Repeated SYN to SYN-ACK to ACK sequences
  - Short-lived sessions
  - High volume of connection attempts within a short time

## 6. Timeline
- 10:43:44.477 - First SYN packet observed, attack initiated on port 22
- 10:43:44.903 - Final FIN packet, attack concluded
- Total duration: Less than 1 second
- Total attempts: 8 TCP conversations (1 probe + 7 password attempts)

## 7. Analysis
The traffic pattern confirms a brute force attack:
- Rapid repeated authentication attempts
- No sustained sessions indicating failed logins
- Automated timing between attempts
- Entire attack completed in under 1 second confirming automated tool usage

## 8. Impact Assessment
- No successful login observed
- No system compromise detected
- Attack limited to authentication attempts

## 9. Response Actions Taken
- Captured and preserved network traffic
- Isolated SSH traffic for focused analysis
- Identified attack pattern and source
- Documented findings

## 10. Recommendations
- Disable root login via SSH
- Implement strong passwords
- Use SSH key-based authentication
- Enable rate limiting or tools like Fail2Ban
- Monitor for repeated failed login attempts

## 11. Conclusion
The incident was a brute force attack against SSH identified through packet-level analysis. The attack was unsuccessful but demonstrates a clear security risk if proper protections are not implemented.
