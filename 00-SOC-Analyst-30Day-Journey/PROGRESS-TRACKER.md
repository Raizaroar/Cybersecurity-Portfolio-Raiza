#  SOC Analyst 30-Day Intensive Training Journey

- **Start Date:** 16-01-2026 

- **Target Completion:** pending  

- **Current Status:**  Day 9 - Lab 2.1.2 - Advanced Splunk & Threat Hunting  
---

##  Progress Overview

| Week | Focus Area | Labs Completed | Status |
|------|-----------|----------------|--------|
| Week 1 | Fundamentals & Reconnaissance | 7/7 |  Completed |
| Week 2 | Threat Detection | 1/7 |  Pending |
| Week 3 | Incident Response | 0/7 |  Pending |
| Week 4 | Advanced Cases & Portfolio | 0/9 |  Pending |

**Overall Progress:** 8/30 Labs (26.6%)

---

##  Daily Log

### Day 1 - 16-01-2026
**Lab:** 1.1.1 - Environment Setup  
**Status:** Completed  
**Time Invested:** 2 hours  
**Key Learnings:**
- Integrated training plan with existing portfolio structure
- Configured professional Git workflow
- Created progress tracking system

**Challenges:**
- None

**Portfolio Updates:**
- Created `00-SOC-Analyst-30Day-Journey` folder
- Established tracking mechanism

---

### Day 2 - 17-01-2026
**Lab:** 1.1.2 - Windows Security Event Log Analysis  

**Status:**  Completed

**Time Invested:** 2 hours

**Location** `04-SIEM-Projects` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/labs/04-SIEM-Projects/Lab-1.1.2-Windows-Event-Analysis)


**Planned Activities:**
- Analyze failed login attempts (Event ID 4625)
- Detect brute force patterns


**Key Learnings:**
- Analyzed Windows Event ID 4625 (Failed Logon) for brute force detection
- Implemented temporal analysis to detect attack velocity
- Correlated failed attempts with successful logins (compromise detection)
- Mapped findings to MITRE ATT&CK T1110.001
- Created professional incident report with executive summary

**Challenges:**

***Permission and Access Control Issues***

- **Challenge**: Encountered `UnauthorizedAccessException` when attempting to read Windows Security event logs

- **Root Cause**: PowerShell terminal running without elevated privileges

- **Resolution**: Executed VS Code as Administrator to gain necessary permissions for accessing Security event logs

- **Learning**: Understanding Windows security contexts and the importance of proper privilege escalation for security analysis tasks


**Skills Developed:**
- PowerShell log analysis
- Pattern recognition in security events
- Incident documentation
- MITRE ATT&CK framework


### Day 3 - 18 - 01- 2026

**Lab:** 1.1.3 - Network Traffic Analysis with Wireshark  

**Status:**  Completed 

**Time Invested:** 2 hours

**Environment:** Kali Linux + Metasploitable lab setup

**Location:** `01-Network-Security` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/labs/01-Network-Security/wireshark-analysis/Lab-1.1.3-Wireshark-Analysis)

**Planned Activities:**

- Capture and analyze network packets
- Identify suspicious traffic patterns
- Detect common network attacks (ARP spoofing, port scanning)

**Key Learnings:**

- Installed and configured Wireshark with Npcap
- Captured and analyzed live network traffic (ICMP, DNS, HTTP, TLS)
- Mastered display filters for traffic isolation
- Detected port scanning patterns using TCP flag analysis
- Followed TCP streams to reconstruct communications
- Established network baseline for anomaly detection

**Challenges:**

**Challenge 1:** ***No Network Interfaces Visible***

- Issue: Wireshark showed no interfaces on first launch
- Root Cause: Npcap not installed or service not running
- Solution: Reinstalled Npcap with "WinPcap API-compatible mode"
- Lesson: Packet capture requires kernel-level drivers

**Challenge 2:** ***Capture Permission Denied (Linux)***

- Issue: "You don't have permission to capture on that device"
- Root Cause: User not in wireshark group
- Solution: sudo usermod -a -G wireshark $USER + logout/login
- Lesson: Non-root capture requires proper group membership

**Skills Developed:**

-  Packet capture and analysis
-  Display filter creation (15+ filters)
-  Protocol analysis (OSI layers 2-7)
-  Attack pattern recognition
-  Network baseline establishment


---

### Day 4 - 19-01-2026

**Lab:** 1.1.4 - Malware Detection with VirusTotal  

**Status:** Completed  

**Time Invested:** 3 hours 

**Location:**`03-Penetrarion-Testing` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/labs/03-Penetration-Testing/Lab-1.1.4-Malware-Detection#lab-114-malware-detection-with-virustotal-and-hash-analysis)

**Key Learnings:**

- Mastered cryptographic hashing (MD5, SHA1, SHA256)
- Configured VirusTotal account with API access
- Analyzed 2 malware families using hash-based detection
- Extracted actionable IOCs from VirusTotal reports
- Created Python script for batch hash analysis
- Documented findings professionally with MITRE ATT&CK mapping

