##  LAB 2.2.1: Detection Engineering Deep Dive - YARA & Advanced Rules

 **Date:** 10-02-2026
 
 **Analyst:** Raiza
 
 **Status:** Completed
 
 **Category:** SIEM Operations
 
 **Difficulty:** Intermediate
 
 **Time Invested:** 5 Hours


## Executive Summary

Successfully deployed comprehensive detection engineering framework combining signature-based (YARA), behavior-based, and anomaly-based detection methods. Created 21 total detections covering 11 MITRE ATT&CK tactics. Achieved 65% MITRE coverage with clear roadmap to 80%.

## OBJECTIVE

- Install and configure YARA on Kali Linux
- Create YARA rules for malware detection
- Integrate YARA with Splunk
- Build behavior-based detections (not just IOCs)
- Implement threat scoring system
- Create detection maturity matrix
- Build automated detection testing framework
- Document detection library professionally


## Tools Used

| Tool                  | Purpose                        | Why This Tool?                          |
|-----------------------|--------------------------------|-----------------------------------------|
| **YARA**              | Malware pattern matching       | Industry standard, file/memory scanning |
| **Splunk**            | SIEM correlation               | From previous days                      |
| **yarGen**            | Automatic YARA generation      | Speeds up rule creation                 |
| **Capa**              | Malware capability detection   | Behavior-based analysis                 |
| **MITRE ATT&CK Navigator** | Coverage visualization     | Gap analysis                            |


## Key Achievements

- **YARA Rules:** 4 production-ready rules
- **Behavior Detections:** 3 advanced analytics
- **Threat Scoring:** Multi-factor scoring system
- **Maturity Assessment:** Complete ATT&CK coverage matrix
- **Testing Framework:** Automated validation

**Behavior Detections:**

1. Unusual Process Graph Relationships
2. Anomalous Network Beaconing
3. Data Staging Before Exfiltration

## Conclusion 

I implemented a comprehensive detection framework that combines signature-based (YARA), behavioral (Capa), and SIEM correlation (Splunk) methods. This allowed us to achieve 65% coverage of MITRE ATT&CK tactics, with a clear plan to reach 80%. In addition, the integration of tools such as yarGen and MITRE ATT&CK Navigator shows that the goal was not only to detect threats, but also to optimize rule creation and visualize coverage gaps.

Overall, it can be concluded that the project is not limited to isolated tests, but constitutes a mature detection and security engineering system, with professional documentation and a scalable approach to continuous improvement.
