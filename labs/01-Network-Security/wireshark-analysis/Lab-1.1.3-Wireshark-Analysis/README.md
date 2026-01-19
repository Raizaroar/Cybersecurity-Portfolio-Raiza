# LAB 1.1.3: Network Traffic Analysis with Wireshark

**Status:** Completed 

**Category:** Network Security 

**Difficulty:** Entry Level  

**Estimated Time:** 2-3 hours  

## Executive Summary

Successfully installed and configured Wireshark for network traffic analysis. Performed live packet captures, analyzed multiple protocols (ICMP, DNS, HTTP, TLS), and demonstrated ability to detect malicious network activity including port scanning reconnaissance.

## Objective

- Install and configure Wireshark with proper drivers
- Capture live network traffic from multiple interfaces
- Understand packet structure (OSI layers in practice)
- Master display filters for traffic isolation
- Detect port scanning patterns
- Analyze DNS traffic for anomalies
- Follow TCP streams to reconstruct communications
- Establish network baseline for anomaly detection
- Export evidence for incident response

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| **Wireshark** | 4.0+ | Packet capture and analysis |
| **Npcap/libpcap** | Latest | Packet capture driver |
| **Display Filters** | Built-in | Traffic isolation and analysis |
| **Statistics Tools** | Built-in | Traffic pattern visualization |


## Step-by-Step Implementation

   - Step 1: Wireshark Installation and Configuration
   - Step 2: First Packet Capture - ICMP Analysis
   - Step 3: Display Filter Mastery
   - Step 4: Port Scan Detection
   - Step 5: DNS Traffic Analysis
   - Step 6: TCP Stream Following
   - Step 7: Network Baseline Establishment

## Key Protocols Analyzed

| **Protocol** | **Layer**       | **Purpose**              | **Security Relevance**                      |
|--------------|------------------|--------------------------|---------------------------------------------|
| ICMP         | Network           | Ping, diagnostics         | Host discovery, tunneling                   |
| DNS          | Application       | Name resolution           | C2 communication, tunneling                 |
| TCP          | Transport         | Reliable connections      | Port scanning, session hijacking            |
| HTTP         | Application       | Web traffic               | Credential theft, data exfiltration         |
| TLS          | Presentation      | Encryption                | Malware encryption, legitimate privacy      |
| ARP          | Data Link         | IP-to-MAC mapping         | ARP spoofing, MITM attacks                  |



## Key Achievements

- **Packet Capture Proficiency:** Captured and analyzed 1000+ packets across multiple protocols
- **Filter Mastery:** Created 15+ display filters for traffic isolation
- **Attack Detection:** Identified port scan patterns using TCP SYN analysis
- **Protocol Analysis:** Deconstructed DNS, HTTP, and TLS communications
- **Baseline Establishment:** Documented normal network behavior for anomaly detection

## Skills Demonstrated

- Network protocol analysis (OSI Layers 2-7)
- Traffic filtering and correlation
- Attack pattern recognition
- Evidence collection and preservation
- Professional documentation of network investigations

---
## Lessons Learned

- Technical Skills

   - Wireshark is essential: Can't be a SOC Analyst without packet analysis skills
   - Filters are everything: 90% of Wireshark proficiency is filter mastery
   - OSI model in practice: Packets literally show each layer
   - Encryption is critical: Plaintext traffic exposes everything

## Security Insights

   - Network tells stories: Every attack leaves packet-level evidence
   - Baselines detect anomalies: Can't find unusual without knowing normal
   - Metadata matters: Even encrypted traffic reveals patterns (IPs, timing, volume)
   - Defense in depth: Network monitoring is one layer of many

## SOC Analyst Perspective

   - Speed matters: Quick filtering saves investigation time
   - Documentation is key: Future-you needs notes
   - Think attacker mindset: What would they do? How would it look in packets?
   - Context is critical: Single packet meaningless, patterns reveal intent


## Conclusion

Successfully completed comprehensive Wireshark training covering installation, configuration, packet capture, protocol analysis, and security-focused traffic investigation. Demonstrated ability to:
- Capture & Analyze: Live traffic across multiple protocols
- Filter & Isolate: Signal from noise using display filters
- Detect Threats: Port scanning patterns, anomalous DNS
- Reconstruct Communications: TCP stream following
- Establish Baselines: Normal behavior documentation
- Professional Documentation: Evidence collection and reporting

**Next lab  1.1.4 - Malware Detection with VirusTotal**