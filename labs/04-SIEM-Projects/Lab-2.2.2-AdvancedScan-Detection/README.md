# Advanced Port Scanning Detection & Behavioral Analysis

**Date:** 10-02-2026

**Analyst:** Raiza

**Status:** Completed

**Category:** SIEM Operations

**Difficulty:** Intermediate

**Time Invested:** 5 Hours

## Executive Summary

Detected and analyzed 5 different types of advanced port scanning techniques including stealth scans and evasion tactics.

***Business Impact:*** Stealth scans are precursors to targeted attacks. Detecting them early can prevent security breaches that would cost millions.

## Objective

- Detect advanced scanning techniques (SYN, ACK, FIN, XMAS, NULL scans)
- Identify slow scans and distributed scans
- Create behavior-based detection rules
- Generate automatic alerts with severity scoring
- Document IOCs (Indicators of Compromise) for threat intelligence

## Tools Used

| Tool              | Description                          | Purpose                                                                 |
|-------------------|--------------------------------------|-------------------------------------------------------------------------|
| **Wireshark**     | Deep traffic analysis                | Inspect and troubleshoot network packets for anomalies or suspicious activity |
| **Zeek (Bro)**    | Network Security Monitor             | Provide detailed network visibility and generate logs for security analysis |
| **Suricata**      | IDS/IPS Engine                       | Detect and prevent intrusions by analyzing network traffic in real time |
| **Python**        | Custom detection scripts             | Automate detection tasks and create tailored security workflows          |
| **Jupyter Notebook** | Documentation and analysis        | Record experiments, visualize data, and present findings in a reproducible format |


## HANDS-ON LAB - STEP BY STEP

1. PHASE: Generating Advanced Scan Traffic (Attacker Perspective)
2. PHASE: Detection & Analysis
3. PHASE: Python Script for Automated Analysis
4. PHASE: Documentation & Reporting

##  Key Findings

- **Attacker IP:** 192.168.100.10
- **Scan Types Detected:** SYN, FIN, XMAS, NULL, ACK
- **Total Ports Scanned:** 1,000+
- **Duration:** 45 minutes (slow scan detected)

##  Detection Methods

1. Suricata IDS Rules
2. Zeek Behavioral Analysis
3. Python Script (Custom)

## Conclusion

This lab provided hands-on experience with essential network security techniques, including multiple scanning methods (SYN, FIN, XMAS, NULL, ACK) and attacker evasion tactics such as slow scans and timing variations. You also practiced creating custom Suricata rules, scripting in Zeek for behavioral analysis, and automating detection workflows with Python.

Together, these exercises built a complete cycle of detection, analysis, and documentation. By professionally recording Indicators of Compromise (IOCs) and integrating automation, you strengthened both technical skills and operational efficiency—laying a solid foundation for real-world incident response and a portfolio that demonstrates readiness for SOC Analyst responsibilities.