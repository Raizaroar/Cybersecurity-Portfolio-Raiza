## PROJECT OVERVIEW

This is the capstone of Week 1: a comprehensive investigation of a multi-stage attack that combines ALL the skills I learned this week. I'll analyze a realistic scenario where an attacker compromised an organization using multiple attack vectors.

### Scenario:

**SecureCorp Inc.** experienced a security incident on January 22, 2025. As a SOC Analyst, you must investigate:

- Initial Access: ***How did the attacker get in?***
- Lateral Movement: ***Which systems did they compromise?***
- Data Exfiltration: ***What data did they steal?***
- Persistence: ***Did they leave backdoors?***
- Attribution: ***Who was the attacker?***

Attack Timeline:

1. 08:00 - Phishing email sent to employees
2. 09:30 - User opens malicious attachment
3. 10:00 - Malware executed, establishes C2
4. 11:00 - Internal network scan
5. 12:00 - Web application exploitation
6. 13:00 - Database access
7. 14:00 - Data exfiltration
8. 15:00 - Backdoor installed

## TOOLS USED – Week 1

| Tool                    | Purpose                        |
|-------------------------|--------------------------------|
| Windows Event Viewer (PowerShell) | System event inspection and audit logs |
| Wireshark               | Network traffic analysis       |
| VirusTotal              | Malware detection via hash/URL |
| MXToolbox               | Email header analysis          |
| grep / awk / Python     | Log parsing and automation     |

## OBJECTIVE

- Investigate multi-stage attack from initial access to exfiltration
- Correlate evidence across multiple data sources
- Create complete attack timeline
- Identify all compromised systems
- Extract comprehensive IOC list
- Map entire attack to MITRE ATT&CK
- Provide executive summary for management
- Recommend remediation actions
- Document professional incident report

## LAB ENVIRONMENT

- **Evidence Sources:**

- Email (.eml file)
- Malware sample (hash)
- Network traffic (PCAP file)
- Windows Event Logs
- Web server logs

- Analysis System: Kali Linux
- Time Allocated: 3-4 hours (realistic incident response time)

## PREREQUISITES

- ALL Week 1 labs completed (1.1.1 - 1.1.6) 
- All tools installed and tested


## PASO A PASO - LABORATORIO 1.1.7

- **STEP 1.1.7.1: Incident Briefing**

**Incident Report (from IT Department):**

[See Incident Report Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Incident-Report.md)

- MY MISSION:

Conduct full investigation and provide:
1. Attack timeline
2. Compromised systems list
3. IOCs for blocking
4. Remediation recommendations
5. Executive summary

## STEP 1.1.7.2: Evidence Collection

Evidence Package Contents:

***Create evidence folder***


| # | Evidence Name                     | Link |
|---|----------------------------------|------|
| 1 | Phishing Email                   | [View Evidence 1](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Evidence/Phishing-email.md) |
| 2 | Malware Hash                     | [View Evidence 2](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Evidence/Malware-Hash.md) |
| 3 | Windows Event Logs               | [View Evidence 3](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Evidence/Windows-Event-logs.md) |
| 4 | Network Traffic (PCAP summary)  | [View Evidence 4](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Evidence/Network-traffic-summary.md) |
| 5 | Web Server Logs                 | [View Evidence 5](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Evidence/Web-server-logs.md) |

![lab1.1.7](/00-SOC-Analyst-30Day-Journey/assets/screenshots/Lab-1.1.7-week1/Lab-1.1.7.png)

## STEP 1.1.7.3: Investigation Phase 1 - Initial Access

***Analyze Phishing Email:***

Questions to answer:

1. Is this legitimate or phishing?
2. What are the red flags?
3. Who sent it?
4. What was the payload?

[See Analysis Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Analysis/PhishingEmail-Analysis.md)

## STEP 1.1.7.4: Investigation Phase 2 - Execution & Persistence

***Analyze Windows Event Logs:***

[See Analysis Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Analysis/MalwareExecution-Analysis.md)

## STEP 1.1.7.5: Investigation Phase 3 - Discovery & Lateral Movement

***Analyze Network Scanning:***

[See Analysis Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Analysis/NetworkScanning-Analysis.md)

## STEP 1.1.7.6: Investigation Phase 4 - Web Application Exploitation

***Analyze Web Server Logs:***

[See Analysis Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Analysis/WebServerLogs-Analysis.md)

## STEP 1.1.7.7: Investigation Phase 5 - Data Exfiltration

***Analyze Database Access & Exfiltration:***

[See Analysis Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Analysis/DataExfiltration.md)

## STEP 1.1.7.8: Create Complete Attack Timeline

**COMPLETE ATTACK TIMELINE - INC-2025-001**

 **Kill Chain Phases**

[See Attack Timeline Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Documents/Attack-Timeline.md)

## STEP 1.1.7.9: Extract Complete IOC List

INDICATORS OF COMPROMISE - INC-2025-001

**Incident:** SecureCorp Data Breach

**Date:** January 22, 2025

**Analyst:** Raiza

**Confidence:** HIGH

[See IOC list Here](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/blob/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation/Documents/IOC-list.md)

## Conclusion
Successfully completed comprehensive incident investigation demonstrating:

- End-to-end attack reconstruction
- Professional forensic methodology
- Business impact understanding
-Strategic security recommendations

This capstone integrates ALL Week 1 skills into single cohesive investigation, proving job-readiness for SOC Analyst / Incident Responder roles.