**Challenges:**

**Challenge 1:** ***API Rate Limiting***

**Issue:** Hit 500 requests/day limit during testing  
**Root Cause:** Repeated queries while debugging script  
**Solution:** Implemented 15-second delays between requests  
**Lesson:** Always respect API limits; implement caching for repeated queries

**Challenge 2:** ***Contradictory Vendor Results***

**Issue:** File flagged by 30 engines, cleared by 40  
**Root Cause:** Heuristic vs. signature-based detection differences  
**Solution:** Weighted analysis (trust reputable vendors more)  
**Lesson:** Detection ratio is a guide, not absolute truth

**Skills Developed:**
- Cryptographic hashing
- Threat intelligence gathering
- VirusTotal platform mastery
- Python automation
- IOC extraction and documentation

### Day 5 - 20-01-2026

**Lab:** 1.1.5 - Phishing Email Analysis  
**Status:**  Completed  
**Time Invested:** 3 hours  
**Location:** `07-Threat-Intelligence`  [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/labs/07-Threat-Intelligence/Lab-1.1.5-Phishing-Analysis)

**Key Learnings:**
- Analyzed email headers for authentication (SPF, DKIM, DMARC)
- Identified 2 phishing campaigns (credential harvesting + malware)
- Used MXToolbox, URLScan.io, AbuseIPDB for analysis
- Created comprehensive phishing detection checklist
- Extracted 15+ IOCs for blocking
- Understood compromised account vs. spoofed email

**Challenges:**

**Challenge 1:** ***SPF/DKIM/DMARC All Pass, Still Malicious***

**Issue:** Sample #2 passed all authentication but was malicious  
**Root Cause:** Legitimate account compromised  
**Learning:** Authentication verifies sender identity, not intent  
**Lesson:** Always inspect attachments/links regardless of authentication

**Challenge 2:** ***URL Shortener Hides Destination***

**Issue:** bit.ly link destination unknown without clicking  
**Root Cause:** URL shortening service obfuscates real URL  
**Solution:** Use URLScan.io or curl redirect checking  
**Lesson:** Never click shortened links in suspicious emails

**Skills Developed:**
- Email security (SPF/DKIM/DMARC)
- Header analysis
- Safe URL/attachment inspection
- Social engineering recognition
- IOC extraction


### Day 6 - 21-01- 2026
**Lab:** 1.1.6 - Web Application Attack Detection  
**Status:**  Completed  
**Time Invested:** 4 hours  
**Location:** `02-Web-Application-Security` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)]( https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/labs/02-Web-Application-Security/Lab-1.1.6-Web-Application-Attack-Detection)

**Key Learnings:**

- Analyzed Apache web server logs for attack patterns
- Detected 11 attacks: SQLi, XSS, path traversal, command injection, brute force
- Used grep/awk/sed for log parsing and filtering
- Created Python automation script (150+ lines, 100% detection rate)
- Extracted IOCs from successful exploitation attempts
- Mapped findings to MITRE ATT&CK (T1190, T1059.004, T1110.001)

**Challenges:**

**Challenge 1:** ***URL Encoding in Logs***

**Issue:** Some attacks URL-encoded (< becomes %3C)  
**Root Cause:** HTTP standard encoding  
**Solution:** Added URL decode patterns to grep/Python  
**Lesson:** Always check for encoded variants

**Challenge 2:** ***Large Log Files***

**Issue:** 10GB+ logs in production  
**Root Cause:** High-traffic websites  
**Solution:** Used grep before Python (pre-filtering)  
**Lesson:** Multi-stage processing for efficiency

**Skills Developed:**

- OWASP Top 10 knowledge
- Apache log analysis
- grep/awk/Python for log parsing
- Web attack pattern recognition
- Python automation for security
---

### Day 7 - 22-01-2026

**Lab:** 1.1.7 - Week 1 Capstone: Multi-Stage Attack Investigation  
**Status:**  Completed  
**Time Invested:** 4 Hours 
**Location:** `00-SOC-Analyst-30Day-Journey/Week-01-Capstone/` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)]( https://github.com/Raizaroar/Cybersecurity-Portafolio-Raiza/tree/main/00-SOC-Analyst-30Day-Journey/Week-01-Fundamentals/Lab-1.1.7-Week1-Capstone-Multi-Stage-Attack-Investigation)


**Key Learnings:**

- Conducted end-to-end incident investigation (phishing → exfiltration)
- Analyzed 5 evidence sources (email, malware, Windows logs, network, web)
- Reconstructed complete 6.5-hour attack timeline
- Identified 3 compromised systems, 45K stolen records
- Extracted 12 actionable IOCs
- Mapped to 17 MITRE ATT&CK techniques across 11 tactics

**Skills Integrated:**
-  Email security (Lab 1.1.5)
-  Malware analysis (Lab 1.1.4)
-  Windows forensics (Lab 1.1.2)
-  Network analysis (Lab 1.1.3)
-  Web security (Lab 1.1.6)

**Deliverables:**
- Complete incident report
- Executive summary
- IOC list

### Day 8 - 23-01-2026
**Lab:** 2.1.1 - Splunk SIEM Fundamentals  
**Status:** Completed  
**Time Invested:** 6 hours 

**Location:** `04-SIEM-Projects/Lab-2.1.1-Splunk-Basics/` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portfolio-Raiza/tree/main/labs/04-SIEM-Projects/Lab-2.1.1-Splunk-Basics)


