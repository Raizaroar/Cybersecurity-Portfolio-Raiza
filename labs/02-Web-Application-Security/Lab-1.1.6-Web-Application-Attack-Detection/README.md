# 1.1.6 - Web Application Attack Detection  


**Date:** 21-01-2026 
**Analyst:** Raiza  
**Status:** Completed  
**Category:** Web Application Security  
**Difficulty:** Intermediate  
**Time Invested:** 4 hours  


## Executive Summary

Successfully detected 11 web attack attempts across 4 attack vectors (SQL injection, XSS, path traversal, command injection) plus 1 brute force campaign.
 Analysis revealed HTTP 200 responses for most attacks, indicating successful exploitation. Created Python automation script achieving 100% detection rate on test dataset.

 ### Attack Summary

| Attack Type | Attempts | Success Rate | Severity |
|-------------|----------|--------------|----------|
| SQL Injection | 2 | 100% (2/2 HTTP 200) | CRITICAL |
| XSS | 2 | 100% (2/2 HTTP 200) | HIGH |
| Path Traversal | 2 | 50% (1/2 HTTP 200) | HIGH |
| Command Injection | 2 | 100% (2/2 HTTP 200) | CRITICAL |
| Brute Force | 5 attempts | 20% (1/5 success) | MEDIUM |

## Objectives

-  Understand OWASP Top 10 web vulnerabilities
-  Generate web attack traffic in controlled environment
-  Analyze Apache access logs for attack patterns
-  Detect SQL injection attempts
-  Identify XSS (Cross-Site Scripting) attacks
-  Recognize path traversal attacks
-  Spot command injection attempts
-  Identify brute force login patterns
-  Use grep/awk for log analysis
-  Create Python automation script
-  Extract attacker IPs and payloads

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| **Apache** | 2.1 | Web server (log source) |
| **DVWA** | Latest | Vulnerable web app (attack target) |
| **grep/awk/sed** | GNU | Log parsing and filtering |
| **Python 3** | 3.3| Automated log analysis script |
| **Kali Linux** | 2025.3 | Attack generation platform |
| **Metasploitable 2** | N/A | Vulnerable server environment |

## Step-by-Step Implementation

   - Step 1: OWASP Top 10 Education
   - Step 2: Attack Traffic Generation
   - Step 3: Apache Log Collection
   - Step 4: Manual Log Analysis with grep
   - Step 5: Python Automation Script


## Attack Patterns Identified

### SQL Injection Indicators

***Pattern: ' OR 1=1--***

Purpose: Authentication bypass.

Detection: Single quotes + OR + comment (--

Impact: Full database access

***Pattern: ' UNION SELECT '***

Purpose: Data exfiltration

Detection: UNION + SELECT keywords

Impact: Extract sensitive data


### XSS Indicators

***Pattern: <script>alert()</script>***

Purpose: JavaScript execution

Detection: <script> tags

Impact: Session hijacking, credential theft

***Pattern: <img onerror=>***

Purpose: Event-handler XSS

Detection: onerror/onload attributes

Impact: Same as above, bypasses some filters

---

## Lessons Learned

### Technical Skills

1. **Log analysis is forensics:** Every attack leaves traces
2. **Patterns are key:** Regex skills are essential
3. **Automation saves time:** Python > manual grep for complex analysis
4. **Context matters:** HTTP 200 + SQLi payload = successful attack

### Security Insights

1. **Web apps are fragile:** DVWA showed how easy attacks are
2. **Defense in depth needed:** Logs alone don't prevent attacks
3. **Speed matters:** Attackers move fast (5-second brute force)
4. **Attribution via IP:** Critical for blocking/investigation

### SOC Analyst Mindset

1. **Proactive hunting:** Don't wait for alerts, search logs
2. **Pattern recognition:** Train eyes to spot anomalies
3. **Tool creation:** Build scripts for repetitive tasks
4. **Documentation:** Logs = evidence in incident response
