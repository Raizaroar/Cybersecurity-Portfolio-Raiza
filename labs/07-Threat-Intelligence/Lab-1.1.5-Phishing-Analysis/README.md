# 1.1.5: Phishing Email Analysis and Detection

**Date:** 20-01-2025  

**Status:** Completed  

**Category:**  Threat Intelligence  

**Difficulty:** Entry Level - Intermadiate  

**Time Invested:** 3 hours

## Executive Summary

Executive Summary
Successfully analyzed 2 phishing email campaigns, identifying credential harvesting and malware delivery attempts. Demonstrated proficiency in email header analysis, authentication verification, and safe URL/attachment inspection. Extracted 15+ IOCs for blocking and created comprehensive detection checklist for ongoing operations.

## Objective

- Understand email structure (headers, body, attachments)
- Extract and analyze email headers
- Verify email authentication (SPF, DKIM, DMARC)
- Identify brand impersonation and typosquatting
- Analyze malicious URLs without clicking
- Inspect email attachments safely
- Recognize social engineering tactics
- Extract actionable IOCs
- Document findings professionally

## Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| **Thunderbird** | Latest | Email viewer (.eml files) |
| **MXToolbox** | Web-based | Email header analysis |
| **URLScan.io** | Web-based | Safe URL inspection |
| **VirusTotal** | API v3 | Attachment hash checking |
| **AbuseIPDB** | Web-based | IP reputation checking |
| **PhishTool** | Community | Automated phishing analysis |

## Step-by-Step Implementation

- Step 1: Email Anatomy Education
- Step 2: Analyze Campaign #1 - Microsoft Phishing
- Step 3: Analyze Campaign #2 - Malicious Attachment
- Step 4: Common Phishing Techniques Catalog
- Step 5: MXToolbox Header Analysis Automation
- Step 6: Phishing Detection Checklist Creation

Phishing Techniques Identified

| Technique | Sample #1 | Sample #2 |
|-----------|-----------|-----------|
| Brand Impersonation | -Y Microsoft | -N |
| Typosquatting | - Y microsoft-security.com | -N |
| Malicious Link | -Y | -N |
| Malicious Attachment | -N | -Y |
| Urgency Tactics | -Y | -W (subtle) |
| Fear Tactics | -Y | -N |
| SPF Failure | -Y | -N |
| Compromised Account | -N | -Y |

| means | -N  (not) | -Y (yes) | -W (warning)

## Key Findings

**Campaign 1: Microsoft Brand Impersonation**

- Type: Credential Harvesting
- Target: Microsoft account credentials
- Method: Fake login page
- Authentication: SPF/DKIM/DMARC all FAIL
- Verdict: High-confidence phishing

**Campaign 2: Invoice Malware**

- Type: Malware Delivery
- Vector: Malicious attachment (.pdf.exe)
- Payload: Banking Trojan
- Detection: 52/73 on VirusTotal
- Verdict: Compromised legitimate account

## Lessons Learned

### Technical Skills

1. **Email headers tell the truth:** Body can lie, headers rarely do
2. **Authentication is necessary but not sufficient:** SPF/DKIM/DMARC help but aren't perfect
3. **Double extensions are red flags:** .pdf.exe, .doc.scr always suspicious
4. **IP reputation matters:** Originating IP often reveals attack origin

### Security Insights

1. **Phishing is social engineering:** Technical controls can't stop everything
2. **User education is critical:** Best firewall is informed user
3. **Attackers evolve:** Modern phishing is professional, well-written
4. **Compromised accounts are dangerous:** Passing authentication means nothing if account is compromised

### SOC Analyst Mindset

1. **Trust but verify:** Even legitimate-looking emails need validation
2. **Context is key:** Unexpected email from known sender? Verify via phone
3. **Document everything:** Today's phishing is tomorrow's threat intel
4. **Share IOCs:** Help protect wider community

---

## Conclusion

Successfully completed comprehensive phishing email analysis training, demonstrating ability to identify, analyze, and respond to the #1 attack vector in cybersecurity. Analyzed 2 distinct phishing campaigns (credential harvesting and malware delivery), extracted actionable IOCs, and created reusable detection checklist for operational use.


**Next lab 1.1.6 Web application attacks (SQL injection, XSS)**