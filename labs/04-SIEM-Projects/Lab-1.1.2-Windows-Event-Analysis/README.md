# Lab 1.1.2: Windows Security Event Log Analysis

**Status:** Completed 
**Category:** SIEM Projects  
**Difficulty:** Entry Level  
**Estimated Time:** 2-3 hours  

---

## Executive Summary

### Incident Overview

A successful brute force attack was detected against the Administrator account originating from internal IP address 192.168.1.100 (DESKTOP-ATTACKER). The attack consisted of 5 failed authentication attempts over a 3-minute window, followed by a successful login approximately 44 minutes later.

---

## Objectives

- Analyze Windows Security Event Logs
- Identify failed login patterns (Event ID 4625)
- Detect potential brute force attacks
- Document findings in MITRE ATT&CK format
- Create actionable recommendations

---

## Tools Required

- PowerShell 7
- Sample Windows Security logs
- CSV viewer
- VS Code for documentation

---

## Step-by-Step Analysis

- Step 1: Environment Verification
- Step 2: Sample Data Preparation
- Step 3: Initial Event Filtering
- Step 4: Source IP Analysis (Pattern Detection)
- Step 5: Temporal Analysis (Attack Velocity)
- Step 6: Account Targeting Analysis
- Step 7: Success After Failure Correlation
- Step 8: MITRE ATT&CK Mapping
- Step 9: Incident Report Generation



## PowerShell Commands – Reference Guide

| **Command**                                               | **Purpose**                                      |
|-----------------------------------------------------------|--------------------------------------------------|
| `$PSVersionTable.PSVersion`                               | Check PowerShell version                         |
| `$logs = Import-Csv "filename.csv"`                       | Import CSV data                                  |
| `$filtered = $logs \| Where-Object {$_.Id -eq 4625}`       | Filter logs by Event ID                          |
| `$filtered.Count`                                          | Count filtered events                            |
| `$logs \| Group-Object FieldName \| Select-Object Name, Count` | Group and count by field                         |
| `... \| Sort-Object Count -Descending`                    | Sort results in descending order                 |
| `... \| Format-Table -AutoSize`                           | Format output as a table                         |
| `[DateTime]$stringDate`                                   | Convert string to DateTime                       |
| `$data \| Out-File -FilePath "report.txt" -Encoding UTF8` | Export data to a text file                       |
| `Get-WinEvent -LogName Security -MaxEvents 500`           | Extract real-world logs (requires admin rights)  |
| `Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4625}` | Filter logs by name and ID (admin required)      |

## Key Event IDs - Reference

| Event ID | Description | Security Significance |
|----------|-------------|----------------------|
| **4624** | Successful Login | Baseline for normal access, critical when following failures |
| **4625** | Failed Login | Primary brute force indicator, password guessing |
| **4634** | Login off | Session end, useful for lateral movement detection |
| **4648** | Login with Explicit Credentials | Credential theft, pass-the-hash indicators |
| **4672** | Special Privileges Assigned | Privileged account usage, escalation detection |
| **4720** | User Account Created | Persistence mechanism, backdoor accounts |
| **4732** | Member Added to Security Group | Privilege escalation, persistence |



### Key Findings

- **Attack Type:** Brute Force (Password Guessing)
- **Target:** Administrator account (highest privilege level)
- **Source:** 192.168.1.100 (internal network)
- **Failed Attempts:** 5 consecutive failures
- **Attack Duration:** 3 minutes (08:15:23 - 08:16:18)
- **Compromise Confirmed:** Successful login at 09:00:12
- **Severity:**  HIGH

### Impact Assessment

- **Confidentiality:** HIGH - Full system access gained
- **Integrity:** HIGH - Ability to modify system configurations
- **Availability:** MEDIUM - Potential for system disruption or ransomware

### MITRE ATT&CK Mapping

- **Tactic:** Credential Access
- **Technique:** T1110 - Brute Force
- **Sub-Technique:** T1110.001 - Password Guessing
- **Platform:** Windows

## What I Learned

1. How to read Windows Event Logs
2. Identifying suspicious authentication patterns
3. Correlating events for threat detection
4. Professional documentation of security findings

## Conclusion

Successfully detected, analyzed, and documented a brute force attack against privileged accounts using Windows Security Event Logs. This investigation demonstrated:

**Technical Competency:**

- PowerShell scripting for log analysis
- Pattern recognition in security events
- Temporal and statistical correlation techniques
- MITRE ATT&CK framework application

**SOC Analyst Skills:**

- Triage and prioritization (severity assessment)
- Incident documentation (executive + technical)
- Actionable recommendations (immediate + strategic)
- Multi-stakeholder communication

**Real-World Readiness:**

- Followed industry-standard investigation methodology
- Produced audit-ready documentation
- Mapped to compliance frameworks (ATT&CK)
- Provided remediation roadmap

### Incident Disposition

- **Status:**  Investigation Complete
- **Severity:** HIGH - Credentials compromised
- **Evidence:** 5 failed attempts + 1 successful login correlated
- **Next Steps:** Escalated to Incident Response team for containment


**Next Lab:** Day 3 - Wireshark Network Analysis