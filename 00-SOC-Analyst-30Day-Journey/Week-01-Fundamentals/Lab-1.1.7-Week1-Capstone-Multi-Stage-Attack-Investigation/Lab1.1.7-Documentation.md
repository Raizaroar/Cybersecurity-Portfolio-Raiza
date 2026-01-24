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