**Key Learnings:**
- Installed Splunk Enterprise (10.2.0) with 60-day trial
- Indexed 3 data sources (Windows, web, firewall logs - 40 events total)
- Mastered SPL basics (10+ searches created)
- Created 7 detection use cases (60% MITRE coverage)
- Configured 3 real-time alerts (brute force, SQLi, C2)
- Built SOC dashboard with 5 visualization panels
- Documented professional detection engineering

**Challenges**

**Challenge 1:** ***Time Zone Parsing***

**Issue:** Timestamps not parsing correctly  
**Resolution:** Adjusted time settings in source type configuration  
**Lesson:** Always verify timestamp extraction

 **Challenge 2:** ***Dashboard Not Updating***

**Issue:** Real-time panels showed stale data  
**Resolution:** Changed search mode from "Fast" to "Smart"  
**Lesson:** Fast mode trades accuracy for speed

**Skills Developed:**git 

- SIEM operations
- SPL query language
- Detection engineering
- Alert configuration
- Dashboard creation

### Day 9 - 24-01-2026

**Lab:** 2.1.2 - Advanced Splunk & Threat Hunting  
**Status:** Completed  
**Time Invested:** 5 hours
**Location:** `04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk/` [![Abrir Proyecto](https://img.shields.io/badge/GitHub-Proyecto-blue?logo=github)](https://github.com/Raizaroar/Cybersecurity-Portfolio-Raiza/tree/main/labs/04-SIEM-Projects/Lab-2.1.2-Advanced-Splunk)

**Key Learnings:**
- Mastered 5 advanced SPL techniques (regex, eval, macros, transactions, subsearches)
- Created 3 multi-log correlation detections
- Implemented 2 Sigma rules with SPL conversion
- Conducted hypothesis-driven threat hunt (5 hypotheses)
- Detected 1 real malicious macro attack
- Built threat hunting dashboard (5 panels)
- Created detection engineering playbook

**Advanced SPL:**
- regex to pattern extraction
- eval to field calculations, case statements
- macros to reusable search fragments
- transaction to event grouping
- advanced subsearches and joins

**Skills Developed:**
- Advanced SIEM operations
- Detection engineering
- Threat hunting methodology
- Sigma rule implementation
- Professional hunt documentation



##  Skills Development Tracker

| Skill Category | Day 1 | Day 7 | Day 14 | Day 21 | Day 30 |
|---------------|-------|-------|--------|--------|--------|
| Log Analysis | 🟢 | ⬜ | ⬜ | ⬜ | ⬜ |
| Network Traffic Analysis | 🟢 | ⬜ | ⬜ | ⬜ | ⬜ |
| Malware Analysis | 🟢 | ⬜ | ⬜ | ⬜ | ⬜ |
| Incident Response | 🟢 | ⬜ | ⬜ | ⬜ | ⬜ |
| Threat Intelligence | 🟢 | ⬜ | ⬜ | ⬜ | ⬜ |
| Automation/Scripting | ⬜ | ⬜ | ⬜ | ⬜ | ⬜ |

**Legend:**  
⬜ Not Started | 🟡 Learning | 🟢 Competent | 🔵 Proficient

---

##  Certification Readiness

- [ ] CompTIA Security+ concepts covered
- [ ] CySA+ practical skills demonstrated
- [ ] Blue Team fundamentals mastered
- [ ] SOC Analyst interview-ready

---

## Resources Utilized

- [ ] MITRE ATT&CK Framework
- [ ] NIST Cybersecurity Framework
- [ ] Cyber Kill Chain
- [ ] SANS Reading Room
- [ ] Wireshark Documentation
- [ ] PowerShell for Security

---

##  Achievements Unlocked

- [/\] Day 1: Environment configured like a professional SOC
- [/\] Day 7: First week milestone - Fundamentals mastered
- [ ] Day 14: Mid-point - Detection capabilities demonstrated
- [ ] Day 21: Week 3 - Incident response skills proven
- [ ] Day 30: COMPLETE - Ready for SOC Analyst roles

---

**What makes this training valuable:**
- Hands-on, practical labs 
- Documented evidence of every skill
- Industry-standard tools and frameworks
- Real-world scenarios and case studies
- Professional documentation standards

**Portfolio Structure Benefits:**
- Organized by cybersecurity domain
- Each lab includes executive summaries
- Screenshots and evidence included 
- GitHub activity shows consistent learning

---

**Last Updated:** [19-01-2026]



> [!NOTE]
> This roadmap has been developed with guidance from Claude, an AI assistant by Anthropic.